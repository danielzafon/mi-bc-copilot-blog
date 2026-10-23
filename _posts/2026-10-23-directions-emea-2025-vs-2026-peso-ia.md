---
title: "Directions EMEA 2025 vs 2026: casi la mitad de las sesiones ya hablan de IA"
date: 2026-10-23 16:30:00 +0200
categories: [Business Central, Inteligencia Artificial]
tags: [business-central, directions-emea, ia, agentes, copilot, al, comunidad]
description: Comparativa de las sesiones de Directions EMEA 2025 en Poznań y 2026 en París; cómo ha crecido el peso de la IA, qué temas pierden protagonismo y qué conclusiones sacar si trabajas con Business Central.
image:
  path: 01-sesiones-por-tema.png
  alt: Gráfico de barras que compara el número de sesiones por tema en Directions EMEA 2025 y 2026
media_subpath: /assets/img/posts/directions-emea-2025-vs-2026-peso-ia/
---

La semana que viene arranca [Directions EMEA 2026](https://www.directionsforpartners.com/emea2026/) en París (27-29 de octubre). Antes de montarme la agenda quise responder a una pregunta sencilla: ¿cuánto ha cambiado el contenido respecto a la edición de Poznań del año pasado?

La intuición era clara: más IA. Pero quería ver cuánta, de qué tipo y, sobre todo, qué ha dejado de tener espacio para hacerle hueco.

## Cómo lo he medido

He sacado los títulos de todas las sesiones de ambas ediciones desde el muro de ponentes de la web oficial: 283 sesiones en 2025 y 285 en 2026, sin contar keynotes ni eventos sociales. Después he clasificado cada título por palabras clave en una serie de temas (IA, agentes, desarrollo con IA, finanzas, fabricación, datos, etc.). Una misma sesión puede caer en varios temas.

> Es una medición por títulos, no por contenido. Una sesión sobre e-documents puede llevar IA dentro sin decirlo en el título, y al revés. Toma las cifras como tendencia, no como dato exacto.
{: .prompt-info }

He usado la página de ponentes también para 2025 para que las dos muestras sean comparables; en el momento de escribir esto, la agenda detallada de 2026 todavía no carga en la web.

![Sesiones por tema en Directions EMEA 2025 y 2026](01-sesiones-por-tema.png){: w="1200" h="630" .shadow }
*Número de sesiones por tema según palabras clave en el título.*

## Los números

| Tema | 2025 (Poznań) | 2026 (París) | Tendencia |
|---|---:|---:|---|
| IA (cualquier mención) | 83 | 133 | ▲ de ~29% a ~47% del total |
| Agentes / agéntico | 21 | 54 | ▲ ×2,6 |
| Desarrollo asistido por IA | 8 | 20 | ▲ |
| Seguridad / gobernanza / compliance | 12 | 19 | ▲ |
| Negocio partner / GTM / ventas | 55 | 60 | = |
| Fabricación / supply / WMS | 24 | 28 | = |
| Finanzas / e-docs / banca | 32 | 20 | ▼ |
| Datos / reporting | 33 | 17 | ▼ |
| DevOps / AL-Go / testing | 17 | 10 | ▼ |
| Power Platform / Copilot Studio | 21 | 9 | ▼ |
| MCP como tema propio | 5 | 2 | ▼ |

El volumen total apenas cambia. Lo que cambia es el reparto: la IA no se ha sumado a la agenda, ha ocupado sitio que antes tenían otros temas.

## Hallazgo 1: de explicar qué es un agente a construirlo, probarlo y cobrarlo

En 2025 los agentes eran novedad. Había sesiones para entender qué es un agente y presentaciones de los primeros agentes de Microsoft: Sales Order Agent y Payables Agent, con sus workshops de iniciación.

En 2026 el tono es otro. Los títulos hablan de:

- **Nuevos agentes de producto**: Expense Agent con una serie de tres sesiones, Order Agent, Integration Agent e Implementation Agents para la propia entrega de proyectos.
- **Construcción**: diseño y codificación de agentes en AL, con workshop de Microsoft incluido.
- **Calidad**: cómo evaluar, experimentar y testear agentes en Business Central.
- **Operación y gobierno**: monitorización de agentes, seguridad en la "era agéntica", confianza del usuario antes de que el agente empiece a trabajar.
- **Negocio**: monetización de la IA y facturación por consumo, como una de las sesiones destacadas de Microsoft.

Es el paso de "esto existe" a "esto hay que entregarlo, mantenerlo y facturarlo". Para quien gestiona proyectos, esa segunda conversación es la que importa.

## Hallazgo 2: el vibe coding se hace mayor

El año pasado había cuatro sesiones con "vibe coding" en el título. Este año el término prácticamente desaparece y lo sustituyen enfoques con más disciplina:

- Spec-driven development y flujos multiagente para AL.
- Harnesses para agentes de código y revisión de código específica de Business Central con agentes.
- Benchmarks sobre si los agentes resuelven tareas reales de AL.
- Testing como red de seguridad del desarrollo agéntico, incluido mutation testing.
- La pregunta incómoda: si la IA escribe el AL, ¿quién es responsable del resultado?

Las sesiones de desarrollo asistido por IA pasan de 8 a 20. Encaja con lo que veo en el día a día: generar código ya no es el problema. El problema es que lo generado sea verificable, mantenible y que alguien responda por ello.

## Hallazgo 3: el coste de la IA entra en la agenda

Esto no aparecía en los títulos de 2025 y me parece la señal más interesante. Hay varias sesiones centradas en cuánto cuesta todo esto:

- Reducir el consumo de tokens combinando razonamiento de IA con herramientas deterministas.
- Lo que cuesta (y ahorra) realmente el desarrollo agéntico.
- El coste por tarea resuelta según el modelo, en lugar de preguntar qué modelo escribe mejor AL.
- La factura de Copilot como riesgo real.

Y en paralelo, el lado comercial: dejar de vender horas, el fin del time & materials y la preparación para precios basados en resultados. Si la IA comprime el esfuerzo, el modelo de facturación por horas se resiente. Que la comunidad lo ponga sobre la mesa en una conferencia de partners dice mucho.

> Si ya usas agentes o Copilot en desarrollo, mide el coste por tarea completada, no por llamada ni por usuario. Es la única métrica que te permite comparar con el coste de hacerlo sin IA. Así lo planteé al medir [cuántos créditos consume cada tarea real en Copilot Cowork](https://danielzafon.github.io/mi-bc-copilot-blog/posts/pago-por-uso-copilot-cowork-creditos/).
{: .prompt-tip }

## Lo que pierde peso

- **Power Platform** cae de 21 a 9 sesiones. Dataverse, Power Apps y page scripting casi desaparecen de los títulos.
- **Datos y reporting** baja a la mitad. En 2025 había muchas sesiones de Excel layouts, Power BI y telemetría; en 2026 el foco se desplaza a Fabric como backend analítico y al conector nativo con Business Central.
- **Finanzas y e-documents** baja de 32 a 20, aunque siguen las sesiones de novedades de Microsoft sobre finanzas y E-Documents/EDI.
- **DevOps y AL-Go** baja de 17 a 10, en parte absorbido por el testing orientado a agentes.
- **MCP** deja de ser tema propio. En 2025 se presentaba el MCP Server de Business Central; en 2026 aparece integrado en Copilot y Cowork. Cuando una tecnología deja de tener sesiones de introducción suele ser porque ya se da por asumida.

## Lo que no cambia

El bloque de negocio (go-to-market, ventas, programas de partner, licencias) se mantiene alrededor de 55-60 sesiones. Fabricación y almacén también aguantan, con novedades funcionales como Quality Management y Subcontracting.

Microsoft sigue aportando alrededor de 90 sesiones, pero con más formato de diálogo: las sesiones de preguntas y respuestas pasan de 1 a 4. Las sesiones de estrategia por país pasan de 7 a 11, y este año hay una del equipo de Microsoft España.

## La comunidad española, en primera línea

Como parte de la comunidad española de Business Central, es un orgullo ver que compañeros como Javier Armesto, Roberto Corella y Fernando Artigas están marcando el camino justo en los temas que más crecen este año: desarrollo AL multiagente guiado por especificaciones, open source y el nuevo perfil del desarrollador que trabaja con agentes. Javi lo hace, además, con [ALDC](https://github.com/javiarmesto/ALDC-AL-Development-Collection), su framework multiagente para AL, que comparte altruistamente con toda la comunidad y que en mi compañía utilizamos en el día a día. Sin duda, sus sesiones merecen un hueco en la agenda.

## Mi apuesta para 2027

Me voy a lanzar a la piscina. Si la tendencia sigue, me atrevo con tres predicciones que podré contrastar el año que viene:

- **La IA empezará a desaparecer de los títulos.** Cuando una tecnología se da por asumida, deja de ser noticia nombrarla. Si pasa, el porcentaje medido por títulos podría estancarse o incluso bajar aunque la IA esté en más sesiones que nunca, y tendré que buscar otra forma de medirlo.
- **El coste y el precio pasarán de un puñado de sesiones a un bloque propio**: coste por tarea, modelos de facturación por resultado y cómo presupuestar proyectos con agentes.
- **El testing y la gobernanza de agentes superarán a las sesiones sobre cómo construirlos.** Montar un agente ya no será la novedad; demostrar que funciona y que alguien responde por él, sí.

En 2027 lo comprobamos.

## Qué me llevo

Tres conclusiones prácticas, según el rol:

1. **Si eres desarrollador AL**, la habilidad que gana valor no es escribir prompts, sino diseñar especificaciones, tests y revisiones que hagan fiable el código generado. El testing vuelve a ser central, ahora como condición para delegar en agentes.
2. **Si eres consultor o jefe de proyecto**, los agentes de producto (Expense, Payables, Sales Order) van a entrar en los alcances. Conviene saber configurarlos, pero sobre todo saber explicar al cliente qué decide el agente, qué supervisa una persona y cómo se audita.
3. **Si diriges un partner**, la conversación sobre precio y coste de la IA ya está aquí. Antes de vender horas de un trabajo que la IA acelera, merece la pena tener claro cuánto cuesta realmente entregarlo y qué valor estás cobrando.

La foto es clara: en un año la IA ha pasado de ser un bloque más de la agenda a ser el hilo que atraviesa casi la mitad de ella. Lo interesante no es la cantidad, sino que las preguntas han madurado: menos "qué puede hacer" y más "cómo lo controlo, cuánto me cuesta y quién responde".

Si vas a estar en París, nos vemos allí. Y si no, a la vuelta te cuento qué sesiones han merecido la pena.
