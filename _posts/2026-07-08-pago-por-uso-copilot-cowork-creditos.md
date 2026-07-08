---
title: "Pago por uso en Copilot Cowork: cuántos créditos consume cada tarea real"
date: 2026-07-08 10:00:00 +0200
categories: [Copilot, Productividad]
tags: [copilot, cowork, pago-por-uso, creditos, productividad, ia]
description: Qué consume en créditos cada tarea real con Copilot Cowork y cómo usar ese dato para decidir qué merece la pena delegar en el agente.
image:
  path: 01-portada.png
  alt: Ilustración del modelo de pago por uso de Copilot Cowork; tareas que consumen créditos
media_subpath: /assets/img/posts/pago-por-uso-copilot-cowork-creditos/
---

El **1 de julio de 2026**, Copilot Cowork cambió su modelo de licenciamiento a un
esquema de **pago por uso**: cada tarea que le encargas al agente consume un número
de créditos. No es un cambio menor: altera de raíz la forma de trabajar. Cuando algo es "gratis e ilimitado" lo usas sin
pensar; cuando cada tarea tiene coste, empiezas a decidir. Y decidir con criterio
exige un dato que hasta ahora nadie te daba: **cuánto cuesta de verdad cada cosa.**

Como consultor, la primera pregunta que me hago ante una herramienta nueva no es
"¿puede hacerlo?", sino "¿cuánto me cuesta que lo haga y compensa frente a hacerlo
yo?". Así que, en esta primera semana con el nuevo modelo, en lugar de teorizar sobre los
créditos me puse a medir mi propio trabajo. Estas ocho tareas ya las venía haciendo con Cowork antes del cambio; son mi día a
día real, no un experimento para el artículo. Lo nuevo no es delegarlas, sino que ahora
tienen precio —así que esta vez anoté lo que consumió cada una.

> Para medir cada tarea me he apoyado en `cost`, una *skill* que Cowork trae de serie y
> que **muestra los créditos consumidos hasta ese momento**. Comprobándola antes y después
> de cada tarea, la diferencia es justo lo que ha costado.
{: .prompt-tip }

## Por qué importa el coste por tarea

En un modelo de pago por uso, el coste deja de ser un detalle de facturación y pasa
a ser una variable de proyecto, como las horas o el alcance. Si vas a apoyarte en un
agente para preparar reuniones, redactar actas u organizar tu jornada, necesitas
saber qué operaciones son baratas (las puedes automatizar a diario sin pensar) y
cuáles son caras (las reservas para los momentos que realmente lo valen).

Lo interesante no es el número absoluto de créditos —depende del plan, del contexto
y del momento—, sino la **relación entre tareas**: qué cuesta el triple que otra
cosa, y por qué.

## El test: ocho tareas reales

Estas son las ocho tareas que medí, con lo que hizo Cowork en cada una y su
consumo:

| Tarea | Qué hizo Cowork | Salida | Créditos |
|-------|-----------------|--------|:--------:|
| Planificar mi jornada | Revisó calendario, bandeja de entrada y chats de Teams no leídos o que requerían respuesta | Lista de tareas priorizada del día | 122 |
| Archivar correo no relevante | Revisó la bandeja, identificó los correos que no aportan y los archivó | Bandeja limpia; solo lo que requiere atención | 121 |
| Preparar una reunión con cliente | Partió de mis tareas del día, cruzó el contexto y ordenó los puntos a tratar | Guión con los puntos a revisar | 596 |
| Preparar el acta de la reunión | Tomó la transcripción y el guión, y extrajo lo relevante | Acta con temas, acuerdos y próximas acciones | 348 |
| Preparar un seguimiento interno de proyecto | Recopiló el estado y los puntos abiertos del proyecto | Preparación de la reunión de seguimiento | 243 |
| Responder a un cliente con horarios | Consultó mi disponibilidad y redactó la respuesta con cuatro franjas para reunirse | Email de respuesta con cuatro huecos propuestos | 160 |
| Revisar una oferta en PowerPoint | Repasó las diapositivas en busca de erratas, faltas de ortografía y fallos de redacción | Lista de erratas y correcciones propuestas | 148 |
| Aplicar las correcciones a la oferta | Aplicó una a una las 34 correcciones detectadas sobre la presentación | Oferta corregida | 286 |

