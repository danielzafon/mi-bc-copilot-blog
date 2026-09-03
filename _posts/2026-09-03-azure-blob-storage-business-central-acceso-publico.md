---
title: "Azure Blob Storage en Business Central; documentos accesibles sin autenticación"
date: 2026-09-03 10:00:00 +0200
categories: [Business Central, Administración]
tags: [business-central, azure, blob-storage, almacenamiento, adjuntos, configuracion]
description: Cómo configurar una cuenta de almacenamiento de Azure como cuenta de archivo externa en Business Central para publicar documentos con acceso anónimo, sin pasar por SharePoint ni por desarrollo a medida.
image:
  path: 05-configuracion-cuenta-blob-bc.png
  alt: Formulario de configuración de la cuenta de almacenamiento de blobs de Azure en Business Central
media_subpath: /assets/img/posts/azure-blob-storage-business-central-acceso-publico/
---

## Cuando el documento tiene que ser público

Hace poco, en un proyecto interno, me encontré con una necesidad muy concreta: ciertos documentos generados por Business Central (albaranes, en este caso) tenían que quedar accesibles públicamente, sin login, para poder compartir el enlace directamente con terceros que no tienen ni van a tener usuario en nuestro Microsoft 365. Ni SharePoint ni OneDrive resuelven esto de forma directa sin meter enlaces de invitado, caducidades y permisos por documento.

En [otro artículo](https://danielzafon.github.io/mi-bc-copilot-blog/posts/almacenamiento-externo-adjuntos-business-central/) vimos cómo configurar SharePoint como cuenta de archivo externa en Business Central para sacar de la base de datos el peso de los adjuntos. Hoy seguimos explorando esa misma funcionalidad, pero resuelta con Azure Blob Storage en lugar de SharePoint: monté una cuenta de almacenamiento de Azure con un contenedor de acceso anónimo y la conecté a Business Central como cuenta de archivo externa, igual que se conecta SharePoint.

## Preparar la cuenta de almacenamiento en Azure

Antes de tocar Business Central hay que dejar montada la parte de Azure:

1. Crea (o localiza) el recurso de **cuenta de almacenamiento** y anota su nombre; lo vas a necesitar en la configuración de Business Central.

   ![Cuenta de almacenamiento de Azure](01-cuenta-almacenamiento-azure.png){: w="1650" h="882" .shadow }
   _Recurso de cuenta de almacenamiento en Azure Portal_

2. Dentro del recurso, crea un **contenedor** (en este caso, `documentos`) y anota también su nombre.

   ![Contenedor de blobs](02-contenedor-documentos.png){: w="1863" h="887" .shadow }
   _Contenedor creado dentro de la cuenta de almacenamiento_

3. Ve a **Claves de acceso** y copia la clave (`key1` o `key2`). La necesitas para la autenticación por Shared Key desde Business Central.

   ![Clave de acceso de la cuenta de almacenamiento](03-clave-acceso.png){: w="1465" h="895" .shadow }
   _Clave de acceso que usará Business Central para autenticarse_

## Configurar la cuenta de archivo externa en Business Central

Con esos tres datos (nombre de cuenta, nombre de contenedor y clave) ya puedes ir a Business Central:

1. Abre **Configurar cuentas de archivo externas** y elige el tipo **Almacenamiento de blobs**.

   ![Elegir tipo de cuenta de archivo externa](04-configurar-cuentas-archivo-externas.png){: w="1502" h="792" .shadow }
   _Selección del tipo de cuenta al configurar el archivo externo_

2. Rellena el formulario: un nombre descriptivo para identificarla entre tus distintas cuentas de almacenamiento, el **nombre de la cuenta de almacenamiento** de Azure, el tipo de autorización **Shared Key** con la clave que copiaste, y el **nombre del contenedor**.

   ![Configuración de la cuenta de blob en Business Central](05-configuracion-cuenta-blob-bc.png){: w="1575" h="457" .shadow }
   _Formulario completo con los datos de la cuenta de almacenamiento de Azure_

3. Al terminar, Business Central confirma que la cuenta se ha agregado correctamente.

   ![Cuenta de archivo añadida](06-cuenta-agregada-correctamente.png){: w="1503" h="784" .shadow }
   _Mensaje de confirmación tras agregar la cuenta_

La nueva cuenta pasa a convivir con el resto de cuentas de archivo externas que ya tengas configuradas (SharePoint incluido).

![Listado de cuentas de archivo externas](07-lista-cuentas-archivo-bc.png){: w="1318" h="783" .shadow }
_Cuentas de archivo externas configuradas en el entorno, con acceso al explorador de almacenamiento_

## Probar la subida y validarla en Azure

Antes de dar nada por bueno, compruébalo con un documento real. Desde el explorador de almacenamiento de la propia cuenta puedes cargarlo directamente.

![Cargar un documento de prueba desde Business Central](08-explorador-almacenamiento-cargar.png){: w="1230" h="511" .shadow }
_Acción Cargar en el explorador de almacenamiento externo_

Después, entra en el contenedor desde Azure Portal para confirmar que el fichero ha llegado donde toca.

![Contenedores de la cuenta de almacenamiento](09-contenedores-azure-portal.png){: w="1850" h="826" .shadow }
_Acceso al contenedor "documentos" desde Azure Portal_

![Documento cargado dentro del contenedor](10-documento-cargado-contenedor.png){: w="1909" h="562" .shadow }
_El documento subido desde Business Central, visible en el contenedor de Azure_

## Acceso anónimo: la pieza que cambia todo

Hasta aquí, el flujo es idéntico al de conectar cualquier cuenta de archivo externa. La diferencia está en un único ajuste: en mi caso, necesitaba que los documentos se pudieran abrir sin login, así que antes de crear el contenedor habilité el acceso anónimo en la configuración de la cuenta de almacenamiento.

![Acceso anónimo al blob habilitado en la cuenta de almacenamiento](11-acceso-anonimo-blob-habilitado.png){: w="1390" h="897" .shadow }
_"Permitir el acceso anónimo al blob" habilitado a nivel de cuenta de almacenamiento_

> Si no habilitas este ajuste a nivel de cuenta antes de crear el contenedor, no podrás crearlo con acceso anónimo. Si ya tienes el contenedor creado, puedes cambiarle el nivel de acceso a anónimo una vez hecho ese cambio a nivel de cuenta, usando la opción **Cambiar nivel de acceso** del propio contenedor.
{: .prompt-info }

![Cambiar nivel de acceso del contenedor](12-cambiar-nivel-acceso-contenedor.png){: w="1876" h="464" .shadow }
_Opción para cambiar el nivel de acceso anónimo de un contenedor ya creado_

> Un contenedor con acceso anónimo publica en internet cualquier blob que subas ahí, sin más control que el propio nombre del fichero. Antes de activarlo, valora si te conviene separar estos documentos públicos en un contenedor (y una cuenta de archivo en Business Central) distinto del resto de adjuntos, para no mezclar por error documentos internos con los pensados para compartir fuera.
{: .prompt-warning }

## En qué casos merece la pena

Si tu necesidad es aligerar la base de datos y los documentos los va a consultar gente de tu propia organización, SharePoint sigue siendo la opción más natural: aprovechas la gobernanza y los permisos que ya tienes montados. Pero cuando el requisito es que el documento sea accesible por cualquiera con el enlace, sin depender de un usuario ni de una invitación, montar una cuenta de Azure Blob Storage con acceso anónimo y conectarla como cuenta de archivo externa es la vía más directa: no requiere desarrollo, se configura en minutos y encaja igual de bien con el resto de escenarios de archivo de Business Central.
