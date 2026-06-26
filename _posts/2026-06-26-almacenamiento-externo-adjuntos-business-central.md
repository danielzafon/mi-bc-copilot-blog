---
title: "Almacenamiento externo de adjuntos en Business Central: aligerar la base de datos sin desarrollo"
date: 2026-06-26 10:00:00 +0200
categories: [Business Central, Administración]
tags: [business-central, sharepoint, adjuntos, almacenamiento, entra-id, configuracion]
description: Cómo configurar de forma nativa una cuenta de almacenamiento externo en Business Central para guardar los adjuntos en SharePoint; registro en Entra ID, migración de los documentos existentes y por qué esta novedad nos quita de encima un problema recurrente, el peso de la base de datos.
image:
  path: 01-cuentas-archivo-externas-opciones.png
  alt: Pantalla de cuentas de archivo externas en Business Central con las opciones de almacenamiento disponibles
media_subpath: /assets/img/posts/almacenamiento-externo-adjuntos-business-central/
---

## El peso de la base de datos, ese viejo conocido

Una de las cosas que más he acabado parcheando con desarrollo a lo largo de los años es el peso de la base de datos de los clientes. Y casi siempre el culpable es el mismo: los documentos adjuntos. Facturas en PDF, albaranes escaneados, contratos… todo eso la engorda, encarece el almacenamiento y, en muchos casos, ni siquiera necesita estar dentro de Business Central.

Por eso me ha parecido muy interesante una novedad que BC ha incorporado recientemente: la posibilidad de **configurar cuentas de almacenamiento externas de forma nativa** para la gestión de los adjuntos. Sin desarrollo y sin extensiones de terceros. En este artículo te cuento cómo lo he configurado contra SharePoint y, sobre todo, qué partes merece la pena tener en cuenta antes de ponerlo en marcha en un cliente.