En total, **2.024 créditos** para las ocho tareas, con una media de unos **253
créditos** por tarea. Pero la media engaña: hay un factor cinco entre la más barata
y la más cara.

### 1. Planificar la jornada — 122 créditos

Una de las dos tareas más baratas del lote, muy por debajo del resto. Le pedí que
ejecutara una *skill* propia que tengo
montada para esto: revisa mi calendario del día, mi bandeja de entrada y mis chats
de Teams, filtrando los que están sin leer o que requieren respuesta mía, y con todo
eso arma una lista de tareas priorizada.

Es lectura y síntesis en una sola pasada, sobre fuentes que ya están estructuradas
(calendario, correo, Teams) y guiada por una *skill* que ya sabe qué buscar. No tiene
que inventar nada ni orquestar pasos: recopila, ordena y prioriza. Por eso sale
barata. Y por eso es justo el tipo de tarea que compensa lanzar **todos los días**.

### 2. Archivar correos no relevantes de la bandeja — 121 créditos

La tarea más barata de todas, 121 créditos. Le pedí que
repasara la bandeja de entrada, identificara los correos que no aportan (avisos,
notificaciones, ruido) y los archivara, dejando a la vista solo lo que requiere
atención.

Es la única que **ejecuta una acción** sobre el buzón sin apenas escribir nada —solo
mueve correos—, y aun así sale baratísima. El motivo es el mismo que con la planificación:
trabaja sobre una fuente ya estructurada, aplica un filtro claro y actúa en lote —poco
contexto que reconstruir y una decisión sencilla por mensaje—. Otra candidata perfecta
para lanzar a diario.

### 3. Preparar una reunión con cliente — 596 créditos

La más cara del lote, casi cinco veces lo que cuesta planificar la jornada. Partiendo de las
tareas del día, Cowork tuvo que entender qué era relevante para ese cliente, cruzar
el contexto disperso y construir desde cero un **guión con los puntos a revisar** en
la reunión.

Aquí el agente no solo lee: **decide y genera**. Selecciona qué entra y qué no,
le da un orden que tenga sentido para una conversación con cliente y produce un
entregable nuevo. Es trabajo de orquestación de varios pasos, y eso se paga. También
es, no por casualidad, la tarea de mayor valor de las ocho: llegar a una reunión de
cliente con un guión claro vale mucho más que lo que costó en créditos.

### 4. Preparar el acta de la reunión — 348 créditos

A partir de la transcripción y del guión anterior, Cowork extrajo los **temas
tratados, los acuerdos y las próximas acciones**, y lo dejó en un acta estructurada.

Cuesta bastante menos que preparar la reunión (348 frente a 596) por una razón que
me parece clave: **partía de material ya ordenado**. La transcripción aportaba el
contenido y el guión aportaba la estructura de lo que había que buscar. El agente no
tuvo que descubrir el contexto, solo destilarlo. Cuando le das buenas entradas, la
salida sale más barata.

### 5. Preparar un seguimiento interno de proyecto — 243 créditos

Costó 243 créditos, por debajo del acta. Recopiló el estado y los puntos abiertos del
proyecto y preparó el material para la reunión de seguimiento. Es una tarea de
recopilación y preparación de complejidad media: más que una simple lectura, pero por
debajo de construir un guión de cliente desde cero. Al apoyarse en información de
proyecto que ya existía, no tuvo que reconstruir demasiado contexto nuevo, y eso se
nota en el consumo.

### 6. Responder a un cliente con franjas disponibles — 160 créditos

160 créditos por contestar el correo de un cliente proponiéndole cita. Cowork leyó su
petición, **cruzó mi calendario de los próximos días** para ver dónde tenía hueco y
redactó la respuesta con **cuatro franjas libres** entre las que elegir.

Sale barata, en la parte baja de la tabla, y tiene sentido: aunque combina leer y
escribir —consultar disponibilidad real y componer una respuesta de cliente correcta—,
el entregable es corto y acotado, un email con cuatro huecos y no un guión completo.
Por eso cuesta una fracción de preparar la reunión entera (596).

### 7. Revisar una oferta en PowerPoint — 148 créditos

