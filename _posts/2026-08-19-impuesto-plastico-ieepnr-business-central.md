---
title: "Impuesto al plástico (IEEPNR) en Business Central: hasta dónde llega el estándar"
date: 2026-08-19 10:00:00 +0200
categories: [Business Central, Finanzas]
tags: [business-central, impuesto-plastico, ieepnr, modelo-592, fiscalidad, compras]
description: Configuración paso a paso del impuesto al plástico (IEEPNR) en Business Central y análisis de qué necesidades del Libro Registro de existencias se quedan fuera del estándar.
image:
  path: 01-configurar-tipo-impuesto.png
  alt: Ficha de tipo de impuesto especial sobre el consumo "PLASTICO" en Business Central
media_subpath: /assets/img/posts/impuesto-plastico-ieepnr-business-central/
---

El impuesto que todo el mundo llama coloquialmente "impuesto al plástico" tiene un nombre oficial menos manejable: **IEEPNR**, Impuesto Especial sobre los Envases de Plástico No Reutilizables. Es regulación española, recogida en la Ley 7/2022, y no todos los que tienen relación con este impuesto están obligados de la misma forma: quién presenta qué depende de si tu cliente es fabricante, adquirente intracomunitario o importador.

## Modelo 592 y Libro Registro no son lo mismo

Es habitual encontrarse con la idea de que "el impuesto al plástico" es una única gestión. En realidad son dos piezas con finalidades distintas, y no todos los sujetos pasivos tienen ambas obligaciones:

- Los **fabricantes en España** deben presentar el Modelo 592, incluso en los periodos en los que no resulte cuota a ingresar.
- Los **adquirentes intracomunitarios** también deben presentarlo, salvo que estén exceptuados de la inscripción en el Registro Territorial porque el plástico no reciclado que adquieren no supera los **5 kg en un mes natural**; en cuanto se supera ese umbral, nace la obligación.
- Los **importadores no presentan el Modelo 592** por esa actividad: el impuesto ya lo liquida la Aduana en el momento de la importación.

Y el **Libro Registro de existencias es una obligación adicional y específica de los adquirentes intracomunitarios**: ni los fabricantes ni los importadores lo llevan.

> La pregunta clave para dimensionar el proyecto es siempre la misma: ¿tu cliente realiza adquisiciones intracomunitarias de envases o productos sujetos al impuesto, por encima de 5 kg de plástico no reciclado al mes? Si la respuesta es sí, va a necesitar el Modelo 592 y el Libro Registro. Si solo compra a proveedores españoles que ya repercuten el impuesto, normalmente no actúa como contribuyente por esas operaciones.
{: .prompt-tip }

| Concepto | Modelo 592 | Libro Registro de existencias (adquirentes intracomunitarios) |
| --- | --- | --- |
| A quién obliga | Fabricantes (siempre) y adquirentes intracomunitarios (salvo exención por debajo de 5 kg/mes); los importadores no lo presentan | Solo a los adquirentes intracomunitarios de envases de plástico |
| Finalidad | Liquidar y pagar el impuesto; es la autoliquidación tributaria | Justificar y controlar los movimientos de los productos sujetos al impuesto |
| Qué contiene | Kilogramos de plástico no reciclado, cuotas devengadas, deducciones, compensaciones y cuota a ingresar o devolver | Entradas, salidas y existencias de los productos sujetos al impuesto |
| Presentación | Modelo tributario 592 | Fichero/libro electrónico presentado en la Sede de la AEAT |
| Objetivo para Hacienda | Determinar cuánto impuesto debes ingresar | Verificar que los datos declarados en el impuesto cuadran con las operaciones reales |
| Relación entre ambos | Se calcula a partir de la información de las operaciones | Cuando aplica, es la trazabilidad detallada que soporta y justifica el 592 |

En resumen: para un adquirente intracomunitario, el Libro Registro es el detalle operativo y el Modelo 592 es la declaración fiscal resultante. Si estás analizando un desarrollo o una implantación que toque este impuesto, lo primero es identificar si tu cliente tiene esa condición, porque el Libro Registro suele ser la parte más compleja: exige mantener toda la trazabilidad que después respalda los importes declarados en el 592.

## Un ejemplo para verlo claro

Supongamos que una empresa, adquirente intracomunitaria de envases de plástico, compra durante el trimestre 10.000 kg de envases con plástico, de los cuales 2.000 kg son plástico reciclado no reciclable y 8.000 kg son plástico no reciclado.

El **Libro Registro** refleja cada adquisición: proveedor, fecha, kilos, movimientos y existencias resultantes. El **Modelo 592**, en cambio, solo utiliza la parte de plástico no reciclado (los 8.000 kg) para calcular la cuota tributaria y presentar la autoliquidación.

