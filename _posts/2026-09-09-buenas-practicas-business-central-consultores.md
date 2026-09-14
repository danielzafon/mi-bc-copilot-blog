---
title: "Buenas prácticas en Business Central: los consejos que repito a mi equipo de consultores"
date: 2026-09-09 10:00:00 +0200
categories: [Business Central, Funcional]
tags: [business-central, buenas-practicas, consultoria, iva, contabilidad, productividad]
description: Recopilación práctica de consejos de uso diario en Business Central sobre registro, IVA, plan de cuentas, diarios y producción, recuperada de una sesión interna con mi equipo de consultores.
image:
  path: 01-test-plan-cuentas.png
  alt: Plan de cuentas de Business Central con la acción "Test plan de cuentas" resaltada
media_subpath: /assets/img/posts/buenas-practicas-business-central-consultores/
---

Hace un tiempo preparé una sesión interna para el equipo de Business Central de mi empresa con un objetivo muy concreto: juntar en un solo sitio los trucos, atajos y precauciones que cada uno va aprendiendo por su cuenta, a base de proyectos, tickets de soporte y algún que otro susto. No era una sesión de teoría de Business Central, sino un repaso de lo que de verdad usamos —o deberíamos usar— en el día a día, tanto nosotros, como consultores, como los propios usuarios de nuestros clientes.

Recupero aquí esa sesión en formato artículo, agrupada en cuatro bloques: trabajo diario y registro, IVA, contabilidad y plan de cuentas, y cobros, pagos y producción.

## Trabajo diario y registro

### La ayuda integrada, el primer sitio donde mirar

Business Central trae ayuda integrada que da acceso a documentación y guías directamente desde la propia aplicación, sin salir del entorno de trabajo. Se accede pulsando el icono de interrogación en la barra superior, y abre un panel con información sobre la página o tarea en la que estás, enlaces a Microsoft Learn y otros recursos de soporte. Antes de escalar una duda a soporte o de ponerse a buscar en Microsoft Learn, merece la pena acostumbrar a los usuarios —y acostumbrarnos nosotros mismos— a mirar primero ahí. Resuelve más dudas de las que parece, y ahorra tickets que ni siquiera hacía falta abrir.

![Panel de ayuda de Business Central abierto desde el icono de interrogación de la barra superior](09-ayuda-integrada.png){: w="1810" h="927" .shadow }
*Panel de ayuda contextual, con información de la página actual y enlaces a Microsoft Learn*

### Fecha de trabajo, no la fecha de hoy

La fecha de trabajo permite indicarle al sistema una fecha distinta a la actual, algo muy útil cuando hay que registrar varios documentos con una fecha diferente a la de hoy. Para los usuarios es cuestión de configurarla correctamente antes de una sesión de registro masivo con fecha atrasada.

> **Para quienes programamos en AL:** el matiz es más importante de lo que parece. A la hora de asignar fechas al crear documentos hay que usar `Workdate()` y no `Today()`. Usar `Today()` donde tocaba `Workdate()` es un error sutil que no falla en desarrollo, pero que se nota en cuanto un usuario cambia su fecha de trabajo y el documento se crea con una fecha que no esperaba.
{: .prompt-tip }

![Ventana "Mi configuración" con el campo "Fecha de trabajo" resaltado sobre la empresa CRONUS ES](03-fecha-trabajo.png){: w="998" h="980" .shadow }
*Campo "Fecha de trabajo" en "Mi configuración"*

### Limitar el periodo de registro permitido

En la Configuración contabilidad tienes los campos "Permitir registro desde" y "Permitir registro hasta", que acotan el rango de fechas en el que se puede registrar cualquier documento. Es un control sencillo pero con mucho impacto: evita que, por error o por despiste, alguien registre un documento con una fecha de un periodo ya cerrado o de un ejercicio que todavía no ha empezado.

