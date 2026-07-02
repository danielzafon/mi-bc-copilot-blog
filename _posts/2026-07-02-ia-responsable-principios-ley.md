---
title: "IA responsable: de los seis principios de Microsoft a la ley española"
date: 2026-07-02 10:00:00 +0200
categories: [Inteligencia Artificial, Copilot]
tags: [ia-responsable, copilot, business-central, gobernanza, ley-ia, ai-act]
description: La IA responsable deja de ser una declaración de buenas intenciones; el compromiso de Microsoft y sus seis principios están alineados con lo que la nueva ley española de gobernanza de la IA empieza a exigir. Qué cambia en tus proyectos de Business Central y Copilot.
image:
  path: 01-portada.png
  alt: Concepto de inteligencia artificial responsable con supervisión humana y un marco legal
media_subpath: /assets/img/posts/ia-responsable-principios-ley/
---

La inteligencia artificial está en todas partes: titulares, comités de dirección, hojas de ruta de producto. Se habla mucho de lo que la IA *puede* hacer y bastante menos de los riesgos y de la responsabilidad que conlleva ponerla a trabajar sobre datos y decisiones reales.

En los proyectos de implantación que llevamos, esa conversación ya ha cambiado. La pregunta que más se repite no es "¿qué puede hacer Copilot?", sino "¿quién responde cuando se equivoca?". Y en España esa pregunta empieza a tener, además, una respuesta legal.

## De la ética a la ley

