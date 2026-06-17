---
title: Funcionalidades que pasan desapercibidas en la configuración de compras y ventas de Business Central
date: 2026-06-17 09:00:00 +0200
categories: [Business Central, Funcional]
tags: [business-central, compras, ventas, configuración, productividad]
description: Repaso práctico de los campos y opciones de la conf. de compras y ventas que muchos equipos no han tocado nunca — y que pueden ahorrar tiempo real en el día a día.
media_subpath: /img/post/2026-06-17-funcionalidades-ocultas-configuracion-compras-ventas/
image:
  path: configuracion-compras-ventas.png
  alt: Configuración de compras y pagos en Business Central
---

Hay páginas en Business Central que todo el mundo ha abierto, pero pocos han leído de verdad. La **Conf. compras y pagos** y la **Conf. ventas y cobros** son dos de ellas. Se tocan al principio del proyecto, se marcan cuatro opciones obvias y se cierran. Lo que queda debajo —campo a campo, opción a opción— suele quedar sin revisar.

A esto se suma que Microsoft va incorporando pequeñas funcionalidades en cada versión que suelen pasar completamente desapercibidas —y que muchas veces descubrimos de casualidad, buscando otra cosa. Este post repasa las que más me he encontrado mal configuradas o directamente sin tocar, y que en mi experiencia tienen impacto directo en cómo trabaja el equipo cada día.

---

## Compras y pagos

### Cantidad de cuenta de contabilidad predeterminada

Al añadir una línea de tipo *Cuenta* en un documento de compra, BC propone automáticamente la cantidad 1. En empresas donde la mayoría de compras son servicios o gastos —y por tanto trabajan con cuentas contables en lugar de artículos— evita introducir la cantidad en cada línea. Pequeño, pero se nota.

### Copiar el nombre del proveedor en los movimientos

Cuando está activo, aparece la columna **Nombre proveedor** en los movimientos de proveedor, con el nombre grabado en el momento del registro. Sin esta opción, esa columna no está visible. Tenerla facilita los informes y búsquedas directamente desde la lista de movimientos, sin tener que navegar a la ficha del proveedor.

### Mismo N.º doc. externo en diferente ejercicio

BC bloquea por defecto registrar dos facturas del mismo proveedor con el mismo número de documento externo —lógico para evitar duplicados—. Pero hay proveedores que reutilizan numeración entre ejercicios. Esta opción permite esa situación sin desactivar el control global.

### Cantidad predeterminada para recepción

Con valor *Restante*, al registrar una recepción BC propone la cantidad pendiente de recibir. Parece obvio, pero el valor por defecto en muchas instalaciones es *En blanco*, lo que obliga al usuario a introducir la cantidad manualmente en cada recepción. Un cambio de un minuto con impacto en cientos de registros al año.

### Comprobación de la fecha de registro al registrar

Si la fecha de registro del documento es distinta de la fecha de trabajo, BC muestra una advertencia antes de continuar. No bloquea el registro, pero obliga a confirmar que la fecha es intencionada. Evita que se cuelen fechas incorrectas por despiste, algo más habitual de lo que parece cuando se trabaja con documentos de días anteriores.

### Registrar automáticamente no exist. a través de alm.

Permite registrar al mismo tiempo artículos, cargos de producto y otros conceptos que no pasan por documentos de almacén. El caso más habitual es el de los portes: si trabajas con documentos de almacén y quieres registrar los gastos de envío en el mismo paso que el albarán, esta opción lo hace posible sin tener que volver al pedido para completar el registro.

### Copiar descripción de línea a movimientos de contabilidad

La descripción que escribes en la línea del pedido o la factura llega a los movimientos contables. Cuando alguien revisa un extracto de cuenta o hace una auditoría, el contexto está ahí directamente. Sin esto, el apunte queda con el texto de registro —el típico *Factura XXXX*— que no dice nada sobre qué se compró.

### Copiar N.º de factura en referencia de pago

Al incluir el número de factura en la referencia de pago, la conciliación bancaria automática tiene más datos para trabajar. Si usáis pagos electrónicos o importáis extractos bancarios, activar esto desde el principio evita trabajo manual posterior.

### Tipo de línea predeterminado en documentos

