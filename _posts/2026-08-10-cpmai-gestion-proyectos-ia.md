---
title: "CPMAI: así plantea PMI la gestión de proyectos de IA"
date: 2026-08-10 10:00:00 +0200
categories: [Gestión de Proyectos, Inteligencia Artificial]
tags: [cpmai, pmi, ia, gestion-proyectos, metodologia]
description: Repaso al curso introductorio gratuito de PMI sobre CPMAI; el reto de adaptar la gestión de proyectos a la IA, el enfoque por patrones y el ciclo iterativo de seis fases.
image:
  path: 01-portada.png
  alt: CPMAI; ciclo iterativo de seis fases para gestionar proyectos de IA
media_subpath: /assets/img/posts/cpmai-gestion-proyectos-ia/
toc: true
---

Estas vacaciones, entre el descanso y algún que otro rato muerto, he aprovechado para hacer el curso online gratuito que PMI tiene publicado en su plataforma sobre CPMAI (Cognitive Project Management in AI). Llevaba tiempo con la curiosidad de ver cómo estaba encajando el mundo de la gestión de proyectos "de toda la vida" con la realidad de los proyectos de IA, y este curso me ha servido para poner orden a esa inquietud.

## El reto de fondo

Las guías clásicas de gestión de proyectos se quedan cortas cuando el entregable no es un proceso o una integración, sino un modelo que aprende de datos y que puede comportarse de forma distinta según lo que se le dé de entrada. Alcance cerrado desde el principio, hitos lineales, poca tolerancia a volver atrás: ese esquema funciona mal en cuanto el dato o el modelo no se comportan como esperábamos.

PMI es consciente de esto: entre el 70% y el 80% de los proyectos de IA fracasan, y buena parte de esos fracasos no vienen de la tecnología en sí, sino de aplicarle a un proyecto de IA la misma disciplina que a uno tradicional. De ahí nace CPMAI, la metodología que Ron Schmelzer y Kathleen Walch desarrollaron en Cognilytica y que PMI incorporó como certificación propia (PMI-CPMAI) tras adquirir la compañía en 2024.

## El enfoque: primero el patrón, luego el proyecto

Antes de hablar de fases, CPMAI pone el foco en algo que me parece clave: identificar qué patrón de IA estás construyendo (merece la pena tenerlos claros si alguna vez te toca defender el enfoque delante de un cliente). La metodología define siete patrones:

1. **Hiperpersonalización**: perfiles individuales que se afinan con el tiempo (recomendaciones, contenido adaptativo).
2. **Interacción conversacional y humana**: chatbots, asistentes virtuales, traducción, generación de contenido.
3. **Reconocimiento**: identificar objetos, voz, texto o imágenes dentro de contenido no estructurado.
4. **Detección de patrones y anomalías**: fraude, ciberseguridad, mantenimiento predictivo.
5. **Analítica predictiva y soporte a la decisión**: previsión de demanda, riesgo, planificación, con el humano en el control.
6. **Sistemas orientados a objetivos**: aprendizaje por refuerzo, optimización de rutas o recursos.
7. **Sistemas autónomos**: agentes físicos o digitales que perciben, deciden y actúan con mínima intervención humana.

Un mismo proyecto puede combinar varios patrones (un asistente conversacional que además personaliza recomendaciones, por ejemplo). Pero saber de entrada qué patrón o combinación de patrones estás abordando condiciona el dato que necesitas, cómo lo vas a evaluar y qué riesgos de gobierno tienes que vigilar. Es, en el fondo, el equivalente a elegir bien el enfoque antes de dimensionar el proyecto.

## Las seis fases del ciclo de vida

