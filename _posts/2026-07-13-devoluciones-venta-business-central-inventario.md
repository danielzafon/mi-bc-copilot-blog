---
title: "Devoluciones de venta en Business Central: por qué desvirtúan tu inventario y cómo evitarlo"
date: 2026-07-13 10:00:00 +0200
categories: [Business Central]
tags: [business-central, ventas, inventario, devoluciones]
description: Cómo crear devoluciones de venta en Business Central sin desvirtuar la valoración de inventario; la clave está en revertir líneas registradas en lugar de copiarlas o crearlas a mano.
image:
  path: 03-revertir-lineas-docs-registrados.png
  alt: Acción "Revertir líneas doc.-tos registrados" en un documento de devolución de venta de Business Central
media_subpath: /assets/img/posts/devoluciones-venta-business-central-inventario/
---

El otro día estuve repasando con un cliente los flujos de devolución en Business Central, y lo comparto por aquí porque es uno de los quebraderos de cabeza que tienen —y tenemos— con casi todos los clientes. No es un problema de la herramienta, sino de cómo se construyen las líneas de la devolución, y esa decisión tiene efecto directo en la valoración del inventario.

## Dos formas de crear una devolución, y solo una es segura

La primera opción, y la más habitual cuando alguien no conoce el proceso, es crear la devolución como si fuera un pedido cualquiera, introduciendo las líneas manualmente. El problema es que para el sistema esto es un documento distinto al original, así que el coste del producto que aplica también es distinto. A la larga, eso se traduce en diferencias en la valoración del inventario.

La segunda opción es usar **Copiar líneas...** para traer las líneas del pedido, albarán o factura original.

![Acción "Copiar líneas..." en un documento de devolución de venta](01-copiar-lineas.png){: w="1373" h="816" .shadow }
*Acción "Copiar líneas..." en un documento de devolución de venta*

**Copiar líneas** es mejor que teclearlas a mano, pero no resuelve el problema por sí sola. En ambos casos, salvo que tengamos marcado en la configuración de ventas (y lo mismo en compras) que se debe indicar en las líneas contra qué movimiento de producto se liquida el coste, la valoración se puede ver desvirtuada igualmente.

![Parámetro "Coste exacto devol. obligatorio" en la configuración de ventas y cobros](02-config-ventas-coste-exacto.png){: w="1710" h="794" .shadow }
*Parámetro "Coste exacto devol. obligatorio" en la configuración de ventas y cobros*

## La forma recomendada: revertir líneas de documentos registrados

La forma que recomiendo para crear las líneas de una devolución es mediante la acción **Revertir líneas doc.-tos registrados**.

![Acción "Revertir líneas doc.-tos registrados" en el documento de devolución](03-revertir-lineas-docs-registrados.png){: w="1372" h="816" .shadow }
*Acción "Revertir líneas doc.-tos registrados" en el documento de devolución*

En la ventana que se abre, activamos **Mostrar solo líneas reversibles** para que muestre únicamente las líneas de documentos con cantidad no devuelta, y aplicamos el filtro de tipo de documento que queremos revertir (normalmente envíos o facturas registradas).

![Ventana de selección con "Mostrar solo líneas reversibles" activado y filtro de tipo de documento](04-mostrar-solo-lineas-reversibles.png){: w="1841" h="789" .shadow }
*Ventana de selección con "Mostrar solo líneas reversibles" activado y filtro de tipo de documento*

Al seleccionar las líneas y pulsar Aceptar, se insertan en nuestra devolución. Si mostramos la columna **Liquid.-de mov. pdto.**, veremos que va a liquidar contra la salida original, recuperando su coste real.

![Columna "Liquid.-de mov. pdto." mostrando la liquidación contra el movimiento de salida original](05-liquidacion-mov-pdto.png){: w="1337" h="734" .shadow }
*Columna "Liquid.-de mov. pdto." mostrando la liquidación contra el movimiento de salida original*

## Un dato pequeño que aporta mucho: el motivo de la devolución

También puede resultar interesante codificar e indicar los diferentes motivos de devolución (dañado, color incorrecto, producto equivocado, tamaño incorrecto...) para poder llevar un registro y analizar las causas.

![Selector de código de auditoría de devolución con motivos como dañado, color o tamaño incorrecto](06-codigo-motivo-devolucion.png){: w="1358" h="854" .shadow }
*Selector de código de auditoría de devolución con motivos como dañado, color o tamaño incorrecto*

Es un dato que cuesta muy poco capturar en el momento y que luego da mucho juego para detectar patrones: si un mismo producto o proveedor concentra devoluciones por el mismo motivo, ahí hay algo que merece la pena revisar.

## Documentos relacionados: reposición y devolución al proveedor, con matices

Otra funcionalidad interesante es la posibilidad de crear documentos relacionados con nuestra devolución, como un pedido de venta para reponer el material al cliente, o un pedido de compra o devolución al proveedor.

![Acción "Crear docs. relacionados-dev..." en el documento de devolución](07-crear-docs-relacionados-dev.png){: w="1411" h="843" .shadow }
*Acción "Crear docs. relacionados-dev..." en el documento de devolución*

Esta funcionalidad tiene bastantes limitaciones, ya que creará un único pedido o devolución para el proveedor que indiquemos. Si los productos de la devolución son de distintos proveedores, no la podremos usar tal cual.

![Ventana de opciones para crear devolución a proveedor, pedido de compra y pedido de venta de reposición](08-opciones-crear-devdoc-relacionados.png){: w="1440" h="824" .shadow }
*Ventana de opciones para crear devolución a proveedor, pedido de compra y pedido de venta de reposición*

Al finalizar, se abre un listado con los documentos que ha creado en función de lo que hayamos indicado.

![Listado de documentos relacionados creados a partir de la devolución](09-listado-documentos-relacionados.png){: w="1747" h="276" .shadow }
*Listado de documentos relacionados creados a partir de la devolución*

## Por qué esto importa: contablemente, una devolución es una compra

Algo muy importante que hay que tener en cuenta es que, en Business Central, una devolución de venta genera una entrada de inventario, por lo que para la valoración del inventario es equivalente a una compra.

Por mi experiencia, las devoluciones de compras y ventas suelen ser uno de los principales motivos de diferencias y problemas de los clientes con la variación de existencias. Generalmente me encuentro con clientes que no facturan (es decir, no registran el abono) buena parte de las devoluciones, tanto de ventas como de compras.

Salvo que esté configurado para tenerlo en cuenta desde los albaranes, la valoración de inventario considera las compras y ventas de material **facturado**. Así que si dejamos las devoluciones de venta sin registrar el abono —aunque sea con importe 0 para el cliente—, esas existencias no se tendrán en cuenta en el valor de inventario. Lo mismo ocurre con las devoluciones de compra: si no registramos el abono, el material no se descontará de la valoración.

> Si dejas una devolución sin facturar el abono, aunque sea a importe cero, esas existencias no entrarán en tu valoración de inventario.
{: .prompt-warning }

## Una reflexión final

Mi recomendación: si trabajas con devoluciones en Business Central, quédate con dos ideas. Construye siempre las líneas con **Revertir líneas doc.-tos registrados** en lugar de copiarlas o teclearlas a mano, y no des por cerrada una devolución hasta que el abono esté registrado, aunque el importe sea cero. Es la única forma de que el inventario refleje la realidad.