Trasladado a Business Central, y para el cliente que sea adquirente intracomunitario, esto se traduce en dos necesidades de información distintas a partir del mismo origen de datos:

1. El Libro Registro requiere **trazabilidad transaccional**: entradas, salidas, stock y kilogramos de plástico por artículo o lote.
2. El Modelo 592 requiere **información agregada** para el periodo fiscal, mensual o trimestral según corresponda.

Lo habitual es generar ambos a partir de la misma información maestra: la ficha de artículo, el peso de plástico, el porcentaje reciclado, las compras intracomunitarias y los movimientos de almacén.

## Cómo se configura el impuesto en Business Central

He estado configurando y probando esta funcionalidad para hacer una demo a un cliente, precisamente para ver cómo funciona en la práctica y si el estándar cubre sus necesidades mínimas. Vamos al paso a paso: cómo dar de alta el impuesto y calcularlo sobre un artículo real.

Lo primero es crear el tipo de impuesto especial sobre el consumo. Le he puesto el código `PLASTICO` y como base imponible uso **Cantidad**.

![Ficha de tipo de impuesto especial sobre el consumo "PLASTICO" con el botón "Configurar tipos de entrada"](01-configurar-tipo-impuesto.png){: w="1347" h="527" .shadow }
*Ficha de tipo de impuesto especial sobre el consumo "PLASTICO", con acceso a "Configurar tipos de entrada"*

Desde ahí, configuras qué tipos de entrada van a generar el impuesto. BC permite compras, ventas, ajuste positivo, ajuste negativo, salida de producción y salida de ensamblado. En este caso he activado solo compras y ventas.

![Tipos de entrada configurados como permitidos: Compra y Ventas](02-tipos-entrada-permitidos.png){: w="1397" h="400" .shadow }
*Tipos de entrada permitidos para el impuesto "PLASTICO": Compra y Ventas*

También hay que configurar la tasa del impuesto, que actualmente es de **0,45 € por kilo de plástico no reciclable**, vigente desde el 1 de enero de 2026.

![Tasa de impuesto específico de 0,45 vigente a partir del 01/01/2026](03-tasa-impuesto-045.png){: w="1375" h="385" .shadow }
*Tasa de impuesto específico sobre productos o servicios: 0,45 €, vigente a partir del 01/01/2026*

Y por último, en la ficha del artículo indicas el tipo de impuesto y el peso de plástico no reciclable correspondiente al envase.

![Ficha de producto con el tipo de impuesto "PLASTICO" y la cantidad de 0,85 kg para el impuesto especial](04-ficha-producto-peso-plastico.png){: w="1648" h="852" .shadow }
*Ficha de producto: tipo de impuesto "PLASTICO" y 0,85 kg de plástico no reciclable por unidad*

> La tasa tiene fecha de vigencia, así que si trabajas con clientes que ya venían aplicando el impuesto en ejercicios anteriores, revisa que la tasa configurada corresponde al periodo que estás liquidando.
{: .prompt-tip }

## Registrar el impuesto: el diario de consumo

Para calcular y registrar el impuesto hay que crear una sección en el nuevo **diario de consumo**. El nombre no ayuda mucho, porque se puede confundir fácilmente con el diario de consumo**s** que usamos en producción; son cosas distintas.

![Sección de diario "PLASTICO" con filtro de tipo de impuesto especial sobre el consumo](05-seccion-diario-consumo.png){: w="1829" h="284" .shadow }
*Sección de diario "PLASTICO", con el filtro de tipo de impuesto especial sobre el consumo*

En el diario, calculas el impuesto con la acción **Generar entradas de impuestos especiales sobre el consumo**.

![Acción "Generar entradas de...s sobre el consumo" en el diario de consumo](06-generar-entradas-consumo.png){: w="1833" h="447" .shadow }
*Acción para generar las entradas de impuesto sobre el consumo en el diario*

El sistema te pide la fecha de registro, el rango de fechas y el impuesto sobre el que quieres calcular.

![Ventana "Generar entradas del diario de impuestos especiales" con fecha de registro, rango de fechas y filtro de tipo PLASTICO](07-filtros-generar-entradas.png){: w="808" h="824" .shadow }
*Filtros para generar las entradas: fecha de registro, rango de fechas y tipo de impuesto*

A partir de la información que has ido registrando —compras y ventas, en este caso— te propone el diario para que lo registres. Para el producto configurado, se ha comprado 20 unidades y, como en su ficha cada unidad tiene 0,85 kg de plástico no reciclable, el cálculo es: 20 unidades × 0,85 kg × 0,45 € el kilo = **7,65 € de impuesto**.

![Líneas del diario de consumo generadas, con el cálculo de 7,65 € de impuesto para 20 unidades](08-diario-consumo-lineas-generadas.png){: w="1800" h="491" .shadow }
*Diario de consumo con las líneas generadas: 20 unidades × 0,85 kg × 0,45 € = 7,65 €*

