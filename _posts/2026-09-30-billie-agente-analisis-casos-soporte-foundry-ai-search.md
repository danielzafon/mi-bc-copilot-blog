---
title: "Billie, el nuevo compañero de soporte de BC: un agente que analiza incidencias con nuestro histórico de casos"
date: 2026-09-30 10:00:00 +0200
categories: [Inteligencia Artificial, Business Central]
tags: [business-central, soporte, azure-ai-search, microsoft-foundry, agentes, rag, copilot]
description: Cómo hemos creado en Microsoft Foundry un agente que clasifica incidencias de soporte y propone causa y resolución a partir de casos reales indexados en Azure AI Search.
image:
  path: 05-respuesta-agente.png
  alt: Respuesta del agente analista-casos en Microsoft Foundry con causa probable, resolución recomendada, evidencia y nivel de confianza
media_subpath: /assets/img/posts/billie-agente-analisis-casos-soporte-foundry-ai-search/
---

Hace poco se incorporó un nuevo compañero al departamento de soporte de Business Central. Se llama **Billie**, no toma café y no se va de vacaciones, pero se ha empapado de nuestros casos de BC de los dos últimos años.

Billie es un agente de IA que he construido en Microsoft Foundry. Su trabajo es sencillo de explicar: el técnico le pasa el problema que ha reportado el cliente y el agente lo clasifica, busca casos parecidos en nuestra base de conocimiento y propone una causa probable y una resolución basadas en lo que ya hicimos en el pasado.

En este artículo te cuento cómo está montado, qué reglas sigue y, sobre todo, dónde ha estado el trabajo de verdad: construir la base de conocimiento.

## El problema: el conocimiento está en los casos, pero nadie lo reutiliza

Cualquier departamento de soporte acumula años de incidencias resueltas. Lo malo es que ese conocimiento queda enterrado en tickets, en comentarios internos y en la memoria de quien lo resolvió. Cuando entra un caso nuevo, lo habitual es:

- Que el técnico empiece el análisis desde cero.
- Que pregunte a un compañero "¿esto no nos pasó ya con otro cliente?".
- Que dos técnicos den respuestas distintas al mismo problema.

La idea de Billie es acortar ese camino: antes de ponerte a investigar, consulta qué dice el histórico.

## Qué hace el agente

A partir del título y la descripción de un caso, Billie busca situaciones similares en la base de conocimiento corporativa y devuelve:

- El **problema detectado**.
- La **causa raíz más probable**.
- La **resolución aplicada** en casos anteriores.
- Los **casos similares** que ha usado como referencia.
- El **nivel de confianza** de la respuesta.

Está pensado para incidencias de Dynamics 365 Business Central, pero sus instrucciones también cubren Dataverse / Dynamics CRM, Microsoft 365, Power BI, portales B2B/PIM e integraciones con sistemas externos. El objetivo es reducir tiempos de análisis, dar respuestas más consistentes y aprovechar el conocimiento histórico del equipo.

### Las reglas que le he puesto

Lo más importante de las instrucciones no es lo que el agente debe hacer, sino lo que **no** debe hacer. Un agente de soporte que se inventa una causa es peor que no tener agente. Por eso las reglas obligatorias son muy estrictas:

- Utilizar **siempre** la base de conocimiento antes de responder.
- No inventar causas ni resoluciones.
- No utilizar conocimiento externo si no hay evidencia suficiente.
- Si los casos encontrados son contradictorios, justificarlo.
- Distinguir claramente entre problema, causa y resolución, y no confundir síntomas con causas raíz.
- No afirmar una causa como confirmada si la evidencia es débil.
- Si la información es insuficiente, pedir datos adicionales.

Además, le pido que empiece con una pequeña checklist (de 3 a 7 puntos) de los pasos que va a seguir, y que después de cada búsqueda indique si ha encontrado evidencia suficiente o qué necesita para continuar.

### Clasificación del caso

Antes de responder, el agente debe encuadrar la incidencia en **una** categoría:

| Categorías |  |  |
|---|---|---|
| Configuración | Datos | Desarrollo |
| Integración | Permisos | Formación |
| Producto estándar Microsoft | Infraestructura | Proceso |
| Extensión funcional | No determinado | |

"No determinado" es una opción válida, y está ahí a propósito: prefiero que lo reconozca a que fuerce una categoría.

### Formato de salida fijo

La respuesta siempre sigue la misma estructura en Markdown: clasificación, problema detectado, causa probable, resolución recomendada, evidencia utilizada, nivel de confianza e información adicional necesaria. Tener un formato fijo hace que el técnico sepa exactamente dónde mirar y que las respuestas sean comparables entre sí.

