---
title: "Copilot en Business Central: lo que ya tienes y no te cuesta nada más"
date: 2026-07-02 10:00:00 +0200
categories: [Business Central, Copilot]
tags: [business-central, copilot, ia, productividad]
description: Recorrido práctico por las funcionalidades de Copilot que hoy vienen incluidas en Business Central sin coste adicional; qué aportan de verdad, cuándo merecen la pena y cómo enseñárselas a tus clientes.
image:
  path: 01-portada.png
  alt: Ilustración de Business Central y Copilot; panel de datos de un ERP junto a un emblema de asistente de IA
media_subpath: /assets/img/posts/funcionalidades-copilot-business-central-sin-coste/
---

Últimamente no se habla de otra cosa: el modelo de pago por uso de la IA en Business Central, los créditos, los agentes, cuánto va a costar todo esto. Y está bien tenerlo en el radar. Pero en esa conversación se nos olvida algo importante: **una buena parte de las funcionalidades de Copilot que hoy tenemos en Business Central no cuestan nada más**. Vienen incluidas y, al menos a día de hoy, no consumen ningún crédito adicional.

Mi experiencia con clientes es que la mayoría ni las conoce, y quien las conoce no las usa. Antes de entrar en la parte de pago —los agentes, que dejo para otro artículo— merece la pena hacer inventario de lo que ya está ahí, encendido y disponible. En este recorrido las repaso una a una, con capturas reales y, sobre todo, con mi opinión sobre cuándo aportan valor de verdad y cuándo son más bien humo de demo.

