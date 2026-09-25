---
title: "Revalorizar inventario a cero en Business Central: por qué el saldo contable no cuadra"
date: 2026-09-25 09:00:00 +0200
categories: [Business Central, Inventario]
tags: [business-central, inventario, costes, revalorizacion, periodos-inventario, contabilidad]
description: Un caso real en el que el valor del inventario quedaba a cero pero la contabilidad no; qué falló, cómo lo corregimos y qué mantenimiento evita que vuelva a pasar.
image:
  path: 01-portada.png
  alt: Inventario con stock, valor revalorizado a 0,00 € y contabilidad con saldo distinto de cero en la cuenta de existencias
media_subpath: /assets/img/posts/revalorizar-inventario-a-cero-business-central/
---

Recientemente, un cliente nos comunicó que había cambiado el coste de un grupo de productos a cero y, sin embargo, seguía habiendo coste. Además, el coste con el que se daban de baja esos productos no era coherente con nada.

Todo empezó por ahí. Lo que parecía una consulta puntual acabó sacando a la luz varias cosas que llevaban tiempo sin hacerse bien.

No es un caso aislado. A lo largo de estos años me he encontrado muchas situaciones parecidas en clientes, y casi siempre por lo mismo: no gestionar bien las entradas de coste, las revalorizaciones, los periodos de inventario o el registro de la variación de existencias. A veces es que no se entiende bien el proceso; otras, que se dejan pasos o que no se mantiene en el día a día. Y muy a menudo, que no está claro qué tiene en cuenta la valoración de existencias y qué no.

Te cuento este caso porque reúne casi todos esos problemas y porque, como suele pasar, se descubrieron justo cuando alguien necesitaba que los números cuadraran.

## Primer error: dar de baja y de alta no cambia el valor

Al revisarlo con el cliente vimos que, en realidad, no había cambiado el coste de nada. Había registrado un ajuste negativo de todo el stock y, el mismo día, un ajuste positivo por la misma cantidad. La idea era "vaciar" el valor y volver a dar de alta las unidades a coste cero.

No funciona así. Si lo haces en la misma fecha, para Business Central el producto en ningún momento llega a tener coste cero: sale stock valorado y vuelve a entrar stock, y el coste se recalcula con todos esos movimientos. En la práctica, el coste hacía cosas raras. Por ejemplo, un producto con 10 unidades valoradas en 1.000 € que se daba de baja y de alta el mismo día terminaba con el coste reducido a la mitad. Ni cero ni el valor original: un número que no representa nada.

Si quieres cambiar el valor de un producto, la forma correcta es el **diario de revalorización**:

1. Abres el diario de revalorización y usas **Calcular valor inventario** para los productos afectados.
2. Informas `0` en el campo de coste unitario revalorizado (o en el importe revalorizado).
3. Registras el diario.

Así el sistema genera movimientos de valor que ajustan el coste de las entradas abiertas sin tocar las cantidades.

> Antes de hacerlo en producción, pruébalo en un sandbox copiado del entorno real. Nosotros lo validamos primero así.
{: .prompt-tip }

## Segundo error: movimientos antiguos fuera de la variación de existencias

En un entorno de pruebas, revalorizamos todos los productos afectados a cero y lanzamos **Valorar stock - movs. producto**. El informe de valoración de inventario ya mostraba estos productos con coste cero. Sin embargo, en contabilidad, las cuentas de existencias correspondientes seguían teniendo saldo.

Investigando ese descuadre salió a la luz todo lo demás. Para empezar, había movimientos de años anteriores que nunca se habían incluido en la variación de existencias. No porque la variación estuviera mal calculada en su momento, sino porque esos movimientos **se generaron después** de registrarla. Algunos, incluso un año más tarde.

Un ejemplo muy representativo: un movimiento de octubre de 2023 que se registró en marzo de 2024. Su fecha caía en un periodo ya cerrado desde el punto de vista del negocio, pero el sistema lo aceptó sin problemas.

Cuando volvimos a registrar la variación de existencias sin fecha de inicio en el filtro (`..31/08/2026`, es decir, desde el principio hasta esa fecha), el valor del inventario y el saldo contable quedaron ambos a cero.

El efecto secundario es que se generaron movimientos contables en periodos que ya deberían estar cerrados. Es el mal menor: contablemente se puede hacer un asiento de reclasificación y llevar ese impacto al ejercicio en curso si no quieres modificar ejercicios anteriores.

## Tercer error: la valoración de stock llevaba meses parada

Que se registren movimientos con fecha antigua puede pasar por muchos motivos. El problema no es ese, sino que el sistema no esté preparado para absorberlos.

