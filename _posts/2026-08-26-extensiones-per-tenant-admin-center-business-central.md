---
title: "Instalar extensiones per-tenant desde el admin center de Business Central"
date: 2026-08-26 11:30:00 +0200
categories: [Business Central, Administración]
tags: [business-central, extensiones, admin-center, pte, novedades]
description: El admin center de Business Central ya permite subir, instalar y programar extensiones per-tenant desde Manage Apps, con seguimiento del resultado en Operations; todavía no permite fijar una fecha y hora concretas de instalación.
image:
  path: 02-install-extension-panel.png
  alt: Panel Install Per-Tenant Extension en el admin center de Business Central, con fichero .app, calendario de despliegue y modo de sincronización
media_subpath: /assets/img/posts/extensiones-per-tenant-admin-center-business-central/
---

Hasta ahora, subir una extensión per-tenant (PTE) a un entorno de producción pasaba casi siempre por la página **Extension Management** dentro del propio entorno, o por la Automation API si querías automatizarlo. Ese segundo camino funcionaba, pero exigía montar la autenticación y la llamada al endpoint `extensionUpload` a mano, sin visibilidad centralizada de qué estaba instalado en cada entorno del tenant.

Con la disponibilidad general de esta funcionalidad, la gestión completa de PTEs —subida, instalación, programación y desinstalación— ya está en la página **Manage Apps** del admin center, tanto desde la interfaz como por API.

## Cómo se instala

Desde la ficha de tu entorno, dentro de **Environments**, entras por el botón **Apps** de la barra superior.

![Botón Apps en la página de detalle del entorno, en el admin center de Business Central](01-entorno-boton-apps.png){: w="1720" h="784" .shadow }
*El acceso está en la propia ficha del entorno, junto a Sessions, Export Database o Restore.*

Ya en **Manage Apps**, el botón **Install Extension** abre un panel con tres decisiones:

![Panel Install Per-Tenant Extension con el fichero .app, el calendario de despliegue y el modo de sincronización](02-install-extension-panel.png){: w="1903" h="880" .shadow }
*Business Central determina automáticamente si el .app es una extensión nueva o una actualización de una ya instalada.*

- **Extension file (.app)**: el paquete a subir.
- **Deployment schedule**: instalar de inmediato, en la próxima ventana de actualización del entorno o en la próxima actualización menor o mayor. Para una PTE nueva solo tiene sentido inmediato o próxima ventana; programar una instalación nueva para una actualización futura no está soportado.
- **Sync mode**: el mismo criterio de siempre a la hora de sincronizar el esquema de datos tras publicar.

El progreso se sigue desde **Operations**, donde puedes ver si, como en mi caso, ha habido un error al instalar tu extensión.

![Página Environment Operations del admin center mostrando una operación App install con estado Failed](03-operations-app-install-failed.png){: w="1585" h="526" .shadow }
*El listado incluye el usuario que la invocó y las fechas de inicio y finalización; desde ahí entras al detalle para ver la causa del fallo.*

Lo único que echo en falta, de momento, es poder programar la instalación para una fecha y hora concretas. En el día a día es habitual subir estas actualizaciones fuera del horario laboral del cliente, para no interferir con su trabajo, y en eso el admin center no cambia nada respecto a lo de siempre: ninguna de las opciones de programación permite fijar un momento exacto, así que si quieres instalarla a esa hora, sigues teniendo que estar tú delante del ordenador para lanzarla de inmediato.