> Si quieres la referencia oficial y actualizada de todo lo que Copilot ofrece en Business Central, el punto de partida es la documentación de Microsoft: [IA en Business Central](https://learn.microsoft.com/es-es/dynamics365/business-central/ai-in-bc).
{: .prompt-info }

> «Sin coste adicional» es cierto **a día de hoy**. El licenciamiento de la IA en el producto se mueve rápido, así que conviene revisar periódicamente qué sigue incluido y qué pasa a consumir créditos.
{: .prompt-warning }

## Chatear con Copilot

El chat es la puerta de entrada. Con él, sin salir de Business Central, un usuario puede:

- Pedir ayuda sobre cómo hacer algo («¿cómo creo un pedido de venta?») y recibir instrucciones paso a paso.
- Localizar registros y datos, y navegar directamente a la página o acción que necesita.
- Pedir que le expliquen un concepto, un campo o una funcionalidad del producto.

En la [documentación de chat con Copilot](https://learn.microsoft.com/es-es/dynamics365/business-central/chat-with-copilot) hay consejos para afinar las preguntas y sacarle más partido.

Además de todo lo anterior, hay algo que suele sorprender en las demos: **puede responder preguntas sobre nuestros propios datos**. Por ejemplo, cuál es el cliente con mayor saldo.

![Chat de Copilot en Business Central respondiendo a la pregunta sobre el cliente con mayor saldo](02-chat-copilot-saldo-cliente.png){: w="1879" h="875" .shadow }
*Copilot resuelve consultas sobre los datos de la empresa desde el propio panel de chat.*

## Sugerencias de textos de marketing

Fue la primera funcionalidad de IA que tuvimos disponible en Business Central. Genera descripciones de marketing para los productos a partir de sus atributos e información. Tienes el detalle en la [visión general de IA en Business Central](https://learn.microsoft.com/es-es/dynamics365/business-central/ai-overview).

![Generación de un texto de marketing para un producto con Copilot](03-textos-marketing.png){: w="1421" h="842" .shadow }

![Texto de marketing propuesto por Copilot para el producto](04-textos-marketing-resultado.png){: w="1459" h="785" .shadow }

Queda muy bien en las demos, pero seamos honestos: **salvo que el cliente tenga sus productos con muchos atributos e información bien completada, no aporta gran cosa**. De todos modos, si se trabaja con e-commerce puede ser interesante para completar las descripciones de catálogo con poco esfuerzo.

## Autocompletar con Copilot

Esta sí me parece práctica en el día a día. Sirve para **completar los datos de un registro con información disponible en internet**. El caso típico: dar de alta un cliente. Para más detalle, la [documentación de autocompletar campos](https://learn.microsoft.com/es-es/dynamics365/business-central/autofill-fields-with-copilot).

Por ejemplo, al dar de alta el cliente *Amazon EU S.à r.l., Sucursal en España*, la funcionalidad me propone la dirección y el CIF (eso sí, hay que tener un poco de paciencia).

![Copilot proponiendo dirección y CIF al dar de alta un cliente](05-autocompletar-amazon.png){: w="1316" h="790" .shadow }

Lo importante es que **mantienes el control**: puedo revisar cada sugerencia y aceptarla o descartarla antes de que toque nada.

![Revisión de las sugerencias de autocompletar antes de aceptarlas](06-autocompletar-revisar.png){: w="1718" h="854" .shadow }

## Conciliar con Copilot

La conciliación bancaria asistida por Copilot va un paso más allá de la automática. Lo tienes documentado en [conciliación bancaria con Copilot](https://learn.microsoft.com/es-es/dynamics365/business-central/bank-reconciliation-with-copilot).

![Conciliación bancaria asistida por Copilot en Business Central](07-conciliar-copilot.png){: w="1767" h="826" .shadow }

La diferencia clave frente a la conciliación automática es que **es capaz de casar un movimiento con varios**, algo que en la vida real pasa constantemente.

![Copilot casando un movimiento bancario con varios apuntes](08-conciliar-varios-movimientos.png){: w="1811" h="843" .shadow }

También tiene una funcionalidad para **proponer la cuenta contable** cuando llevas al diario una línea con diferencia y hay que contabilizarla.

![Copilot proponiendo la cuenta contable para una línea con diferencia](09-conciliar-cuenta-diferencia.png){: w="1804" h="729" .shadow }

## Sugerir sustituciones de productos

Pensada para los clientes que trabajan con productos sustitutivos. La referencia oficial: [sugerir sustituciones de productos con Copilot](https://learn.microsoft.com/es-es/dynamics365/business-central/suggest-item-substitutions-copilot).

![Copilot sugiriendo productos sustitutivos](10-sustituciones-productos.png){: w="1808" h="432" .shadow }

Un detalle que me gusta: podemos pedirle que **nos muestre su nivel de confianza en la sugerencia y los términos de búsqueda que ha utilizado**, lo que ayuda mucho a entender por qué propone lo que propone.

![Sugerencias de sustitución con el nivel de confianza de Copilot](11-sustituciones-confianza.png){: w="1823" h="746" .shadow }

![Términos de búsqueda utilizados por Copilot para las sustituciones](12-sustituciones-terminos.png){: w="1774" h="393" .shadow }

Como con los textos de marketing, la regla es la misma: **si no tenemos los productos con suficiente información, las sugerencias no serán muy precisas**.

## Sugerir series numéricas

Copilot puede proponer series numéricas a partir de una indicación. Tienes el detalle en [sugerir series numéricas con Copilot](https://learn.microsoft.com/es-es/dynamics365/business-central/suggest-number-series-copilot).

![Copilot sugiriendo series numéricas en Business Central](13-series-numericas.png){: w="1753" h="803" .shadow }

Es útil en dos momentos concretos: para la **configuración inicial** de un cliente y, como en la imagen, para pedirle que **nos prepare las series para el próximo año** sin tener que ir una a una.

## Resumir con Copilot

Una funcionalidad interesante que hace, con IA, un resumen de los **principales hitos** de un cliente, proveedor, cuenta, etc. La tienes en [resumir con Copilot](https://learn.microsoft.com/es-es/dynamics365/business-central/summarize-with-copilot).

![Resumen generado por Copilot en la ficha de un cliente](14-resumir-ficha.png){: w="1828" h="840" .shadow }

No solo está disponible en las fichas (donde probablemente es más interesante), sino también en **documentos como los pedidos**.

![Resumen de Copilot disponible también en un pedido](15-resumir-pedido.png){: w="1461" h="829" .shadow }

## Analizar con Copilot

Si tuviera que quedarme con una, sería esta. Para mí es **probablemente la funcionalidad más útil e interesante** de todas, sobre todo para esos usuarios a los que les cuesta trabajar con el modo de análisis de datos. La referencia: [asistencia de análisis](https://learn.microsoft.com/es-es/dynamics365/business-central/analysis-assist).

![Panel de analizar con Copilot en Business Central](16-analizar-copilot.png){: w="1593" h="727" .shadow }

En mi caso, por ejemplo, le pido que me **agrupe los pedidos de compra por proveedor y estado**, y me abre directamente el análisis con la tabla correspondiente.

![Petición a Copilot para agrupar los pedidos de compra por proveedor y estado](17-analizar-agrupar-pedidos.png){: w="1637" h="703" .shadow }

![Análisis resultante generado por Copilot con la tabla agrupada](18-analizar-resultado.png){: w="1700" h="863" .shadow }

Recientemente han añadido incluso la posibilidad de **mostrar campos relacionados de otra tabla** en la vista, lo que amplía bastante lo que se puede montar sin salir de aquí.

![Análisis mostrando campos relacionados de otra tabla](19-analizar-campos-relacionados.png){: w="1041" h="478" .shadow }

> Muy recomendable que los usuarios vean las sugerencias de análisis: es la mejor forma de que se hagan una idea de todo lo que pueden llegar a hacer con sus propios datos.
{: .prompt-tip }

## Sugerir líneas de venta con Copilot

Copilot puede **sugerir líneas de venta** para un pedido a partir de unas indicaciones o incluso de un archivo. Documentado en [sugerir líneas de venta con Copilot](https://learn.microsoft.com/es-es/dynamics365/business-central/sales-suggest-sales-lines-with-copilot).

![Panel de líneas de ventas sugeridas por Copilot](20-lineas-venta-sugeridas.png){: w="1339" h="859" .shadow }

En mi caso, le he pedido que me **copie las líneas del último pedido del cliente**.

![Indicación a Copilot para insertar las líneas del último pedido del cliente](21-lineas-venta-ultimo-pedido.png){: w="1423" h="842" .shadow }

Me ha sugerido las líneas y, como se ve en la imagen, **incluso me indica de dónde las ha cogido**.

![Líneas de venta sugeridas por Copilot con el origen de cada una](22-lineas-venta-resultado.png){: w="979" h="417" .shadow }

![Detalle del origen de las líneas sugeridas por Copilot](23-lineas-venta-origen.png){: w="990" h="499" .shadow }

Las sugerencias son especialmente útiles para **aquellos usuarios que no saben muy bien por dónde empezar** un documento.

## Conclusión

Antes de meternos en la conversación del pago por uso, mi recomendación es sencilla: **saca partido de lo que ya tienes**. Todas estas funcionalidades vienen incluidas, no consumen créditos adicionales a día de hoy y, bien enseñadas, son una vía estupenda para que los usuarios pierdan el miedo a la IA dentro de su ERP. Enséñaselas a tus clientes; el retorno respecto al esfuerzo es difícil de superar.

Ninguna es magia: donde no hay buenos datos —productos sin atributos, fichas incompletas— las sugerencias flojean. Pero funciones como **autocompletar**, **conciliar** o, sobre todo, **analizar** ya aportan valor real en el día a día sin abrir la cartera.

Más adelante veremos las funcionalidades de Copilot que **sí tienen coste adicional**, que a día de hoy son los agentes:

- Agente de ventas
- Agente de cuentas por pagar (terrorífica traducción, por cierto)
- Agente de gastos
- Agentes personalizados

Pero eso, como digo, será en otro artículo. Te lo iré contando.