> Documentación oficial de Microsoft: [Store document attachments externally](https://learn.microsoft.com/es-es/dynamics365/business-central/across-store-document-attachments-externally).
{: .prompt-info }

## Antes de empezar: los permisos

Un detalle que conviene resolver primero, porque si no te bloquea todo lo demás: el usuario debe tener permisos en el sitio de SharePoint que vayas a utilizar. Parece obvio, pero en proyectos reales es uno de los puntos donde más tiempo se pierde.

## Configurar la cuenta de archivo externa

Para añadir la cuenta vamos a **Cuentas de archivo externas**. BC nos ofrece varias opciones de destino.

![Opciones de cuentas de archivo externas en Business Central](01-cuentas-archivo-externas-opciones.png){: w="1824" h="777" .shadow }
*Las opciones de almacenamiento disponibles al crear una cuenta de archivo externa.*

En mi caso me voy a centrar en **SharePoint**, que es la que me interesa para este escenario.

## Registrar la aplicación en Entra ID

Antes de configurar nada en BC, tenemos que registrar una aplicación en **Microsoft Entra ID**. Es la parte más tediosa de todo el proceso, pero tiene premio: solo hay que hacerla una vez. De esa aplicación necesitaremos anotar sus datos de identificación para introducirlos después en Business Central.

![Información de la aplicación registrada en Entra ID](02-registro-app-entra-id.png){: w="1601" h="760" .shadow }
*Datos de la aplicación que tendremos que copiar a Business Central.*

Hay que añadir la siguiente URL como **URI de redireccionamiento web** (es siempre la misma, así que cópiala tal cual):

```text
https://businesscentral.dynamics.com/OAuthLanding.htm
```

![Configuración del URI de redireccionamiento web](03-uri-redireccionamiento.png){: w="1355" h="607" .shadow }
*Registro del URI de redireccionamiento en la aplicación de Entra ID.*

Para que funcione, he tenido que añadir los permisos de API que se ven en la siguiente captura.

![Permisos de API necesarios para SharePoint](04-permisos-api-sharepoint.png){: w="1239" h="683" .shadow }
*Permisos de API configurados en la aplicación.*

> Revisa que los permisos cuenten con el consentimiento de administrador concedido. Es otro de los puntos que suele frenar la configuración si se pasa por alto.
{: .prompt-warning }

Además, hay que crear un **secreto** de cliente.

![Creación del secreto de cliente](05-crear-secreto.png){: w="1576" h="825" .shadow }
*Generación del secreto que usaremos en la configuración de BC.*

> Copia el valor del secreto en el momento de crearlo: una vez sales de la pantalla ya no podrás volver a verlo. Y trátalo como una credencial sensible.
{: .prompt-danger }

## Volver a BC y configurar la cuenta de SharePoint

Una vez que tenemos toda la información, volvemos a Business Central a configurar la cuenta de SharePoint.

![Configuración de la cuenta de SharePoint en Business Central](06-cuenta-sharepoint-anadida.png){: w="1475" h="462" .shadow }
*Asistente de configuración de la cuenta de SharePoint.*

En el nombre del sitio de SharePoint, en mi caso he indicado lo siguiente:

```text
https://{MI TENANT}.sharepoint.com/sites/grupo.bc
```

Y la ruta de acceso a la carpeta:

```text
Documentos%20compartidos/Prueba
```

Para el ejemplo he creado una carpeta llamada `Prueba` dentro de los documentos compartidos del sitio. Cuando hayamos indicado todo, le damos a **Siguiente** y confirmamos.

## Validar que la conexión funciona

Volvemos a **Cuentas de archivo externas** y veremos la cuenta que acabamos de añadir. Podemos validar que funciona **explorando el almacenamiento**.

![Cuenta de SharePoint en el listado de cuentas externas](07-explorar-almacenamiento.png){: w="1792" h="507" .shadow }
*La cuenta aparece ya registrada y lista para explorar.*

Se abrirá una ventana con las carpetas que tengas en el sitio (si las tienes).

![Ventana de exploración del almacenamiento de SharePoint](08-ventana-carpetas.png){: w="1339" h="518" .shadow }
*Exploración del contenido del almacenamiento externo desde BC.*

En mi caso había creado dos carpetas de prueba para comprobar que todo se lee correctamente.

![Carpetas de prueba en SharePoint](09-carpetas-prueba.png){: w="1279" h="409" .shadow }
*Las carpetas de prueba se muestran correctamente desde Business Central.*

## Configurar los escenarios de archivo

Una vez validado que funciona, tenemos que configurar los **escenarios de archivo**.

![Escenarios de archivo disponibles](10-escenarios-archivo.png){: w="1474" h="527" .shadow }
*Configuración de los escenarios de archivo.*

Al menos de momento, solo está disponible el escenario de **documentos adjuntos**.

![Escenario de documentos adjuntos](11-escenario-documentos-adjuntos.png){: w="1130" h="436" .shadow }
*El escenario disponible actualmente es el de documentos adjuntos.*

Seleccionamos la carpeta raíz y lo habilitamos.

![Selección de la carpeta raíz](12-seleccionar-carpeta-raiz.png){: w="1869" h="425" .shadow }
*Selección de la carpeta raíz donde se guardarán los adjuntos.*

## Probar con un documento real

Con el escenario habilitado, vamos a añadir un PDF a una **factura de compra** para validar el funcionamiento de extremo a extremo.

![Factura de compra de prueba](13-factura-compra.png){: w="1740" h="836" .shadow }
*Factura de compra sobre la que adjuntamos el documento.*

![Adjuntar el PDF a la factura](14-adjuntar-pdf.png){: w="1741" h="833" .shadow }
*Subimos el PDF como adjunto del documento.*

Si accedemos a los documentos de nuestro sitio en SharePoint, comprobamos que el fichero se ha almacenado dentro de una carpeta con un nombre un tanto extraño.

![Archivo almacenado en SharePoint](15-archivo-en-sharepoint.png){: w="1743" h="357" .shadow }
*El adjunto ya reside físicamente en SharePoint, no en la base de datos de BC.*

Dentro de esa carpeta, BC va creando subcarpetas por tipo de documento.

![Estructura de carpetas por tipo de documento](16-carpetas-por-tipo.png){: w="1241" h="480" .shadow }
*BC organiza los adjuntos en carpetas por tipo de documento.*

![Detalle de las carpetas por tipo](17-carpetas-por-tipo-2.png){: w="1480" h="381" .shadow }
*Detalle de la estructura generada automáticamente.*

## Lo más interesante: migrar y sincronizar lo que ya existe

Aquí es donde, para mí, esta funcionalidad pasa de "está bien" a "esto lo voy a usar en serio". No solo sirve para los adjuntos nuevos: **podemos migrar todos los adjuntos que ya están en BC** al nuevo alojamiento.

![Acción para migrar los adjuntos existentes](18-migrar-adjuntos.png){: w="1869" h="425" .shadow }
*Acción para migrar al nuevo almacenamiento los adjuntos ya existentes en BC.*

Tenemos una acción específica para **migrar los datos** al nuevo almacenamiento, y otra para **sincronizar**, que permite copiar o mover los adjuntos de BC hacia el almacenamiento externo y viceversa.

![Acción de sincronización de adjuntos](19-sincronizar-adjuntos.png){: w="1013" h="573" .shadow }
*La sincronización permite mover o copiar adjuntos en ambos sentidos.*

También es muy útil la acción **Datos adjuntos de documentos almacenamiento externo**, porque nos permite ver todos los adjuntos de BC, comprobar que se han cargado en SharePoint e incluso ver la ruta exacta donde está almacenado cada uno. Desde ahí podemos copiar todos los ficheros al almacenamiento externo o descargarlos.

![Vista de datos adjuntos en almacenamiento externo](20-datos-adjuntos-externo.png){: w="1790" h="816" .shadow }
*Vista consolidada de los adjuntos y su ubicación en el almacenamiento externo.*

Para la puesta en marcha y la migración masiva de documentos, esta acción es oro.

## Conclusión

Es una funcionalidad muy esperada para **aligerar el peso de las bases de datos** de nuestros clientes, algo que en muchos casos habíamos tenido que resolver con desarrollo a medida. Que ahora venga de serie y de forma nativa, con migración y sincronización incluidas, cambia bastante el planteamiento.

Mi recomendación práctica, desde la experiencia en proyectos:

- **Resuelve primero la parte de permisos y Entra ID.** Es la que más fricción genera; tener claros el secreto, el URI de redireccionamiento y el consentimiento de administrador te ahorra la mayoría de los problemas.
- **Decide pronto la estrategia de migración.** Aprovecha la acción de migrar y la de sincronizar para vaciar la base de datos de adjuntos históricos, no solo para los nuevos.
- **Valida siempre de extremo a extremo** con un documento real antes de darlo por bueno en producción.

Una novedad que, sin grandes titulares, soluciona un dolor muy real del día a día. Si ya la has puesto en marcha en algún cliente o te has encontrado con alguna pega en la migración, me encantará leerte.