148 créditos por repasar una oferta en PowerPoint —diapositiva a diapositiva— en
busca de erratas, faltas de ortografía y frases que chirrían. Le pasé el archivo y me devolvió
una lista con las correcciones propuestas.

Es barata por lo mismo que planificar o archivar: trabaja
sobre una fuente ya cerrada —la propia presentación—, hace una única pasada de lectura y
evaluación, y no tiene que orquestar pasos ni construir nada nuevo, solo señalar qué cambiar.
Da igual que la entrada sea larga: lo que marca el coste es que la tarea consiste en leer y
juzgar, no en recopilar contexto disperso y montar un entregable. Y es justo el repaso que da
tranquilidad antes de mandarle una propuesta a un cliente.

### 8. Aplicar las correcciones a la oferta — 286 créditos

Aquí le pedí que cogiera las **34 correcciones** que había detectado en el caso anterior y
las aplicara una a una sobre la oferta, hasta dejarla corregida.

Cuesta 286 créditos, casi el doble que detectarlas (148), y tiene su lógica: **detectar es
una sola pasada de lectura; aplicar son 34 operaciones concretas** —localizar cada fallo en
su diapositiva y modificarlo—, una por corrección. No es orquestación de contexto disperso
como preparar la reunión de cliente, pero sí mucho trabajo mecánico acumulado, y eso también
se paga. Con todo, detectar y corregir juntas (434) aún salen por debajo de preparar una sola
reunión de cliente (596).

## Qué me dicen los números

Sacando la lupa de cada tarea y mirando el conjunto, salen cuatro patrones bastante
claros:

- **El coste sigue a la orquestación, no a la longitud de la salida.** La tarea más
  cara no es la que produce el texto más largo, sino la que obliga al agente a
  recopilar contexto disperso, decidir qué es relevante y montar algo nuevo. Preparar
  la reunión de cliente (596) es cara porque el agente trabaja mucho *antes* de
  escribir.
- **Buenas entradas abaratan la salida.** El acta (348) costó menos que la reunión de
  cliente (596) porque partía de una transcripción y un guión ya hechos. Cuanto más
  ordenado sea lo que le das, menos tiene que reconstruir el agente, y menos paga uno.
- **Lo rutinario es barato; resérvalo para el día a día.** Planificar la jornada (122)
  y archivar el correo no relevante (121) son las dos tareas más baratas: leer,
  filtrar y actuar sobre fuentes ya estructuradas. Encapsúlalas en *skills* y lánzalas
  cada mañana; el agente no explora a ciegas, va directo a lo que importa.
- **Detectar es más barato que ejecutar.** Revisar la oferta y listar sus fallos (148)
  costó casi la mitad que aplicar esas mismas 34 correcciones (286): encontrar el
  problema es una pasada de lectura; resolverlo son muchas operaciones concretas, una
  por cambio.

> Estos créditos son de **mi** prueba concreta. Variarán según la tarea, el volumen
> de correo y chats, el proyecto y el momento. Tómalos como órdenes de magnitud para
> comparar tipos de tarea, no como una lista de precios.
{: .prompt-warning }

## Mi recomendación práctica

Si vas a trabajar con Cowork en modo pago por uso, mi consejo tras medirlo es sencillo:

1. **Automatiza lo barato y recurrente.** Las tareas de leer, filtrar y priorizar
   (planificar la jornada, archivar el correo no relevante) cuestan poco. Encapsúlalas
   en *skills* y lánzalas a diario.
2. **Reserva lo caro para lo que lo vale.** La orquestación de varios pasos —preparar
   una reunión de cliente— es cara, pero es justo donde el agente te ahorra el trabajo
   más incómodo. Gástalo ahí, no en trivialidades.
3. **Prepara buenas entradas.** Un guión decente hace que el acta salga más barata; una
   transcripción limpia, también. Invertir en estructura al principio reduce el coste
   del resto de la cadena.

El modelo de créditos, bien mirado, no es solo una forma de facturar: es un incentivo
a **usar el agente con criterio**. Y para tener criterio, primero hay que medir. Estos
2.024 créditos son mi primera medición; el siguiente paso es repetirla sobre tareas
distintas y afinar dónde compensa delegar y dónde no.

¿Has medido ya tus propias tareas con el nuevo modelo? Me encantará leer qué te sale a ti.
