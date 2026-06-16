---
title: Controlar rangos de fechas permitidos en Business Central v28 y v28.2
date: 2026-06-11 10:00:00 +0200
categories: [Business Central, Finanzas]
tags: [business-central, finanzas, inventario, costes, cierre, configuracion]
description: "Dos novedades de Business Central v28 y v28.2 para controlar qué se puede registrar y cuándo: fórmulas de fecha en los periodos de registro permitidos y una fecha de corte para los costes de inventario."
image:
  path: /assets/img/posts/rangos-fechas-permitidos-business-central-v28/portada-rangos-fechas.png
  alt: Configuración contabilidad y Config. inventario de Business Central con los campos de fórmula de fecha y fecha de valoración más temprana permitida
---

## El eterno dolor de cabeza de las fechas de registro

Cualquiera que haya gestionado cierres en Business Central sabe que controlar *qué se puede registrar y cuándo* nunca ha sido tan trivial como parece. Los rangos de fechas permitidos se quedan obsoletos cada mes, alguien registra una factura con fecha del periodo cerrado, y de repente el ajuste de coste vuelve a tocar un periodo que dabas por bueno.

En las versiones **28 y 28.2** Microsoft ha incorporado dos funcionalidades que atacan precisamente este problema desde dos frentes distintos: el control general de fechas de registro y el control específico de los costes de inventario. Las he estado revisando y me parecen de esas mejoras que pasan desapercibidas en las notas de versión pero que ahorran muchas horas de soporte. Las cuento por separado porque resuelven cosas diferentes.

## 1. Fórmulas de fecha en los periodos de registro permitidos

Hasta ahora, los campos **Permitir registro desde** y **Permitir registro hasta** trabajaban con fechas fijas. El problema es evidente: hay que actualizarlas manualmente cada periodo, y si te olvidas, o bien dejas la puerta abierta a registros en meses cerrados, o bien bloqueas a los usuarios sin querer.

La novedad es que ahora puedes escribir una **fórmula de fecha** en lugar de una fecha fija. Business Central evalúa esa fórmula contra la fecha de hoy **cada vez que valida una fecha de registro**, de modo que la ventana permitida se recalcula sola.

Los nuevos campos de fórmula están disponibles en:

- **Configuración de contabilidad** (puede que tengas que pulsar *Mostrar más* para verlos).
- **Configuración de usuarios**.

El orden de prioridad con el que se evalúan las restricciones es: primero Configuración de usuarios y después Configuración de contabilidad.

### El caso más habitual: solo el mes en curso

El escenario que más se repite en proyecto es "que solo se pueda registrar dentro del mes actual". En un entorno **en castellano** lo configuro así:

| Campo | Fórmula |
|---|---|
| Permitir registro desde | `-PM` |
| Permitir registro hasta | `PM` |

![Página Configuración contabilidad con los campos Permitir registro desde/hasta fórmula de fecha rellenos con -PM y PM](/assets/img/posts/rangos-fechas-permitidos-business-central-v28/config-contabilidad-formula-fecha.png){: w="1200" h="630" .shadow }
*Los campos de fórmula de fecha en la Configuración contabilidad: `-PM` y `PM` para limitar el registro al mes en curso.*

Y se acabó el mantenimiento mensual: la ventana se desplaza sola cada vez que cambia el mes.

También puedes mezclar fórmula en un límite y fecha fija en el otro; al escribir una fórmula se borra la fecha fija correspondiente y viceversa.

### Cuidado con la documentación de Microsoft Learn

Aquí va el aviso que de verdad te va a ahorrar un rato de frustración:

> Los ejemplos de fórmulas que aparecen en la documentación de Microsoft Learn están **en inglés** (`<-CM>`, `<CM>`, `<-30D>`…) y **no funcionan si tienes Business Central en castellano**.
{: .prompt-warning }

Las fórmulas de fecha dependen del idioma del entorno. Si copias literalmente `<-CM>...<CM>` desde Learn en una instalación en español, no te funcionará. Hay que usar los códigos del idioma de tu BC (en castellano, mes → `M`, y de ahí `-PM`/`PM` para el mes actual). Es un detalle pequeño, pero he visto perder un buen rato a más de uno por no caer en ello.

## 2. Fecha de corte para los costes de inventario

La segunda funcionalidad va a otro terreno, históricamente más delicado: la **valoración y el coste de los productos**.

