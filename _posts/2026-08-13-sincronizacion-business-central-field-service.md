---
title: "Sincronizar Business Central con Field Service; diario de proyectos frente a módulo de servicios"
date: 2026-08-13 10:00:00 +0200
categories: [Business Central, Integración]
tags: [business-central, field-service, dataverse, dynamics-365, integracion, proyectos]
description: Cómo valoré y probé la sincronización entre Business Central y Field Service para una oferta de implantación de Field Service a un cliente de Business Central; por qué elegí el diario de proyectos frente al módulo de servicios, las tablas virtuales y los problemas que me encontré por el camino.
image:
  path: 01-conexion-dataverse.png
  alt: Pantalla de configuración de la conexión de Business Central con Dataverse
media_subpath: /assets/img/posts/sincronizacion-business-central-field-service/
---

Tenía que preparar una valoración para la sincronización de Business Central con Field Service, dentro de una oferta para implantar Field Service en un cliente que ya trabaja con Business Central. Para poder estimar el trabajo y pasar los números a los compañeros de Dynamics, me puse a probar la sincronización en un entorno de pruebas.

Esto no pretende ser una guía paso a paso de configuración; más bien te comparto las posibilidades que tenemos y las dificultades con las que me he encontrado.

## Dos formas de activar la sincronización

A grandes rasgos, Business Central nos da dos caminos para sincronizar con Field Service:

- Contra el diario de proyectos.
- Contra el módulo de servicios y el diario de proyectos.

La primera opción se ha añadido recientemente y permite sincronizar Business Central con Field Service **sin necesidad de la licencia Premium** ni de tener el módulo de servicios (ese gran desconocido) puesto en marcha.

## Por qué elegí ir contra el diario de proyectos

Después de explorar por encima la experiencia con el módulo de servicios, mi propuesta fue hacerlo contra el diario de proyectos para registrar los consumos.

¿Por qué tomé esta decisión? Para este proyecto, el cliente va a utilizar Field Service como herramienta para su SAT (servicio de asistencia técnica), y la única necesidad que tiene en Business Central es el registro de los consumos de los repuestos. Para este escenario no tiene sentido montar el módulo de servicios: duplicaría el trabajo (y el coste para el cliente) y, mirando más allá, cuantas más cosas haya que sincronizar, más problemas dará.

![Listado de órdenes de trabajo en Dynamics 365 Field Service](23-ordenes-trabajo-field-service.png){: w="1881" h="882" .shadow }
*Así se ven las órdenes de trabajo en Field Service, el punto de partida de todo lo que vamos a sincronizar con Business Central.*