Una vez registrado, puedes consultar la información en la página **Registro de transacciones de impuesto de consumo**. Una cosa interesante —y que está oculta por defecto— es que arrastra el número de movimiento del producto que originó cada línea. Juntando ambas fuentes obtienes bastante más información de la que se ve a simple vista.

![Registro de transacciones de impuesto de consumo con la columna "N.º mov. producto" resaltada](09-registro-transacciones-mov-producto.png){: w="1805" h="430" .shadow }
*Registro de transacciones de impuesto de consumo, con la columna "N.º mov. producto" que lo relaciona con el movimiento de inventario origen*

## Lo que Business Central no resuelve

Hasta aquí, la parte que Business Central cubre razonablemente bien: dar de alta el impuesto, calcularlo sobre compras y ventas y consultar el resultado. Pero hay tres frentes donde se queda corto: el Libro Registro propiamente dicho, una limitación de la propia ficha de producto y la repercusión del impuesto al cliente.

### Limitaciones del Libro Registro de existencias

El problema aparece cuando miras específicamente lo que necesitas para el **Libro Registro de existencias de los adquirentes intracomunitarios**, que es donde de verdad está la complejidad de este impuesto:

- No se pueden configurar las claves del régimen fiscal correspondiente.
- No se puede restringir para que el libro se genere solo con proveedores de la UE.
- Si tienes productos con plástico que no es 100 % reciclable, no hay forma de obtener el total de kilos de plástico por esa vía, un dato que sin embargo se debe reportar.
- El libro exige indicar el albarán como justificante, pero en los registros no aparece directamente; puedes recuperarlo a través del movimiento de producto relacionado, aunque no es inmediato.
- No se pueden configurar excepciones ni deducciones.

### Un único impuesto especial por producto

Esto ya no es específico del impuesto al plástico, sino de cómo está montada la funcionalidad de impuestos especiales sobre el consumo en general: está pensada para poder gestionar varios impuestos distintos —el propio release plan de Microsoft habla de plástico y azúcar dentro de la misma funcionalidad—, pero a nivel de ficha de producto solo puedes indicar **un** tipo de impuesto especial. Si un mismo producto estuviera sujeto a dos impuestos especiales sobre el consumo distintos, el estándar no te deja configurarlo.

Y esta limitación tiene una consecuencia práctica sobre el propio problema del plástico parcialmente reciclado que comentaba antes: se podría pensar en sortear ese hueco configurando dos impuestos sobre el mismo producto, uno para el total de kilos de plástico y otro para la parte no reciclada, y así obtener directamente el dato que el Libro Registro necesita. Pero como solo se puede indicar un único tipo de impuesto especial por producto, tampoco esta vía es viable.

### Repercutir el impuesto al cliente: otro frente sin cubrir

Aquí el problema ya no es el Libro Registro, sino la operativa comercial: Business Central no ofrece ninguna forma de repercutir el impuesto a los clientes en las facturas. Es una carencia que afecta a cualquier empresa sujeta al IEEPNR —adquirente intracomunitaria o no— que necesite trasladar ese coste, y que hay que cubrir por fuera de lo que trae de serie el impuesto especial sobre el consumo.

## Conclusión y recomendación

En mi opinión, tal como está hoy, el estándar no cumple un mínimo para gestionar este impuesto de forma medianamente ágil en Business Central. Sirve para calcular la cuota sobre compras y ventas y tener un primer registro de movimientos, pero en cuanto el cliente necesita entregar el Libro Registro completo —régimen fiscal, filtrado por proveedores UE, porcentajes de reciclado parciales—, gestionar más de un impuesto especial sobre el mismo producto o repercutir el impuesto en factura, vas a necesitar trabajo adicional, ya sea configuración avanzada, desarrollo a medida o un proceso externo al ERP para cerrar esa parte.

Si estás dimensionando un proyecto que incluya este impuesto, mi recomendación es separar bien el alcance desde el principio: una cosa es dar de alta el impuesto y calcularlo sobre movimientos de compra y venta, otra es entregar un Libro Registro completo y defendible ante Hacienda, y otra distinta es poder repercutirlo al cliente en factura. Ninguna de las dos últimas viene resuelta de serie hoy por hoy.

> **Documentación de Microsoft**
> - [Set up excise taxes](https://learn.microsoft.com/es-es/dynamics365/business-central/finance-set-up-excise-tax)
> - Ficha del release plan: [Calculate taxes for plastic and sugar](https://learn.microsoft.com/es-es/dynamics365/release-plan/2026wave1/smb/dynamics365-business-central/calculate-taxes-plastic-sugar)
{: .prompt-info }
