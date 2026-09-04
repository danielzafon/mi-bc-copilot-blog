---
title: "Registros recientes en el selector de Business Central v29: menos clics para elegir artículo, cliente o cuenta"
date: 2026-09-07 10:00:00 +0200
categories: [Business Central, Funcional]
tags: [business-central, v29, productividad, ux, novedades]
description: "Una novedad pequeña de la preview de Business Central v29; el desplegable de selección de un registro ahora puede quedarse mostrando los usados recientemente en lugar de volver siempre al listado completo."
image:
  path: 01-recent-boton-lookup.png
  alt: Línea de un pedido de venta con el botón Recientes resaltado en el desplegable de selección de artículo
media_subpath: /assets/img/posts/seleccion-registros-recientes-business-central-v29/
---

Explorando las novedades que llegan con la versión 29 de Business Central, ahora que la preview pública ya está disponible, me he encontrado con una funcionalidad pequeña que va a ahorrar bastantes clics en el día a día: cuando vas a seleccionar un registro desde un campo de búsqueda —por ejemplo un artículo en una línea de pedido de venta—, el desplegable te ofrece la opción de quedarte viendo solo los registros que has usado recientemente, y se mantiene ahí mientras no le pidas explícitamente el listado completo.

## El problema de siempre: catálogos grandes, mismos registros repetidos

En cualquier implantación con un catálogo mínimamente grande —artículos, clientes, cuentas contables— seleccionar un registro en una línea implica casi siempre lo mismo: escribir parte del código o la descripción para filtrar, o abrir el listado completo y buscar entre cientos o miles de filas. Y sin embargo, en la práctica, durante una misma sesión de trabajo se repiten muy pocos registros: los artículos de un pedido habitual, los mismos clientes recurrentes, las cuentas que usas en cada cierre. El desplegable no tenía memoria de eso, así que cada selección partía de cero.

## La novedad: una opción Recientes en el propio desplegable

Al abrir el desplegable de selección de un campo —lo he probado en las líneas de un pedido de venta, seleccionando artículo— ves el listado completo, con la opción **Recientes** disponible junto a **Nuevo**.

![Línea de un pedido de venta con el botón Recientes resaltado en el desplegable de selección de artículo](01-recent-boton-lookup.png){: w="1453" h="853" .shadow }
*El desplegable de selección de artículo, con la opción Recientes disponible junto al listado completo.*

Al pulsar Recientes, ese mismo panel se queda mostrando solo los últimos artículos que has utilizado.

![Desplegable de selección de artículo mostrando los artículos usados recientemente tras pulsar Recientes](02-recent-resultado-lineas.png){: w="1729" h="837" .shadow }
*Tras pulsar Recientes, se muestran los últimos artículos utilizados.*

Y aquí está lo interesante: el desplegable **guarda esa selección**. No vuelve al listado completo por defecto la próxima vez que abras el campo; se queda en Recientes hasta que tú mismo pulses **Buscar** para pedir explícitamente el listado completo de nuevo.

![Botón Buscar resaltado en el desplegable de selección de artículo para volver al listado completo](03-search-volver-listado.png){: w="1729" h="837" .shadow }
*El botón Buscar es el que hay que pulsar para volver al listado completo; el desplegable no lo hace por sí solo.*

> Todavía no tengo claro si "recientes" se calcula por usuario, por tabla o por combinación de campo y tabla, ni cuántos registros guarda. Lo iré confirmando a medida que avance la preview.
{: .prompt-tip }

## Por qué importa en el día a día

Es una funcionalidad pequeña, pero encaja bien con cómo se trabaja realmente en Business Central: la mayoría de sesiones de captura —pedidos, facturas, diarios— se repiten sobre un conjunto reducido de registros. Que el desplegable se quede en Recientes línea tras línea, en lugar de volver al listado completo cada vez, se nota especialmente en:

- **Captura de pedidos recurrentes**, donde se repiten los mismos artículos o clientes de un pedido a otro.
- **Catálogos grandes**, donde abrir el listado completo tiene un coste de carga y de scroll nada despreciable.
- **Usuarios de negocio** que no memorizan códigos y hasta ahora dependían de escribir bien la descripción para encontrar lo que buscaban.

## Conclusión y recomendación

Como toda funcionalidad en preview, conviene probarla en un entorno de test antes de dar por sentado su comportamiento final. Pero si tu proyecto tiene captura intensiva de líneas —pedidos, diarios, presupuestos— merece la pena tenerla en el radar cuando v29 llegue a producción, porque es de esas mejoras que no cambian ningún proceso pero sí el número de clics que hacen tus usuarios cada día.