El problema de fondo es conocido. Cada entrada de valor de inventario tiene una **fecha de valoración**, y el ajuste de coste la usa para decidir a qué periodo pertenece un coste. Si entra una transacción con fecha retroactiva, o una factura que se contabiliza mucho después del envío o la recepción, ese coste puede caer en un periodo que ya habías cerrado. Y cuando eso pasa, la siguiente ejecución del ajuste de coste tiene que volver a recorrer el periodo anterior: lento con volúmenes grandes y, sobre todo, con resultados que no esperabas.

La novedad es el campo **Fecha de valoración más temprana permitida**, en la página **Config. inventario**. Funciona como una barrera de escritura sobre la fecha de valoración.

![Página Config. inventario con el campo Fecha de valoración más temprana permitida resaltado](/assets/img/posts/rangos-fechas-permitidos-business-central-v28/config-inventario-fecha-valoracion.png){: w="1200" h="630" .shadow }
*El campo Fecha de valoración más temprana permitida en la página Config. inventario.*

### Qué bloquea y qué deja pasar

Cuando estableces la fecha, Business Central **bloquea** las operaciones que generarían o modificarían costes antes de ese límite. Entre otras:

- Líneas del diario de revalorización que retrotraerían la fecha de valoración.
- Cargos de producto asignados a una recepción o envío registrados antes del corte.
- **Deshacer** una recepción de compra o un envío de venta anteriores a la fecha.
- Reabrir una orden de producción o de ensamblado terminada que tenga entradas de valor anteriores al corte.

Lo importante es que **no lo bloquea todo**. El sistema sigue siendo operativo:

- El ajuste de coste y los redondeos siguen funcionando con normalidad sobre los movimientos existentes.
- Puedes facturar envíos o recepciones registrados antes del corte, siempre que la factura tenga fecha de registro igual o posterior. El diferencial de coste se lleva al **periodo actual** en lugar de reabrir el cerrado.

Esos movimientos que no aplican, simplemente se omiten, sin lanzar error.

### Es global y no se sobrescribe

A diferencia de los campos *Permitir registro desde/hasta* de la Configuración de contabilidad, la Fecha de valoración más temprana permitida es una **configuración global** y **no se puede sobrescribir** por usuario. Es un único límite mínimo que vas adelantando como parte de cada cierre.

Para poder establecerla hay un requisito previo razonable: el inventario tiene que estar **totalmente ajustado** hasta esa fecha. Es decir, ejecutar *Ajustar coste: entradas de elemento* y resolver órdenes abiertas o periodos pendientes antes de fijar la fecha. Se permiten registros en la propia fecha de corte; solo se bloquea lo anterior.

### Complemento, no sustituto, de los periodos de inventario

Esto es clave para no liarse: la Fecha de valoración más temprana permitida **no sustituye a los periodos de inventario**, trabaja **en paralelo y como complemento** a ellos.

| | Periodos de inventario | Fecha de valoración más temprana permitida |
|---|---|---|
| Qué es | Uno o varios periodos con nombre que se cierran y pueden reabrirse | Un único límite mínimo de fecha |
| Cómo se usa | Cierre/reapertura por periodo | Se adelanta en cada cierre |
| Ámbito | Por periodo | Global, no sobrescribible |

La regla práctica: **si cualquiera de los dos mecanismos bloquearía un registro, el registro queda bloqueado**. Usados juntos, te dan una red de seguridad mucho más sólida sobre los costes.

## Conclusión y recomendación

Son dos mejoras pequeñas en apariencia, pero muy bien orientadas a problemas reales de operación y cierre:

- **Fórmulas de fecha**: configúralas una vez (`-PM`/`PM` para el mes en curso en castellano) y olvídate del mantenimiento mensual. Pero ten presente que los ejemplos de Learn vienen en inglés y hay que traducir los códigos a tu idioma.
- **Fecha de valoración más temprana permitida**: incorpórala a tu rutina de cierre de costes, justo después de registrar todo, valorar el stock y conciliar con contabilidad. Adelántala al primer día del nuevo periodo y dejarás el periodo cerrado protegido frente a revalorizaciones y ajustes retroactivos.

Mi recomendación: pruébalas primero en un entorno de test, especialmente la fecha de corte de inventario, porque obliga a tener el coste totalmente ajustado y conviene entender bien qué deja pasar y qué no antes de aplicarla en producción.

> **Documentación de Microsoft**
> - [Restringir las contabilizaciones de costes retroactivas](https://learn.microsoft.com/es-es/dynamics365/business-central/finance-restrict-backdated-cost-postings)
> - [Usar fórmulas de fecha para controlar los periodos de contabilización permitidos](https://learn.microsoft.com/es-es/dynamics365/release-plan/2026wave1/smb/dynamics365-business-central/use-date-formulas-control-allowed-posting-periods)
{: .prompt-info }