Define qué tipo aparece por defecto al abrir una nueva línea en pedidos y facturas de compra. En la mayoría de implantaciones tiene sentido dejarlo en *Artículo*, pero si el volumen de compras es principalmente de servicios o gastos contra cuenta contable, cambiar el predeterminado evita un clic extra en cada línea.

---

## Ventas y cobros

![Configuración de ventas y cobros en Business Central](configuracion-ventas.png){: .shadow }

### Advertencia de crédito

Define qué situación del cliente dispara una advertencia al introducir documentos de venta. Con valor *Ambos*, BC avisa tanto si el cliente tiene el límite de crédito superado como si tiene saldo vencido pendiente. También puedes configurarlo para que avise solo por crédito, solo por vencidos, o nunca. Dejarlo en *Nunca* —que es lo que me encuentro con más frecuencia— hace que el equipo comercial trabaje sin ninguna señal sobre la situación financiera del cliente.

### Advertencia de falta de stock

Cuando está activo, BC avisa si la cantidad solicitada supera el stock disponible. No sustituye una gestión de reservas seria, pero evita comprometer cantidades que no se pueden servir. En entornos con almacén básico y sin reservas, es la única red de seguridad que tiene el comercial.

### Cantidad de producto predeterminada

Al añadir una línea de tipo *Artículo* en un documento de venta, BC propone automáticamente la cantidad 1. El equivalente a la opción de compras para artículos.

### Cantidad de cuenta de contabilidad predeterminada

Lo mismo para líneas de tipo *Cuenta*: propone cantidad 1 por defecto. Útil cuando se facturan servicios o conceptos directamente contra cuenta contable.

### Copiar el nombre del cliente en movimientos

Igual que en compras con los proveedores: cuando está activo, aparece la columna **Nombre cliente** en los movimientos de cliente con el nombre grabado en el momento del registro. Sin esta opción, esa columna no está visible.

### Fecha de registro predeterminada

Por defecto usa la fecha de trabajo, pero puede dejarse en blanco. Con valor en blanco, el campo queda vacío en cada nuevo documento y BC obliga al usuario a indicar una fecha antes de registrar. Útil cuando se quiere asegurar que nadie registra con una fecha incorrecta por despiste.

### Cantidad predeterminada para enviar

El mismo criterio que en compras para la recepción: con *Restante*, BC propone la cantidad pendiente de enviar al registrar un albarán. Reduce el margen de error en almacenes con mucho movimiento.

### Registrar automáticamente no exist. a través de alm.

El mismo comportamiento que en compras: permite registrar al mismo tiempo cargos de producto y otros conceptos que no pasan por documentos de almacén. En ventas el caso típico son también los portes de venta, que se pueden registrar en el mismo paso que el albarán de salida.

### Comprobación de la fecha de registro al registrar

El mismo comportamiento que en compras: si la fecha de registro no coincide con la fecha de trabajo, BC muestra una advertencia. Si lo activas en compras, actívalo también aquí.

### Copiar descripción de línea a movimientos de contabilidad

Igual que en compras. Si el proceso de facturación incluye descripciones específicas por línea —referencias de cliente, proyectos, obras— esta opción hace que esa información llegue al libro mayor en lugar del texto genérico del registro.

### Tipo de línea predeterminado en documentos

Si los comerciales trabajan principalmente con artículos —que es lo habitual—, dejarlo en *Artículo* evita que tengan que seleccionarlo en cada línea nueva.

### Vincular fecha de documento a fecha de registro

Cuando está activo, al cambiar la fecha de registro en un documento de venta, BC actualiza automáticamente la fecha de documento para que coincidan. Evita divergencias entre ambas fechas que luego generan preguntas en auditoría o en los informes de antigüedad.

---

## Una reflexión final

Ninguna de estas opciones requiere desarrollo. Están en estándar, esperando a que alguien las revise. El problema es que en la mayoría de proyectos la configuración inicial se hace con prisa —con el go-live encima— y estas pantallas se quedan a medio explorar.

Mi recomendación: dedica una sesión específica a recorrer estas dos páginas con el cliente, opción por opción. Una hora bien invertida que puede cambiar cómo trabajan decenas de personas cada día.

> **Tip:** Si no tienes claro el efecto de una opción, actívala en el entorno de pruebas, crea un documento y regístralo. BC es bastante transparente en cómo se comporta.
{: .prompt-tip }