> En un [artículo anterior sobre los rangos de fechas permitidos en Business Central v28](https://danielzafon.github.io/mi-bc-copilot-blog/posts/2026-06-11-rangos-fechas-permitidos-business-central-v28/) vimos herramientas para evitar que se registren movimientos en periodos cerrados: las fórmulas de fecha en los periodos de registro permitidos y la **Fecha de valoración más temprana permitida** para los costes de inventario.
{: .prompt-info }

En este caso, el proceso **Valorar stock - movs. producto** (*Adjust Cost – Item Entries*) llevaba parado desde junio. Es el proceso que calcula el coste real y lo propaga de las entradas a las salidas. Si no se ejecuta, los costes no se ajustan y todo lo que viene después (variación de existencias, cierre de periodos, conciliación con contabilidad) trabaja sobre datos incompletos.

Microsoft recomienda ejecutarlo con la mayor frecuencia posible, fuera del horario laboral, o bien activar el ajuste automático de coste en la configuración de inventario. Mi recomendación es tenerlo en la **cola de proyectos** para que se ejecute cada noche.

> Revisa de vez en cuando las entradas de la cola de proyectos. Una entrada que se queda en estado *Error* o *En espera* puede pasar meses sin ejecutarse sin que nadie se dé cuenta.
{: .prompt-warning }

## Cuarto error: periodos de inventario sin mantener

El último periodo de inventario creado terminaba el 31/12/2025 y estaba cerrado. No había ninguno abierto.

Aquí está la clave. Según la documentación de Microsoft, cuando el ajuste de coste genera un movimiento de valor cuya fecha cae en un periodo de inventario cerrado, **lo registra en la fecha de inicio del siguiente periodo abierto**. Pero si no hay ningún periodo abierto al que llevarlos, los movimientos van a su propia fecha, aunque ese periodo esté cerrado.

La regla es simple: **cuando cierres un periodo de inventario, crea el siguiente y déjalo abierto.**

Y ya que estás, revisa también los **periodos contables**. En este cliente, el periodo de enero de 2027 existía pero no tenía marcado **Inicio ejercicio**. Es un detalle que no da problemas hasta que llega el cierre del ejercicio.

## Plan para corregirlo en producción

Con todo validado en pruebas, este fue el plan que propusimos, con responsables claros:

| Paso | Tarea | Responsable |
|---|---|---|
| 1 | Revalorizar todos los productos afectados para dejar su valor a 0 con el diario de revalorización | Nosotros |
| 2 | Ejecutar **Valorar stock - movs. producto** después de la revalorización | Nosotros |
| 3 | Registrar la variación de existencias con filtro de fecha `..31/08/2026` | Cliente |
| 4 | Comprobar que el valor del informe de inventario y el saldo de las cuentas de existencias correspondientes son 0 | Cliente |
| 5 | Si no se quieren tocar periodos anteriores, registrar los asientos de reclasificación al ejercicio actual | Cliente |

Separar quién hace cada paso importa tanto como el orden. Nosotros les ayudamos con la revalorización y con el lanzamiento de la valoración de stock; la parte contable, es decir, la variación de existencias, las comprobaciones y los asientos de reclasificación, la dejamos en sus manos.

## Un apoyo adicional: la fecha de valoración más temprana permitida

Del campo **Fecha de valoración más temprana permitida** (en **Config. inventario**) ya hablé en el artículo que mencionaba antes, pero en un caso como este merece la pena volver sobre él. Actúa como barrera: impide que un registro cree o modifique coste en fechas anteriores a ese valor. Si se factura un envío o una recepción antigua, la diferencia de coste va al periodo actual en lugar de reabrir el antiguo.

Funciona en paralelo a los periodos de inventario, y un registro se bloquea si cualquiera de los dos lo impide. Aquí habría evitado que el movimiento de 2023 escribiera coste en un ejercicio ya cerrado.

## Lo que hay que mantener para que no vuelva a pasar

- **Revaloriza siempre con el diario de revalorización.** Dar de baja y volver a dar de alta el stock no cambia su valor.
- **Asegúrate de que Valorar stock - movs. producto se ejecuta cada día** y vuelve a ponerlo en marcha si se para.
- **Mantén los periodos de inventario**: siempre uno abierto, y cierra cada periodo cuando hayas registrado la variación.
- **Mantén los periodos contables creados y bien configurados**, incluido el indicador de inicio de ejercicio.
- **Valora usar la fecha de valoración más temprana permitida** como red de seguridad frente a registros con fecha atrasada.

El problema no estaba en la herramienta. Como en tantos otros casos, estaba en no entender bien el proceso de costes y en no tener claro qué tiene en cuenta la valoración de existencias y qué no. Antes de tocar el coste de un producto, merece la pena entender qué va a pasar después.

Y esto es solo una parte. En otro artículo te contaré los problemas que más me he encontrado con la valoración de existencias: albaranes y devoluciones de venta o de compra pendientes de facturar, y otros casos que, sin hacer ruido, acaban descuadrando el inventario.
