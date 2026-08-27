---
title: "Azure AI Translator y Blob Storage; traducir documentos completos por API con Postman"
date: 2026-08-26 10:00:00 +0200
categories: [Azure, Integración]
tags: [azure, blob-storage, ai-translator, postman, rest-api, sas]
description: Cómo montar un flujo REST con Azure Blob Storage y Azure AI Translator para subir, listar, traducir y eliminar documentos completos conservando su formato; permisos SAS mínimos y validación paso a paso con Postman.
image:
  path: 01-sas-origen-acceder-tokens.png
  alt: Acceso a la opción Tokens de acceso compartido de un contenedor de Azure Blob Storage
media_subpath: /assets/img/posts/azure-ai-translator-blob-storage-postman/
---

La semana pasada, un compañero me planteó una necesidad muy concreta en un proyecto interno: traducir documentos completos —manuales, fichas técnicas, contratos— a otro idioma sin perder su formato original ni depender de copiar y pegar texto en herramientas de traducción sueltas.

A partir de esa necesidad diseñé un flujo práctico con Azure Blob Storage y Azure AI Translator: subir el documento a un contenedor de origen, lanzar la traducción por lotes desde la API REST y recoger el resultado ya traducido en un contenedor de destino. Antes de integrarlo en la aplicación, validé cada paso del flujo con Postman.

## El problema real

En muchos proyectos, traducir documentación no es una tarea puntual: crece con cada idioma nuevo, cada actualización de contenido y cada cliente internacional. Hacerlo a mano —copiar texto, pegarlo en un traductor, recomponer el documento— es lento y termina rompiendo el formato original.

El objetivo era resolver justo eso:

- entrada simple: un documento depositado en un contenedor de origen
- salida útil: el mismo documento traducido, con su estructura intacta, en un contenedor de destino
- posibilidad de automatizar el flujo completo por API, sin pasar por el portal de Azure

## Enfoque de solución