> Lo habitual es ir moviendo ese rango según avanza el cierre del periodo o el ejercicio, en lugar de dejarlo abierto de forma indefinida. Como conté en [una entrada anterior](https://danielzafon.github.io/mi-bc-copilot-blog/posts/rangos-fechas-permitidos-business-central-v28/), desde la v28 estos campos admiten una fórmula de fecha en lugar de una fecha fija, así que puedes dejar el registro siempre limitado al mes actual (`-PM`/`PM` en un entorno en castellano) sin tener que ir actualizándolo periodo a periodo.
{: .prompt-tip }

![Configuración contabilidad con los campos "Permitir registro desde" y "Permitir registro hasta" resaltados, con un rango de fechas indicado](07-permitir-registro-desde-hasta.png){: w="1748" h="760" .shadow }
*Campos "Permitir registro desde" y "Permitir registro hasta" en la Configuración contabilidad*

### Vista previa antes de registrar

Antes de registrar cualquier transacción, la función de vista previa de registro te deja verificar que todos los datos son correctos. Es especialmente útil al crear registros nuevos en los maestros —clientes, proveedores, productos, activos— porque así puedes comprobar antes de registrar ningún documento que el cliente, proveedor, producto o lo que corresponda tiene los datos y los grupos contables correctamente indicados.

![Ventana de vista previa de registro con los movimientos relacionados: contabilidad, cliente, producto, IVA y movimiento valor](04-vista-previa-registro.png){: w="1683" h="706" .shadow }
*Vista previa de registro, con el desglose de movimientos relacionados antes de confirmar*

### Registrar es para la posteridad

Una vez que una transacción se registra, ya no se puede modificar ni eliminar. Si hace falta un cambio, hay que revertir el asiento contable o emitir un abono en el caso de una factura. Es una de las primeras cosas que explico a cualquier usuario nuevo, porque cambia por completo cómo hay que abordar un error: no se "corrige", se revierte o se abona, y el registro original queda ahí, para siempre.

> **Por experiencia:** dejar esto claro desde un principio a los usuarios evita muchos disgustos en el futuro.
{: .prompt-tip }

### Editar en Excel, con cuidado

Editar en Excel es muy útil para importaciones y modificaciones masivas de registros. Pero es también muy peligroso si lo usan usuarios que no son avanzados: permite cambios directos y en bloque, sin las validaciones línea a línea que sí tienes al editar desde la propia página. Tanto abrir en Excel como editar en Excel requieren tener asignado un conjunto de permisos específico, así que es una barrera adicional —y una palanca de control— que conviene usar: no lo des por defecto a todo el mundo, resérvalo para consultores o usuarios avanzados. Mi recomendación es, además, trabajar siempre con una copia o un filtro claro de qué se va a tocar antes de guardar.

![Menú de exportación con las opciones "Abrir en Excel" y "Editar en Excel" resaltadas sobre un listado de clientes](02-editar-en-excel.png){: w="1657" h="856" .shadow }
*Opciones "Abrir en Excel" y "Editar en Excel" desde un listado de clientes*

### Analizar sin salir de BC

La función Analizar es útil para contar registros o hacer sumatorios, y permite filtrar, agrupar o dinamizar los datos de forma parecida a una tabla dinámica sin salir de Business Central. Desde la versión 26.2 además permite incluir información de tablas relacionadas, lo que la hace todavía más práctica para resolver de un vistazo preguntas que antes requerían exportar a Excel.

![Modo de análisis sobre el histórico de facturas de venta, con el importe agrupado y sumado por cliente](10-analizar-importe-por-cliente.png){: w="1606" h="861" .shadow }
*Análisis del histórico de facturas de venta, agrupado por cliente con el importe sumado*

## IVA y fiscalidad

### Editar movimientos de IVA, cuando hace falta

Es posible editar los movimientos de IVA para corregir o actualizar información como el CIF y el código de país. No es algo que se deba hacer a la ligera, pero es útil y necesario para corregir errores al enviar el SII y para extraer correctamente los modelos 347 y 349. Por experiencia, la mayoría de problemas al sacarlos vienen de clientes o proveedores con movimientos sin CIF o sin el código de país informado, así que cuando un cliente detecta una discrepancia en estos modelos, ahí es el primer sitio donde miro antes de buscar el problema en otro lado.

![Página "Movs. IVA" en modo "Editar lista" con las columnas "Cód. país/región" y "CIF/NIF" resaltadas](11-modificar-movs-iva.png){: w="1738" h="843" .shadow }
*Movimientos de IVA en modo "Editar lista", con el código de país/región y el CIF/NIF editables*

> La columna CIF/NIF que se ve en la captura no está visible por defecto; la he sacado yo mismo para poder editarla directamente desde la lista.
{: .prompt-info }

### Fecha de registro y fecha de IVA no son lo mismo

Trabajar con ambas fechas permite controlar por separado el periodo de registro de contabilidad y el de facturas. Es una distinción que muchos usuarios no tienen presente hasta que se topan con un caso real —una factura que llega tarde, un cierre de periodo que no cuadra— y ahí es donde entender la diferencia entre las dos fechas evita bastantes quebraderos de cabeza a la hora de declarar.

### Rango de fechas de IVA

En la Configuración de IVA, los campos "Permitir fecha de IVA desde" y "Permitir fecha de IVA hasta" limitan el rango de fechas de IVA en el que se pueden registrar movimientos, de forma independiente al rango de registro general. Es el mismo criterio que el periodo de registro contable, pero aplicado específicamente al IVA: te permite cerrar un periodo de IVA ya declarado sin tener que bloquear también el registro contable general, algo especialmente útil cuando la periodicidad de declaración de IVA no coincide con la del cierre contable.

> Por ejemplo, esto nos permite limitar el registro de facturas —las que generan IVA— al mes actual, mientras que seguimos permitiendo registrar asientos durante un periodo más amplio. Es una combinación habitual: control estricto sobre lo que va a declaración de IVA, y algo más de margen para la contabilidad general.
{: .prompt-tip }

![Configuración de IVA con la sección "Fecha de IVA" resaltada: "Permitir fecha de IVA desde" y "Permitir fecha de IVA hasta"](08-rango-fecha-iva.png){: w="1276" h="434" .shadow }
*Sección "Fecha de IVA" en la Configuración de IVA*

### Bloquear cruces de IVA no permitidos

Configurar el sistema para bloquear cruces de IVA no permitidos asegura que todas las transacciones cumplan con la normativa fiscal y evita errores en la declaración de IVA. Es una de esas configuraciones que se hacen una vez al principio del proyecto y luego se olvidan, pero que conviene revisar cuando el cliente amplía la actividad a nuevos países o tipos de operación.

![Config. grupos registro IVA con la columna "Bloqueado" resaltada y el cruce VAT7/VAT7 marcado](06-bloquear-cruces-iva.png){: w="1671" h="782" .shadow }
*Configuración de grupos de registro de IVA, con un cruce marcado como bloqueado*

## Contabilidad y plan de cuentas

### Cuentas contables en facturas

Para poder utilizar una cuenta contable en una factura de compra o de venta, esta tiene que tener indicado en su ficha un grupo contable de registro (normalmente VARIOS), la entrada directa marcada y, siempre que sea posible, el grupo de IVA correspondiente. Sin el grupo contable configurado, la cuenta sí aparece como opción en la línea, pero al intentar registrar el documento da error: no te deja continuar y te obliga a indicar el grupo contable manualmente en cada línea.

![Ficha de cuenta contable con "Registro directo" activado y los grupos contables de producto general e IVA resaltados](12-cuenta-contable-registro-directo.png){: w="1412" h="845" .shadow }
*Ficha de cuenta contable, con "Registro directo" y los grupos contables de producto general e IVA configurados*

### Entrada directa, solo donde toca

Para evitar descuadres conviene desmarcar la entrada directa en todas las cuentas asociadas a grupos contables: si una cuenta ya recibe sus movimientos a través del grupo contable, no debería poder recibir también entradas manuales sueltas. La acción "Lista punto uso" te dice si una cuenta está siendo utilizada en algún grupo o parametrización contable, así que antes de tocar la entrada directa de una cuenta, revisa primero ahí para no romper nada que dependa de ella.

![Ficha de cuenta contable con la acción "Lista punto uso" resaltada, junto a los interruptores de saldo controlable en diarios y entrada directa](05-entrada-directa-lista-punto-uso.png){: w="1530" h="871" .shadow }
*Ficha de cuenta contable, con "Lista punto uso" y los interruptores de "Saldo controlable en diarios" y "Registro directo"*

### Test plan de cuentas, siempre

Después de crear una cuenta, lanza siempre el Test plan de cuentas. Evita problemas a la hora de registrar el asiento de regularización, y de paso evita encontrarte con el plan de cuentas mostrando cuentas sin indentar cuando lo revisas más adelante. Es un paso de treinta segundos que ahorra bastante más tiempo del que cuesta.

![Menú "Inicio" del plan de cuentas con la opción "Test plan de cuentas" resaltada](01-test-plan-cuentas.png){: w="2063" h="974" .shadow }
*Acción "Test plan de cuentas" desde el menú "Inicio" del plan de cuentas*

### Arrastrar la descripción a los movimientos contables

En la configuración de compras y ventas puedes indicar que se arrastre la descripción de las líneas de tipo cuenta de las facturas a los movimientos contables. Sin esto, el apunte queda con el texto genérico de registro, que no dice nada sobre qué se ha comprado o vendido; con esto activado, cualquiera que revise un extracto de cuenta tiene el contexto ahí directamente.

![Conf. compras y pagos con la opción "Copiar descripción línea a mov. contabilidad" resaltada y activada](14-copiar-descripcion-movs-contabilidad.png){: w="1758" h="814" .shadow }
*Opción "Copiar descripción línea a mov. contabilidad" en la Conf. compras y pagos*

### Mostrar importes diarios y asientos completos

Esta es una de las primeras cosas que cambio siempre en la Configuración contabilidad. Por defecto, el campo "Mostrar importes" viene en "Solo importe", cuando lo normal es que los contables trabajen al menos con Debe/Haber.

![Configuración contabilidad con el campo "Mostrar importes" resaltado y la opción "Solo Debe/Haber" seleccionada](13-mostrar-importes.png){: w="1745" h="799" .shadow }
*Campo "Mostrar importes" en la Configuración contabilidad, con las opciones "Solo importe", "Solo Debe/Haber" y "Todos los importes"*

### Saldo controlable en diarios

Activar el saldo controlable en diarios es útil para comprobar el saldo resultante antes de registrar, por ejemplo al hacer alguna corrección. Te deja ver de un vistazo si el diario va a cuadrar antes de darle a registrar, en lugar de descubrirlo después.

![Ficha de cuenta contable con "Saldo controlable en diarios" activado](15-saldo-controlable-diarios.png){: w="1711" h="853" .shadow }
*Opción "Saldo controlable en diarios" en la ficha de cuenta contable*

Con la opción activada, en el diario general aparece la acción "Control", que te muestra el saldo resultante de esa cuenta antes de registrar.

![Diario general con la acción "Control" resaltada en la cinta de opciones](16-control-diario.png){: w="1462" h="800" .shadow }
*Acción "Control" disponible en el diario general para la cuenta con saldo controlable activado*

Al pulsarla, se abre la ventana "Control saldos", que te permite ver el saldo resultante en la cuenta después de registrar el diario, junto con el saldo del periodo en el propio diario.

![Ventana "Control saldos" con las columnas "Saldo periodo en diario" y "Saldo después del registro"](17-control-saldos.png){: w="1206" h="397" .shadow }
*Ventana "Control saldos", con el saldo del periodo en el diario y el saldo resultante tras el registro*

### Secciones de diario bien configuradas

Al montar secciones de diario conviene revisar tres cosas: desmarcar "Copiar conf. IVA" cuando no corresponda, decidir si se copian las líneas del diario ya registrado, y configurar bien las secciones por banco con su contrapartida. Son detalles de configuración que no se ven hasta que alguien registra un diario y el resultado no es el esperado.

![Secciones diario general con las columnas "Copiar conf. IVA a lín. dia." y "Copiar a líneas dia. registradas" resaltadas](18-secciones-diario-general.png){: w="1854" h="395" .shadow }
*Secciones de diario general, con "Copiar conf. IVA a lín. dia." y "Copiar a líneas dia. registradas"*

Recomiendo marcar "Copiar a líneas dia. registradas": permite copiar fácilmente asientos ya registrados al diario, tanto si necesitas deshacerlos como si vas a registrar uno parecido.

![Diario general registrado con las acciones "Copiar las líneas..." y "Copiar el registro..." resaltadas](19-copiar-lineas-diario-registrado.png){: w="1848" h="545" .shadow }
*Diario general registrado, con las acciones para copiar líneas o el registro completo de vuelta al diario*

## Cobros, pagos y producción

### Liquidar por remesas o desde la orden de pago

Liquidar por lotes desde el listado genera un único movimiento de banco por orden o remesa. Liquidar desde dentro de las órdenes, en cambio, genera un movimiento en el banco por cada documento. Cuando se trabaja con muchos documentos o vencimientos, es muy útil marcar que solo liquide los documentos vencidos, para no arrastrar en la liquidación algo que todavía no tocaba.

### Revertir salidas y consumos de producción

Hay una funcionalidad relativamente reciente para deshacer salidas y consumos erróneos en órdenes de producción. Evita tener que deshacerlo manualmente como se hacía hasta ahora, mediante ajustes positivos y negativos calculados a mano. Si trabajas con órdenes de producción, vale la pena tenerla localizada antes de que haga falta usarla con prisa.

> En las capturas de las órdenes de producción he enmascarado el código y la descripción de los productos para que no se vean.
{: .prompt-info }

![Movs. productos de una orden de producción terminada, con la acción "Revertir movimiento de producción" resaltada en el menú Acciones](20-revertir-movimiento-produccion.png){: w="1822" h="319" .shadow }
*Acción "Revertir movimiento de producción" desde los movimientos de productos de una orden terminada*

También puedes volver a abrir una orden de producción terminada desde el menú Acciones > Volver a abrir. Pero ojo: solo se puede hacer una vez.

![Orden de producción terminada con la acción "Volver a abrir" resaltada en el menú Acciones](21-volver-a-abrir-orden-produccion.png){: w="1354" h="839" .shadow }
*Acción "Volver a abrir" en una orden de producción terminada*

## Reflexión final

Todas las empresas compran y venden, y algunas transforman; eso vale para cualquier empresa del mundo, por especial que se crea. Ni tú ni yo somos cirujanos ni físicos cuánticos: estamos contabilizando asientos, facturas y abonos con los mismos flujos y las mismas herramientas que usan miles de empresas —hoy en día, más de 50.000 solo en BC SaaS—. Por experiencia, es muy importante recalcar a los clientes que utilicen las herramientas que nos da Business Central para minimizar los errores del día a día: cambiar la fecha de trabajo, hacer una vista previa a tiempo o dedicar un poco de tiempo a configurar y mantener los periodos de fechas en los que podemos registrar evita muchos disgustos.