> Te recomiendo que, a diferencia de mí, te leas la documentación de Microsoft antes de empezar: [Integrate Business Central with Dynamics 365 Field Service](https://learn.microsoft.com/es-es/dynamics365/business-central/admin-integrate-field-service).
{: .prompt-tip }

## Aplicaciones necesarias

Antes de tocar nada en Business Central, hay que instalar dos aplicaciones desde AppSource. Sin ellas ni siquiera vas a ver las opciones de configuración de las que hablo más abajo, así que no te las saltes:

- [Virtual Entities for Dynamics 365 Business Central](https://marketplace.microsoft.com/en-us/product/DynamicsCE/microsoftdynsmb.businesscentral_virtualentity): habilita las tablas virtuales, la pieza que más adelante necesitarás para exponer la disponibilidad de stock en Field Service.
- [Dynamics 365 Field Service Integration](https://marketplace.microsoft.com/en-us/product/dynamics-365-business-central/PUBID.microsoftdynsmb%7CAID.fieldserviceintegration%7CPAPPID.1ba1031e-eae9-4f20-b9d2-d19b6d1e3f29?tab=Overview): añade la parametrización específica de Field Service sobre la integración estándar con Dynamics 365 Sales, que es la que vamos a configurar a continuación.

## Configurar las tres conexiones: Dataverse, Sales y Field Service

Tenemos tres configuraciones que activar: la del Dataverse, la de Dynamics 365 Sales y la específica de Field Service.

> Para poder activar la sincronización necesitas al menos dos usuarios: uno con permisos de administrador en el entorno de Field Service, que se usa para configurar la sincronización y desplegar la solución, y otro sin permisos de administrador, que será el que se use para la sincronización de los registros. Este segundo usuario tiene que ser distinto del primero: si tiene permisos de administrador, la configuración te dará un error indicando que ese usuario no puede ser administrador.
{: .prompt-warning }

![Configuración de la conexión de Dataverse](01-conexion-dataverse.png){: w="1447" h="777" .shadow }
*Conexión de Business Central al entorno de Dataverse.*

![Configuración de integración de Dynamics 365 Sales](02-conexion-dynamics-365-sales.png){: w="1391" h="781" .shadow }
*Conexión de Dynamics 365 Sales, necesaria como paso previo a Field Service.*

![Configuración de integración de Dynamics 365 Field Service](03-conexion-field-service-resumen.png){: w="1365" h="723" .shadow }
*Resumen de la conexión con Field Service, ya con el tipo de integración configurado.*

## Configuración de la sincronización con Field Service

Si ya has configurado alguna vez la sincronización con Dynamics 365 Sales, te sonará la **configuración asistida**.

![Asistente de configuración de integración con Field Service](04-asistente-configuracion-field-service.png){: w="1351" h="746" .shadow }
*Pantalla de bienvenida del asistente de conexión con Dynamics 365 Field Service.*

Para el caso de la integración con Field Service, además del entorno, tendremos la siguiente parametrización.

![Parámetros de la integración con Field Service en el asistente](05-parametros-integracion-field-service.png){: w="1847" h="428" .shadow }
*Plantilla de diario de proyecto, sección de diario, unidad de medida y tipo de integración.*

Aquí está el parámetro más importante de todo el proceso: decidir el **tipo de integración** que vamos a realizar, Proyectos o Servicios y proyectos.

![Opciones del tipo de integración: Proyectos o Servicios y proyectos](06-tipo-integracion-opciones.png){: w="1885" h="485" .shadow }
*El tipo de integración determina si trabajaremos solo contra proyectos o también contra el módulo de servicios.*

Nos permite seleccionar cuándo se van a sincronizar los productos o servicios: cuando se utilizan en las órdenes de trabajo o cuando se completan.

![Opciones de cuándo sincronizar productos o servicios de las órdenes de trabajo](07-sincronizar-productos-servicios-cuando.png){: w="1870" h="486" .shadow }
*Momento en el que se sincronizan los productos o servicios de la orden de trabajo.*

Para los consumos de material, además tenemos la opción de registrar manualmente el diario que hemos seleccionado previamente, junto con la unidad de medida para el tiempo.

![Opciones para registrar automáticamente las líneas del diario de proyecto](08-registrar-lineas-diario-opciones.png){: w="1884" h="452" .shadow }
*También se puede optar por registrar las líneas del diario de forma manual.*

> Es muy recomendable crear una sección específica para la integración y no utilizarla para otras cosas.
{: .prompt-warning }

El tipo de integración **no se puede cambiar una vez finalizado** (bueno, sí es posible, pero realizando la configuración de nuevo). El resto de parámetros, como la sección del diario o cuándo se registran los consumos, sí se pueden modificar después.

![Resumen de la configuración de sincronización con Field Service](09-resumen-configuracion-sincronizacion.png){: w="1771" h="623" .shadow }
*Los parámetros de sincronización, editables una vez finalizada la configuración inicial.*

## Qué implica trabajar con la integración por proyectos

Si te decantas, como yo, por la sincronización contra el diario de proyectos, ten en cuenta que hay unas entidades mínimas que vas a necesitar sincronizar sí o sí para que todo esto funcione:

- Clientes
- Almacenes
- Productos, tanto de inventario como de servicio
- Unidades de medida
- Recursos
- Tareas de proyecto

Esta última no es casual: cuando trabajamos la integración mediante proyectos, Field Service nos obliga a indicar en la orden de trabajo la tarea de proyecto de Business Central sobre la que se va a registrar el diario de proyecto.

![Selector de tarea de proyecto de Business Central desde una orden de trabajo de Field Service](24-elegir-tarea-proyecto-orden-trabajo.png){: w="1830" h="873" .shadow }
*Desde la orden de trabajo hay que elegir la tarea de proyecto de Business Central contra la que se registrará el consumo.*

Esa tarea se indica en el campo **Proyecto externo** de la propia orden de trabajo.

![Campo "Proyecto externo" en la creación de una orden de trabajo de Field Service](25-campo-proyecto-externo-orden-trabajo.png){: w="1909" h="852" .shadow }
*El campo "Proyecto externo" de la orden de trabajo, donde queda enlazada la tarea de proyecto de Business Central.*

Para mi caso he creado un proyecto con una tarea genérica donde se registrarán todos los consumos. En un entorno real, lo más probable es que esta información se complete de forma automática, ya que todos los consumos se acabarán registrando sobre el mismo proyecto de Business Central.

Algo muy interesante que descubrí en este punto: como la orden de trabajo lleva su propio campo de compañía, podemos sincronizar **varias empresas de Business Central contra el mismo entorno de Field Service**.

![Campo "Compañía" en una orden de trabajo de Field Service](26-campo-compania-orden-trabajo.png){: w="1332" h="848" .shadow }
*Cada orden de trabajo lleva su propio campo "Compañía", lo que permite repartir órdenes de distintas empresas de Business Central contra un único entorno de Field Service.*

Para mi caso, el cliente tiene tres entornos de Business Central con localizaciones diferentes que se van a sincronizar contra el mismo entorno de Field Service, así que esta pieza no era un detalle menor a la hora de valorar el proyecto.

## Disponibilidad de stock: activar las tablas virtuales

Hay un requisito que conviene tener claro desde el principio: si quieres que los técnicos vean en Field Service la disponibilidad de un artículo en Business Central, las **tablas virtuales** tienen que estar activas. Es la misma aplicación de Virtual Entities que instalamos antes, pero aquí es donde entra en juego de verdad, así que no es una casilla más a marcar sin pensar.

![Activar disponibilidad de inventario por ubicación](10-disponibilidad-inventario-ubicacion.png){: w="1392" h="718" .shadow }
*La disponibilidad de inventario por ubicación solo está disponible si la aplicación Virtual Table está instalada.*

En cuanto la activas, Business Central da de alta un trabajo específico en la cola de trabajos para mantener sincronizada esa disponibilidad.

![Trabajo de disponibilidad de producto en la cola de trabajos](11-cola-trabajos-crm-item-availability.png){: w="1744" h="838" .shadow }
*El trabajo "CRM Item Availability Job" en la cola de trabajos, encargado de sincronizar la disponibilidad.*

Asegúrate de que las tablas virtuales están configuradas correctamente antes de dar nada por bueno; a mí, en la cola de trabajos, me apareció un error.

![Error en la cola de trabajos al habilitar las tablas virtuales de Dataverse](12-cola-trabajos-error-tablas-virtuales.png){: w="1737" h="817" .shadow }
*El trabajo "Enable CDS Virtual Tables" terminó en estado de error.*

![Mensaje de error de comunicación con Dataverse](13-error-comunicacion-dataverse.png){: w="598" h="273" .shadow }
*"The data configuration for calling Business Central is incomplete. Specify a value for Environment."*

## Un problema con una configuración anterior a medio hacer

En la configuración de tablas virtuales tenía una entrada que, supongo, venía de alguna prueba anterior incompleta.

![Acceso a las tablas virtuales disponibles desde la conexión de Dataverse](14-tablas-virtuales-disponibles-acceso.png){: w="1363" h="806" .shadow }
*Acceso a "Tablas virtuales disponibles" desde la configuración de la conexión de Dataverse.*

![Dos configuraciones duplicadas de origen de datos de Business Central](15-configuraciones-duplicadas-tablas-virtuales.png){: w="1917" h="444" .shadow }
*Aparecían dos entradas "Business Central" en el listado de configuraciones.*

Probé a eliminarla y, como no me dejaba, completé la información con la de mi entorno.

![Configuración de origen de datos con el entorno vacío](16-configuracion-incompleta-entorno.png){: w="1901" h="524" .shadow }
*Nombre de entorno y empresa predeterminada sin rellenar: el origen del error.*

![Configuración de origen de datos con el entorno completado](17-configuracion-completada-entorno.png){: w="1002" h="463" .shadow }
*Tras completar el nombre de entorno y la empresa predeterminada, la conexión funcionó.*

Probé a revisar las tablas disponibles y ya aparecían.

![Listado de tablas virtuales disponibles de Business Central](18-listado-tablas-disponibles-business-central.png){: w="1806" h="878" .shadow }
*Las 147 tablas de Business Central expuestas como tablas virtuales.*

## Por qué el inventario no aparecía en Field Service

Revisando en Field Service, el inventario seguía sin aparecer, así que volví a la documentación y encontré [este apartado sobre disponibilidad de artículo](https://learn.microsoft.com/es-es/dynamics365/business-central/admin-integrate-field-service#optional-make-item-availability-information-in-business-central-available-in-field-service).

![Documentación de Microsoft sobre la asignación de tabla LOCATIONS](19-doc-microsoft-asignacion-locations.png){: w="801" h="734" .shadow }
*La asignación LOCATIONS solo está disponible si se activa "Almacén obligatorio" en Configuración de inventario.*

En mi caso, no lo tenía marcado.

![Almacén obligatorio activado en Configuración de inventario](20-almacen-obligatorio-config-inventario.png){: w="1386" h="774" .shadow }
*Hay que activar "Almacén obligatorio" para que la asignación de ubicaciones funcione.*

Tiene lógica, pero puede que a alguien no le sea tan obvio: el almacén no puede ser de tránsito ni utilizar manipulación de almacén en los consumos de los proyectos.

![Documentación de Microsoft sobre los requisitos de ubicación y almacén](21-doc-microsoft-requisitos-ubicacion-almacen.png){: w="900" h="580" .shadow }
*Requisitos de la tarjeta de ubicación para que la sincronización funcione: sin tránsito y sin manipulación de almacén.*

> Llegado a este punto debería funcionar, pero seguía sin aparecer en Field Service. Sospecho que es un tema de la otra aplicación; lo revisaré con mis compañeros y actualizaré el post con el resultado.
{: .prompt-info }

## Ojo con la sincronización completa

Con la sincronización de consumos ya en marcha, di varias veces a **ejecutar la sincronización completa** para hacer pruebas, y me registró muchas veces el mismo consumo.

![Movimientos de proyecto con el mismo consumo registrado varias veces](22-consumos-duplicados-sincronizacion-completa.png){: w="1725" h="834" .shadow }
*El mismo consumo de "Bomba remota" registrado repetidas veces tras varias ejecuciones de la sincronización completa.*

Esto tiene pinta de que va a ser un problema seguro de cara a una puesta en marcha en cliente. Lo tendré en cuenta a la hora de valorar el proyecto: probablemente convenga limitar quién puede lanzar esa sincronización completa, o al menos advertir de que no es una acción para repetir alegremente en pruebas.

## Conclusión

Con todo esto pude cerrar una estimación razonable para la oferta: el tipo de integración contra el diario de proyectos evita montar el módulo de servicios, pero el tiempo que hay que reservar para las tablas virtuales, la disponibilidad de stock por ubicación y la validación de la sincronización completa no es menor.

Mi recomendación: si vas a valorar un escenario parecido, no des por hecho que la parte de configuración es trivial solo porque no toques el módulo de servicios, y prueba varias veces la sincronización completa en un entorno de pruebas antes de darla por buena en cliente.

Si te has encontrado con esta integración en algún proyecto, o sabes por qué el inventario seguía sin aparecer en Field Service en mi caso, me encantará leerte en los comentarios.