Antes de montar nada, revisé las opciones que ofrece el servicio de traducción de documentos de Azure Translator y valoré las dos: la [traducción sincrónica de un solo archivo](https://learn.microsoft.com/es-es/azure/ai-services/translator/document-translation/latest/quickstarts/synchronous) y la [traducción por lotes asíncrona](https://learn.microsoft.com/es-es/azure/ai-services/translator/document-translation/latest/quickstarts/asynchronous).

La síncrona tiene la ventaja de que no requiere alojamiento: envías el documento directamente y recibes la traducción en la propia respuesta, sin pasar por Blob Storage. Como desventaja principal, admite menos formatos de fichero que la vía asíncrona; en concreto, no traduce PDF, que es precisamente el formato que más íbamos a traducir. La asíncrona, en cambio, traduce varios documentos o archivos grandes en paralelo —conservando la estructura original, con soporte de todos los [idiomas y dialectos admitidos](https://learn.microsoft.com/es-es/azure/ai-services/translator/language-support) y compatibilidad con PDF—, pero exige una cuenta de Blob Storage con contenedores de origen y destino, y sondear el estado del trabajo hasta que termina.

Para mi caso —principalmente PDF, y sin descartar otros formatos por el camino— la asíncrona era la única opción viable, así que fue la que desarrollé en este artículo. Si tu escenario es traducir un único documento suelto que no sea PDF, revisa antes la [traducción sincrónica](https://learn.microsoft.com/es-es/azure/ai-services/translator/document-translation/latest/quickstarts/synchronous) para confirmar que de verdad necesitas montar toda esta infraestructura.

El flujo que validé con Postman fue este:

1. Preparar los contenedores de origen y destino en Blob Storage
2. Generar tokens SAS con los permisos mínimos necesarios para cada contenedor
3. Subir el documento al contenedor de origen
4. Lanzar el trabajo de traducción por lotes en Azure AI Translator
5. Consultar el estado del trabajo hasta que finaliza
6. Eliminar los documentos de prueba cuando ya no son necesarios

```text
Documento
  ↓
Blob Storage (origen)
  ↓
Azure AI Translator
  ↓
Blob Storage (destino)
  ↓
Documento traducido
```

> Documentación oficial de Microsoft: [¿Qué es la traducción de documentos de Azure Translator?](https://learn.microsoft.com/es-es/azure/ai-services/translator/document-translation/latest/overview?tabs=async).
{: .prompt-info }

## Preparar los contenedores

En la cuenta de almacenamiento necesitas dos contenedores: uno para los documentos pendientes de traducir y otro para los ya traducidos. En mi prueba los llamé `simad-origen` y `simad-destino`. El primero recibe los documentos originales; el segundo, los resultados de la traducción.

Todas las operaciones de subida, listado y eliminación que usamos a continuación (`Put Blob`, `List Blobs`, `Delete Blob`...) forman parte de la [API REST de Azure Blob Storage](https://learn.microsoft.com/es-es/rest/api/storageservices/blob-service-rest-api).

## Generar los tokens SAS

> Ojo con las fechas de vigencia: al cambiar los permisos, el portal resetea la fecha de inicio y caducidad a los valores por defecto. Si no te fijas y das por buena la caducidad que habías puesto antes, el token puede quedarte válido solo unas horas.
{: .prompt-warning }

### Contenedor de origen

Para traducir documentos, el contenedor de origen necesita permisos de **lectura y listado**:

```text
sp=rl
```

![Acceso a la opción Tokens de acceso compartido del contenedor de origen](01-sas-origen-acceder-tokens.png){: w="1912" h="827" .shadow }
*Acceso a la opción Tokens de acceso compartido del contenedor de origen.*

![Configuración de permisos y periodo de validez del token SAS](02-sas-origen-permisos-caducidad.png){: w="1903" h="827" .shadow }
*Configuración de permisos y periodo de validez del token SAS.*

![Generación del token y la URL SAS desde Azure Portal](03-sas-origen-generar-token.png){: w="1903" h="827" .shadow }
*Generación del token y la URL SAS desde Azure Portal.*

### Contenedor de destino

El contenedor de destino necesita permisos de **escritura** adecuados para que Azure AI Translator pueda depositar ahí el documento traducido:

```text
sp=rw
```

![Token y URL de SAS ya generados en el portal, con los valores ocultos](04-sas-destino-token-generado.png){: w="1915" h="890" .shadow }
*Token y URL de SAS ya generados en el portal (valores ocultos por seguridad).*

> No publiques tokens SAS ni claves de acceso en artículos ni capturas. Limita siempre permisos y caducidad, y regenera cualquier secreto que haya quedado expuesto.
{: .prompt-warning }

## Subir un documento desde Postman

Para cargar un archivo en el contenedor de origen se utiliza `PUT Blob`.

### Petición

```http
PUT https://<cuenta>.blob.core.windows.net/simad-origen/manual.pdf?<SAS>
```

### Cabeceras

```text
x-ms-blob-type: BlockBlob
x-ms-version: <versión compatible>
Content-Type: application/pdf
```

### Cuerpo

En Postman, selecciona **Body > binary** y elige el archivo.

![Carga de un documento al contenedor de origen mediante Postman](05-postman-put-blob-origen.png){: w="1409" h="607" .shadow }
*Carga de un documento al contenedor de origen mediante Postman.*

Una carga correcta devuelve:

```text
HTTP/1.1 201 Created
```

## Listar documentos del contenedor

`List Blobs` devuelve los blobs del contenedor.

### Petición

```http
GET https://<cuenta>.blob.core.windows.net/simad-origen?restype=container&comp=list&<SAS>
```

El SAS debe incluir permiso de **List** (`l`). La respuesta es XML e incluye los nombres y propiedades de los blobs. Si hay más resultados, `NextMarker` permite solicitar la página siguiente.

### Filtro opcional

```http
GET https://<cuenta>.blob.core.windows.net/simad-origen?restype=container&comp=list&prefix=facturas/&<SAS>
```

El parámetro `prefix` limita la respuesta a los nombres que comienzan por el valor indicado.

## Eliminar un documento desde Postman

`Delete Blob` elimina el documento indicado por su nombre exacto.

### Petición

```http
DELETE https://<cuenta>.blob.core.windows.net/simad-origen/manual.pdf?<SAS>
```

### Cabeceras

```text
x-ms-version: <versión compatible>
```

El SAS debe incluir permiso de **Delete** (`d`). Una eliminación aceptada devuelve:

```text
HTTP/1.1 202 Accepted
```

### Blobs con instantáneas

Si el blob tiene instantáneas, añade la cabecera siguiente para eliminar el blob base y todas sus instantáneas:

```text
x-ms-delete-snapshots: include
```

> Prueba `DELETE` únicamente con documentos de prueba. La eliminación temporal, si está habilitada en la cuenta, puede permitir recuperar el blob durante el periodo de retención configurado, pero no des por hecho que está activa.
{: .prompt-danger }

## Lanzar la traducción

```http
POST https://<endpoint-translator>/translator/text/batch/v1.1/batches
```

### Cabeceras

```text
Ocp-Apim-Subscription-Key: <clave-del-servicio>
Content-Type: application/json
```

### Cuerpo JSON

```json
{
  "inputs": [{
    "source": {
      "sourceUrl": "https://<cuenta>.blob.core.windows.net/simad-origen?<SAS>"
    },
    "targets": [{
      "targetUrl": "https://<cuenta>.blob.core.windows.net/simad-destino?<SAS>",
      "language": "en"
    }]
  }]
}
```

La respuesta incluye el identificador del trabajo y, inicialmente, puede mostrar el estado `NotStarted`.

> En mi prueba, meter varios idiomas en la misma petición (varios `targets` o varios `inputs`) no me funcionó. Terminé lanzando una petición independiente por idioma: una con `"language": "en"` y otra con `"language": "fr"`, cada una como un trabajo de traducción separado.
>
> Revisando después con más calma la [documentación de referencia de Start batch translation](https://learn.microsoft.com/es-es/azure/ai-services/translator/document-translation/reference/start-batch-translation), vi cómo se hace correctamente. Aun así, para mi caso particular me sigue resultando mejor lanzar una traducción por idioma: así controlo mejor el resultado de cada una por separado.
{: .prompt-tip }

El resto de operaciones disponibles —cancelar un trabajo, la traducción síncrona de un solo archivo...— están recogidas en la [guía de la API REST de traducción de documentos](https://learn.microsoft.com/es-es/azure/ai-services/translator/document-translation/latest/rest-api/guide-overview).

## Consultar el estado

```http
GET https://<endpoint-translator>/translator/text/batch/v1.1/batches/<id>
```

| Estado | Interpretación |
|---|---|
| `NotStarted` | Creado, todavía no iniciado. |
| `Running` | En proceso. |
| `Succeeded` | Completado; archivos disponibles en destino. |
| `Failed` | Ejecución fallida. |
| `ValidationFailed` | La solicitud no superó la validación previa. |

La respuesta completa da más detalle que el simple `status`: dentro de `summary` aparecen los contadores de documentos: totales, con éxito, en error, en curso, pendientes y cancelados, además de `totalCharacterCharged`, los caracteres por los que se te va a cobrar.

![Respuesta de consulta de estado con el resumen de documentos traducidos, en error y caracteres facturados](06-consultar-estado-respuesta.png){: w="1408" h="795" .shadow }
*Ejemplo de respuesta real: el trabajo aparece como `Succeeded`, pero el resumen indica que de tres documentos, solo uno se tradujo correctamente y dos fallaron.*

> El `Succeeded` del trabajo es el estado global, no una garantía de que todos los documentos se hayan traducido bien. Revisa siempre el `summary` para saber cuántos han fallado de verdad.
{: .prompt-warning }

## Errores encontrados y solución

### Missing content-type boundary

Envía el cuerpo como **Body > raw > JSON** con `Content-Type: application/json`, no como `multipart/form-data`.

### InvalidDocumentAccessLevel

```text
Cannot access source document location with the current permissions.
```

Revisa que el SAS del origen incluya `Read` y `List`, que la firma esté completa y que no haya caducado.

### AuthorizationPermissionMismatch

Revisa que el SAS incluya el permiso requerido para la operación concreta: `Write`/`Create` al subir, `List` al enumerar o `Delete` al eliminar.

## Precio

Según la documentación, el coste de traducir documentos de texto se calcula por número de caracteres y, si hay contenido en imágenes, por número de imágenes; siempre en modalidad de pago por uso, aunque tengas contratado algún nivel de compromiso. Tienes el detalle actualizado en la [página de precios de Azure Translator](https://azure.microsoft.com/es-es/pricing/details/translator/).

A eso hay que sumarle el coste del alojamiento en Blob Storage, aunque debería ser muy bajo en comparación con el de la traducción en sí.

> En las pruebas que estamos haciendo hemos lanzado la traducción de unos 1.200 archivos —principalmente pequeños documentos PDF de fichas técnicas e imágenes— y el coste total ha sido de 190 €: 133,10 € en caracteres de documento (`S1 Document Characters`) y 56,17 € en imágenes (`S1 Image Images`). Sale a poco más de 0,15 € por documento, aunque es una media a coger con cuidado: depende del volumen de cada archivo, así que un lote con documentos grandes puede salir más caro.
>
> En otra prueba, con un único PDF de 500 páginas traducido a 6 idiomas, el coste fue de 349 €: los 1.200 archivos anteriores no llegaban entre todos a 2 millones de caracteres, y este documento, multiplicado por los 6 idiomas, se acercaba a los 20 millones. Fue una prueba bruta para ver hasta dónde llegaba el coste, no un caso real: el objetivo de nuestra herramienta no es traducir documentos tan grandes, y para ese volumen tampoco creo que esta sea la herramienta más adecuada.
{: .prompt-tip }

Mi recomendación: haz esta misma cuenta —coste total entre documentos traducidos— con tus propias pruebas antes de llevar el flujo a producción a mayor escala; el precio depende del volumen de caracteres (documento × idiomas de destino), no del número de archivos, así que un único dato no basta para proyectar el coste real.

## Buenas prácticas

- Utiliza permisos SAS mínimos para cada operación, nunca un token con todos los permisos por comodidad.
- Configura una caducidad corta y usa únicamente HTTPS.
- No publiques firmas SAS, claves ni secretos en artículos, repositorios o capturas.
- Valida cada operación por separado en Postman antes de automatizar el flujo completo.
- Conserva el identificador del trabajo de traducción para consultar estado y errores.
- Revisa la estrategia de eliminación temporal de la cuenta antes de usar `DELETE` en producción.

## Conclusión

Azure AI Translator y Azure Blob Storage permiten construir un flujo documental completo mediante APIs REST, sin necesidad de una integración de código a medida desde el primer día. Con Postman puedes validar la carga, el listado, la traducción, la monitorización y la eliminación de documentos antes de llevar la integración a una aplicación o proceso automatizado.

Si vas a implementar este patrón, te recomiendo empezar generando los SAS con permisos mínimos por operación y probando cada paso de forma aislada; automatizar el flujo completo es mucho más sencillo cuando ya sabes que cada pieza funciona por separado.