Sobre la metodología en sí, CPMAI organiza el proyecto en seis fases (PMI las detalla en su [guía oficial de la metodología](https://learning.pmi.org/resources?coursekey=CPMAI&filename=CPMAI_Overview_Guide.pdf)):

| Fase | Pregunta clave que hay que responder |
|---|---|
| I. Comprensión del negocio | ¿Es la IA la solución adecuada para este problema y qué patrón encaja? |
| II. Comprensión del dato | ¿Existe el dato necesario, es accesible y refleja el contexto real del problema? |
| III. Preparación del dato | ¿El dato está limpio, etiquetado y en condiciones de alimentar un modelo? |
| IV. Desarrollo del modelo | ¿El modelo elegido, entrenado con ese dato, cumple los objetivos medibles fijados antes? |
| V. Evaluación del modelo | ¿El modelo rinde de forma fiable y se alinea con los criterios de negocio y de confianza (equidad, sesgo)? |
| VI. Operacionalización | ¿El modelo se integra en el flujo real de trabajo, con monitorización y realimentación? |

## Iterativo no significa "repetir sin más"

Lo que más me ha cambiado el chip del curso es esto: CPMAI no es iterativo en el sentido genérico de "repetir ciclos", sino que cada fase se cierra respondiendo a una pregunta de validación concreta, como las de la tabla anterior. Si la respuesta confirma que se cumplen los criterios, el equipo avanza a la siguiente fase; si no, no se fuerza el avance, se vuelve a una fase anterior para corregir. El propio PMI lo resume así: "estas seis fases no son de una sola pasada; los proyectos de IA suelen requerir ajustes según cambian los datos, evolucionan los objetivos o el modelo se comporta de forma inesperada".

El matiz que yo añadiría es que no hay un checklist rígido de aceptación/rechazo por fase, como si fuera una puerta de calidad clásica (tipo stage-gate). Es más bien un criterio de madurez: si al evaluar el modelo descubres que el problema real está en cómo entendiste el negocio en la fase I, vuelves ahí, no solo a repetir el entrenamiento. La iteración no es siempre "un paso atrás"; puede ser volver dos o tres fases si el origen del problema está más arriba en la cadena.

## Por qué me importa esto en mi día a día

Trasladado a nuestro terreno, el de quienes gestionamos proyectos de Business Central con Copilot o con automatizaciones basadas en IA, esto encaja con algo que ya intuía por experiencia: cuando un cliente pide "meter IA" en un proceso, el error más caro no está en el modelo, está en saltarse la fase de comprensión del negocio y del dato para llegar antes a construir algo. CPMAI le pone nombre y estructura a algo que muchos gestores de proyecto ya aplicábamos de forma intuitiva, y me da un vocabulario más claro para explicarle a un cliente por qué, si la calidad del dato no acompaña, lo sensato es volver a la fase anterior antes de seguir invirtiendo en el modelo.

## Mi recomendación práctica

Si te dedicas a gestionar proyectos y te está tocando lidiar con iniciativas de IA, esto es lo que yo haría:

1. **Identifica primero el patrón de IA** (o la combinación de patrones) antes de estimar nada; condiciona todo lo que viene después.
2. **No te saltes la fase de datos** para llegar antes al modelo; ahí está el origen de la mayoría de los fracasos que cita PMI.
3. **Define de antemano la pregunta de validación de cada fase**, para saber cuándo avanzar y cuándo tienes que volver atrás.
4. **Haz el curso introductorio de PMI**: es gratuito y en un par de horas te deja el mapa mental claro. Merece la pena, aunque sea en vacaciones.

---

*Fuentes: [CPMAI Overview Guide](https://learning.pmi.org/resources?coursekey=CPMAI&filename=CPMAI_Overview_Guide.pdf), [6 Phases to Run a Successful AI Project](https://www.pmi.org/blog/6-stages-to-run-a-successful-ai-project), [Understanding the Seven Patterns of AI](https://www.pmi.org/blog/seven-patterns-of-ai), [Preparing Project Managers for an AI-Driven Future](https://www.pmi.org/blog/preparing-project-managers-for-an-ai-driven-future), [Why Most AI Projects Fail: 10 Mistakes to Avoid](https://www.pmi.org/blog/why-most-ai-projects-fail) y [Free Introduction to Cognitive Project Management in AI (CPMAI)](https://www.pmi.org/shop/p-/elearning/free-introduction-to-cognitive-project-management-in-ai-cpmai/el185).*