Durante años, la IA responsable ha vivido en el terreno de los principios y las buenas prácticas. Eso está cambiando. El 26 de mayo de 2026, el Consejo de Ministros aprobó el **proyecto de Ley Orgánica para el buen uso y la gobernanza de la inteligencia artificial** y lo remitió al Congreso de los Diputados. La norma adapta al ordenamiento español el [**Reglamento europeo de IA (RIA)**](https://artificialintelligenceact.eu/es/), que ya está en vigor desde agosto de 2024.

Conviene ser precisos: el Reglamento europeo es de aplicación directa y ya obliga; el texto español es todavía un proyecto de ley en tramitación. Pero la dirección es inequívoca y, para quien diseña e implanta sistemas con IA, marca por dónde van a ir las reglas del juego.

Lo que plantea el proyecto de ley, en lo esencial:

- **Supervisión humana** en los casos que puedan afectar a derechos fundamentales.
- **Transparencia algorítmica** y medidas específicas para la **protección de menores**.
- **Responsabilidad** exigible a quien use sistemas prohibidos por la Unión Europea.
- Un **marco de gobernanza** con autoridades de vigilancia del mercado. Para los sistemas no regulados por normativa sectorial —empleo, biometría, educación— la supervisión recae principalmente en la **AESIA** (Agencia Española de Supervisión de la Inteligencia Artificial), junto a la **AEPD** y el **CGPJ** según el ámbito, con la AESIA como punto de contacto único.
- Un **inventario de sistemas de IA** en el sector público estatal y la figura del **delegado de IA**, para reforzar la transparencia.
- Un **espacio de pruebas (sandbox) a escala nacional** operado por la AESIA, para que los proveedores puedan validar el cumplimiento antes de salir al mercado.

> El régimen sancionador no es simbólico: las infracciones se clasifican en muy graves, graves y leves, con multas de hasta **35 millones de euros o el 7 % del volumen de negocio** en los casos más graves, y hasta **500.000 euros o el 0,5 %** en los leves. Se contemplan reducciones por pronto pago y por adoptar medidas correctoras, y un trato específico para pymes y startups.
{: .prompt-warning }

## El compromiso de Microsoft: de los principios al método

Microsoft lleva años apoyándose en seis principios de IA responsable, y su compromiso va bastante más allá del enunciado. Los ha convertido en un marco operativo: un **Estándar de IA Responsable** que traslada esos principios a requisitos concretos de producto, un **Informe de Transparencia de IA Responsable** que publica cada año (el más reciente, de 2025), **notas de transparencia** por servicio y herramientas de gobernanza para que sus clientes hagan lo mismo. No es una declaración de intenciones: es un programa que atraviesa política, investigación e ingeniería. Puedes consultarlo en su página oficial de [principios y enfoque de IA responsable de Microsoft](https://www.microsoft.com/es-es/ai/principles-and-approach) (merece un vistazo si alguna vez te toca defender ante un cliente por qué la IA que despliegas es de fiar).

Lo relevante en este momento es que esos principios **no nacen como respuesta a la ley: la anticipan**. Lo que Microsoft venía defendiendo —personas al mando, sistemas explicables, protección de datos y equidad— es, en gran medida, lo que el Reglamento europeo y el proyecto de ley español empiezan a exigir. Puestos uno al lado del otro, la alineación es evidente:

| Principio de Microsoft | Qué significa | Con qué se alinea en la norma |
| --- | --- | --- |
| **Equidad** | Evitar sesgos y garantizar decisiones equitativas | Prohibición de clasificar con biometría por raza u orientación, y del *social scoring* |
| **Confiabilidad y seguridad** | Sistemas robustos que funcionan como se espera | Clasificación por nivel de riesgo del RIA y obligaciones para el alto riesgo |
| **Privacidad y seguridad** | Protección de datos y confidencialidad | Supervisión de la AEPD y protección reforzada de menores |
| **Inclusión** | Diseñar para todos, sin excluir a nadie | Prohibición de explotar vulnerabilidades por edad, discapacidad o situación |
| **Transparencia** | Explicar cómo y por qué actúa la IA | Transparencia algorítmica e inventario de sistemas de IA |
| **Responsabilidad** | Las personas al mando, no la tecnología | Supervisión humana obligatoria y régimen sancionador |

El mensaje de fondo es el mismo en ambos lados de la tabla: la persona sigue al mando y el sistema tiene que poder explicarse. La diferencia es que lo que era un compromiso voluntario de compañías como Microsoft empieza a ser, además, una obligación con consecuencias.

## Qué cambia en un proyecto real con Business Central y Copilot

Todo esto puede sonar lejano para los que nos dedicamos a implantar Business Central y a desplegar Copilot y agentes sobre procesos de negocio. Pero no lo es. Estos son los puntos donde lo veo aterrizar en proyectos concretos:

- **Supervisión humana en las decisiones sensibles.** Copilot es excelente redactando, resumiendo o proponiendo, pero no debería *cerrar el círculo* solo en aprobaciones de crédito, selección de proveedores o cualquier decisión que afecte a personas. El humano valida; la IA asiste.
- **Transparencia real, no un eslogan.** Hay que poder explicar qué hace una capacidad de IA, con qué datos trabaja y qué la dispara. Si desarrollas una extensión AL que llama a un modelo o montas un agente, deja documentado el flujo: entrada, tratamiento y salida.
- **Gobierno del dato y privacidad.** Qué datos entran en el modelo, qué permisos se aplican y qué sale del tenant. La protección de datos personales —y muy en especial la de menores— pasa a ser un requisito de diseño, no una revisión de última hora.
- **Trazabilidad.** Registrar las acciones relevantes de los agentes (qué hicieron y por qué) es lo que te permite responder cuando alguien pregunta "¿por qué el sistema decidió esto?".
- **Inventario de lo que tienes en marcha.** Es obligación para el sector público, pero es buena práctica para cualquiera: saber qué asistentes, agentes y automatizaciones con IA están vivos en tu organización y quién es responsable de cada uno.

> Empieza por clasificar cada caso de uso por su nivel de riesgo **antes** de desplegarlo. No es lo mismo un Copilot que resume un pedido que un sistema que decide sobre personas: el primero necesita buenas prácticas; el segundo, controles y supervisión explícita.
{: .prompt-tip }

## Mi recomendación práctica

No hace falta esperar a que el texto termine su tramitación. Si me preguntas por dónde empezar, esto es lo que yo haría ya:

1. **Clasifica** tus casos de uso de IA por riesgo.
2. **Define** dónde entra la supervisión humana y quién valida.
3. **Documenta** qué hace cada capacidad de IA y sobre qué datos.
4. **Revisa** el tratamiento de datos personales y el impacto en menores.
5. **Mantén** un inventario sencillo de sistemas y agentes de IA (un Excel te vale para empezar).
6. **Forma** a los equipos: la propia norma insiste en la concienciación.

## Conclusión

La confianza no es un freno a la innovación; es lo que la hace sostenible. La ley pone límites, sí, pero sobre todo establece reglas del juego claras, y eso es una buena noticia para quien trabaja con clientes y datos reales. En mi experiencia, los proyectos que integran estos principios desde el diseño **no van más lentos: van más seguros y, precisamente por eso, llegan más lejos**.

Adoptar la IA responsable ya no es solo lo correcto: empieza a ser también lo que toca por ley.

¿Y tú, cómo lo estás gestionando en tus proyectos? Me encantará leerte.

---

*Fuentes: [nota de prensa del Ministerio para la Transformación Digital y de la Función Pública (26/05/2026)](https://digital.gob.es/comunicacion/notas-prensa/mtdfp/2026/05/el-gobierno-aprueba-el-proyecto-de-ley-que-garantizara-una-super), [principios y enfoque de IA responsable de Microsoft](https://www.microsoft.com/es-es/ai/principles-and-approach) y el [Reglamento europeo de IA (AI Act)](https://artificialintelligenceact.eu/es/).*