El nivel de confianza tampoco es libre; tiene criterios definidos:

| Confianza | Cuándo |
|---|---|
| **Alta** | Existen varios casos similares y coinciden causas y resoluciones. |
| **Media** | Hay pocos antecedentes o diferencias menores entre casos. |
| **Baja** | La evidencia es poco clara o contradictoria, o faltan datos clave. |

Y cuando no hay antecedentes suficientes, la respuesta está pautada: el agente indica que no ha encontrado evidencia fiable y pide más información (mensaje exacto de error, producto, versión, módulo, pasos para reproducir, sistema externo implicado o captura de pantalla).

Este es un ejemplo real de respuesta, sobre facturas electrónicas rechazadas por FACe por un problema de redondeo:

![Respuesta del agente con causa probable, resolución, evidencia y nivel de confianza](05-respuesta-agente.png){: w="1708" h="873" .shadow }
*El agente cita los casos similares en los que se apoya y marca la confianza como alta.*

Fíjate en que la resolución no sale de la nada: se apoya en tres casos anteriores con el mismo patrón. Y aun así, pide confirmar producto, versión y si hay alguna extensión implicada.

## Cómo está montado

### El agente en Microsoft Foundry

El agente (`analista-casos`) está creado en Microsoft Foundry con el modelo `gpt-4.1`. La pieza clave es la herramienta **Azure AI Search**, conectada al índice `busqueda-casos-rag`, que es de donde saca toda la evidencia.

![Agente analista-casos en Microsoft Foundry con la herramienta Azure AI Search](01-agente-foundry-herramienta-ai-search.png){: w="1442" h="870" .shadow }
*Configuración del agente en Foundry: instrucciones, modelo y herramienta Azure AI Search.*

> Billie se publica desde Foundry en Microsoft 365 como agente de motor personalizado. En ese escenario, la orquestación y la IA responsable (RAI) las gestiona quien construye el agente, no Microsoft 365 Copilot. Si quieres entender las implicaciones, revisa la [documentación de agentes de motor personalizado](https://learn.microsoft.com/en-us/microsoft-365-copilot/extensibility/overview-custom-engine-agent).
{: .prompt-info }

### La base de conocimiento en Azure AI Search

Antes de entrar en cómo la he montado, merece la pena explicar qué es Azure AI Search y por qué encaja en este escenario.

[Azure AI Search](https://learn.microsoft.com/es-es/azure/search/search-what-is-azure-search?tabs=indexing%2Cquickstarts) es un servicio de búsqueda totalmente gestionado en Azure pensado para conectar tus datos con la IA. En la práctica, permite que agentes y modelos de lenguaje (LLM) respondan apoyándose en tu propio contenido, en lugar de en su conocimiento general. Es la pieza que hace posible el patrón **RAG** (*Retrieval-Augmented Generation*): primero se recuperan los documentos relevantes y después el modelo genera la respuesta a partir de ellos.

Los conceptos básicos que necesitas conocer:

- **Servicio de búsqueda**: el recurso de Azure que creas (en nuestro caso, `buscador-casos`). Dentro puede haber varios índices.
- **Índice**: donde se almacena el contenido que se puede buscar, organizado en documentos con campos. En nuestro caso, cada documento es un caso de soporte normalizado.
- **Tipos de consulta**: admite búsqueda de texto completo (por palabras clave), búsqueda vectorial (por similitud de significado), híbrida (ambas a la vez, combinando resultados) y multimodal.

La búsqueda híbrida es especialmente útil en un escenario de soporte. La parte vectorial encuentra casos conceptualmente parecidos aunque el cliente describa el problema con otras palabras. La parte de texto completo es más precisa con términos exactos, como un código de error, el nombre de un campo o una plataforma concreta.

Además, Azure AI Search se integra de forma nativa con Microsoft Foundry: basta con añadirlo como herramienta del agente y seleccionar el índice. También es la base de [Foundry IQ](https://learn.microsoft.com/es-es/azure/foundry/agents/concepts/what-is-foundry-iq), la capa de conocimiento gestionada de Foundry; por eso en el portal verás el servicio etiquetado como "servicio Search (Foundry IQ)".

Nuestra base de conocimiento es un servicio de Azure AI Search con un índice construido a partir del histórico de soporte. El índice que usa el agente, `busqueda-casos-rag`, contiene actualmente 1.164 documentos.

![Índices del servicio Azure AI Search](02-indices-azure-ai-search.png){: w="1872" h="825" .shadow }
*Índices del servicio de búsqueda; el agente trabaja sobre busqueda-casos-rag.*

La conexión entre Foundry y el servicio de búsqueda está hecha con **clave de API**:

![Crear conexión con Azure AI Search usando clave de API](03-conexion-clave-api.png){: w="706" h="381" .shadow }
*Al añadir la herramienta, se selecciona el servicio de búsqueda y el tipo de autenticación.*

La clave la obtienes desde el propio servicio de Azure AI Search, en **Seguridad y redes > Claves**:

![Pantalla de claves del servicio Azure AI Search](04-claves-servicio-search.png){: w="1363" h="888" .shadow }
*Claves de administrador y de consulta del servicio de búsqueda.*

> Una clave de API concede acceso sin las restricciones de un rol. Microsoft recomienda autenticación con Microsoft Entra ID para tener un control de acceso más fino. Si vas por ese camino, recuerda que Foundry debe tener asignaciones de rol sobre el servicio de Azure AI Search.
{: .prompt-warning }

## El verdadero reto: construir el índice

Montar el agente en Foundry es cuestión de un rato. Lo que de verdad ha costado es la base de conocimiento. El proceso ha sido este:

1. **Extracción.** Saqué por API todos los casos que quería usar para alimentar al agente. Para empezar, los de Business Central de los dos últimos años, con todos sus comentarios, tanto internos como de cliente.
2. **Normalización con Copilot.** Procesé esa información con Copilot para estructurar cada caso en tres piezas: problema, causa y solución aplicada.
3. **Filtrado.** Como le pedí que no inventase nada que no estuviera identificado en el caso, en muchos de ellos no fue capaz de determinar las tres piezas. Esos casos los he excluido de momento y los incorporaré en futuras iteraciones.
4. **Indexación.** Con los casos normalizados creé el índice `busqueda-casos-rag`, que es el que consulta el agente.

Entre el paso 2 y el 4 hubo muchos intentos y bastantes tokens consumidos. Pero la decisión de excluir casos en lugar de rellenar huecos es, probablemente, la más importante de todo el proyecto: la calidad de las respuestas de Billie depende directamente de la calidad de lo que hay en el índice. Un caso mal normalizado es una respuesta equivocada con apariencia de fiable.

> Si vas a montar algo parecido, dedica el esfuerzo a la normalización de los casos, no al prompt del agente. Un ticket con veinte comentarios de ida y vuelta no es conocimiento reutilizable hasta que alguien (o algo) extrae qué pasó, por qué y cómo se resolvió.
{: .prompt-tip }

## Permisos para el equipo

Para que los técnicos puedan trabajar con el agente, cada uno necesita acceso al proyecto de Foundry. En nuestro caso, les he asignado el rol **Foundry User** desde **Control de acceso (IAM)** del proyecto (más abajo te explico por qué este y no otro):

![Asignaciones del rol Foundry User en el proyecto de Foundry](06-permisos-iam-foundry-user.png){: w="1762" h="873" .shadow }
*Asignaciones del rol Foundry User en el control de acceso del proyecto.*

Algunos detalles que conviene conocer (los tienes en la [documentación de RBAC de Microsoft Foundry](https://learn.microsoft.com/es-es/azure/foundry/concepts/rbac-foundry)):

- Los roles de Foundry se han renombrado recientemente: **Foundry User** es el antiguo *Usuario de Azure AI*. Puedes encontrarte los dos nombres mientras se completa el cambio.
- **Foundry User** permite crear y probar agentes dentro del proyecto. Para quien solo necesita **usar** agentes, sin crearlos ni modificarlos, la documentación describe el rol **Foundry Agent Consumer**, con privilegios mínimos.
- Para **publicar** agentes necesitas como mínimo el rol **Foundry Project Manager**.

> En mi tenant el rol **Foundry Agent Consumer** no está disponible, y por eso he asignado **Foundry User** a los técnicos. Revisa si lo tienes en el tuyo antes de decidir qué rol asignar.
{: .prompt-warning }

## Habilitar el agente para los usuarios en el Centro de administración de Microsoft 365

Publicar el agente desde Foundry no es el último paso. Una vez publicado, aparece en el Centro de administración de Microsoft 365, pero **un administrador tiene que habilitarlo** para que los usuarios puedan usarlo.

El proceso es el siguiente:

1. En el Centro de administración de Microsoft 365, entra en **Agentes > Todos los agentes**.
2. Localiza el agente (en nuestro caso, *Billie - Técnico soporte*, con plataforma Microsoft Foundry) y ábrelo.
3. En la pestaña **Usuarios**, sección **Disponibles para**, elige quién puede instalarlo: **Todos los usuarios**, **No hay usuarios** o **Usuarios o grupos específicos**.
4. Si eliges usuarios o grupos específicos, búscalos, añádelos y pulsa **Guardar**.

![Disponibilidad del agente en el Centro de administración de Microsoft 365](07-admin-center-disponibilidad-agente.png){: w="1889" h="854" .shadow }
*Configuración de disponibilidad de Billie en Agentes > Todos los agentes.*

Nosotros lo hemos limitado a usuarios concretos del equipo de soporte. Te recomiendo empezar así: un grupo reducido que lo use en casos reales y te dé feedback antes de abrirlo a más gente.

> Tenlo en cuenta al planificar el despliegue. Si quien construye el agente no es administrador de Microsoft 365, los técnicos no podrán usarlo hasta que alguien lo habilite desde el Centro de administración.
{: .prompt-info }

## Impacto en el día a día de soporte

Billie no sustituye al técnico. Lo que hace es darle un punto de partida: en lugar de analizar desde cero, arranca con una hipótesis apoyada en casos reales, una categoría y una lista de datos que conviene pedir al cliente. Eso tiene tres efectos claros:

- **Menos tiempo de diagnóstico** en incidencias que ya hemos visto antes.
- **Respuestas más consistentes** entre técnicos, porque todos parten de la misma evidencia.
- **Mejor incorporación de gente nueva**, que accede desde el primer día al histórico del equipo sin depender de preguntar a los veteranos.

Y un efecto secundario interesante: los casos en los que Copilot no pudo identificar problema, causa y solución dicen mucho de cómo documentamos. Si un caso cerrado no deja claro por qué pasó y cómo se resolvió, el problema no es de la IA.

## Conclusión

Un agente de análisis de casos es de los usos de IA con retorno más claro en un departamento de soporte, porque trabaja sobre algo que ya tienes: tu histórico. Pero el valor no está en el agente, sino en la base de conocimiento que hay detrás.

Mi recomendación si quieres montar algo similar: empieza por un ámbito acotado (en nuestro caso, BC y dos años de casos), normaliza con criterio estricto aunque eso deje fuera parte del histórico, obliga al agente a citar su evidencia y a declarar su nivel de confianza, e itera. El siguiente paso para nosotros es recuperar los casos que se quedaron fuera y ampliar a otros productos.

## Lo que estoy probando ahora

Billie no es el final del camino. En paralelo tengo dos líneas de trabajo abiertas:

- **El mismo agente en Copilot Studio.** Estoy construyendo un agente equivalente en Copilot Studio para comparar ambos enfoques: qué ofrece cada plataforma, cómo se comporta con la misma base de conocimiento y qué diferencias hay en la configuración y el despliegue. Una de las comparativas que más me interesa es el **coste**: en Azure el agente funciona con pago por uso, mientras que Copilot Studio funciona con créditos, así que quiero ver cuánto cuesta en cada caso el mismo volumen de consultas.
- **Un flujo agéntico para el primer cribado.** Estoy probando a integrar Billie en un flujo agéntico para que, cuando se cree un caso, se envíe automáticamente al agente. Así el primer cribado ya está hecho y, cuando el técnico coge el caso, tiene el análisis incorporado: clasificación, causa probable, casos similares y datos que falta pedir al cliente.

Si esta segunda línea funciona, el técnico ya no tendrá que acordarse de consultar a Billie: el análisis le estará esperando en el caso.

Seguramente haya otras formas de hacerlo, y probablemente mejores. Lo que te comparto aquí es en lo que estoy trabajando: mi experiencia y los problemas que me he ido encontrando por el camino. Si te sirve de inspiración para montar algo parecido, habrá merecido la pena.

## Enlaces de interés

- [¿Qué es Azure AI Search?](https://learn.microsoft.com/es-es/azure/search/search-what-is-azure-search?tabs=indexing%2Cquickstarts)
- [¿Qué es Foundry IQ?](https://learn.microsoft.com/es-es/azure/foundry/agents/concepts/what-is-foundry-iq)
- [Control de acceso basado en rol para Microsoft Foundry](https://learn.microsoft.com/es-es/azure/foundry/concepts/rbac-foundry)
- [Agentes de motor personalizado en Microsoft 365 Copilot](https://learn.microsoft.com/en-us/microsoft-365-copilot/extensibility/overview-custom-engine-agent)
