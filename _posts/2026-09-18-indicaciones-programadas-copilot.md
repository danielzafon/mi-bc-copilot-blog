---
title: "Indicaciones programadas en Copilot: automatiza los prompts que repites cada día"
date: 2026-09-18 10:00:00 +0200
categories: [Copilot, Productividad]
tags: [copilot, microsoft-365, productividad, automatizacion, ia]
description: Con las indicaciones programadas de Microsoft 365 Copilot puedes lanzar tus prompts habituales sin abrir un chat nuevo cada vez; cómo configurarlas paso a paso y qué merece la pena programar en el día a día.
image:
  path: 01-portada.png
  alt: Panel de indicaciones programadas de Microsoft 365 Copilot con tres programaciones activas; imputación de jornada, dashboard diario y resumen semanal de logros
media_subpath: /assets/img/posts/indicaciones-programadas-copilot/
---

Cada mañana le escribo prácticamente el mismo prompt a Copilot: que revise mis reuniones y correos y me arme un plan para el día. Cada tarde, otro para imputar la jornada de ayer. Y cada viernes, uno más para que me saque un resumen de lo que he conseguido esa semana. Durante meses los he ido lanzando a mano, abriendo un chat nuevo y escribiendo el mismo texto una y otra vez.

Microsoft 365 Copilot tiene una funcionalidad pensada exactamente para esto: las **indicaciones programadas**. En vez de repetir el prompt, lo escribes una vez, le dices cuándo tiene que ejecutarse y Copilot lo lanza solo, sin que tengas que abrir el chat. Está documentada en el soporte de Microsoft —[programar tus indicaciones más usadas](https://support.microsoft.com/es-es/microsoft-365-copilot/schedule-your-most-used-copilot-prompts)—, pero es una de esas opciones que están a un par de clics y que casi nadie usa.

## Dónde se activa

Se accede desde el mismo menú de los tres puntos donde está la configuración de Copilot, en la entrada **Indicaciones programadas**.

![Menú de opciones de Microsoft 365 Copilot con la entrada Indicaciones programadas remarcada](02-menu-indicaciones-programadas.png){: w="1911" h="784" .shadow }
*El acceso está en el menú de opciones de Copilot, junto a Configuración y Páginas recientes.*

Al entrar ves la lista de programaciones que ya tienes activas —en mi caso, tres— con la fecha y hora de la próxima ejecución de cada una.

![Panel de indicaciones programadas con tres programaciones activas y su próxima ejecución](01-portada.png){: w="974" h="611" .shadow }
*Mis tres indicaciones programadas activas: imputación de jornada, dashboard diario y resumen semanal de logros.*

## Crear una programación nueva

El botón **Nuevo**, arriba a la derecha, abre el formulario de creación.

![Panel de administrar programación con el botón Nuevo remarcado](03-nuevo-programacion.png){: w="965" h="642" .shadow }

El formulario tiene dos partes. La primera es la propia indicación: el texto exacto que le pasarías a Copilot si abrieras un chat normal. Por ejemplo, para un dashboard semanal:

```text
Dashboard de logros y victorias de la semana

Revisa mis reuniones, chats, correos electrónicos, documentos, tareas y grabaciones de esta semana.

No te limites a resumir la actividad reciente. Analiza el impacto y el valor aportado.

Identifica y destaca:
- Proyectos estratégicos en los que he trabajado.
- Implantaciones Business Central en curso.
- Clientes con mayor avance esta semana.
- Decisiones importantes tomadas.
- Reuniones relevantes y acuerdos alcanzados.
- Documentación creada o actualizada.
- Planificaciones, convocatorias o iniciativas puestas en marcha.
- Problemas desbloqueados o riesgos mitigados.
- Coordinación de equipos o asignación de recursos.
- Actividades de liderazgo, gestión de proyectos y dirección funcional.

Presta especial atención a cualquier cliente o proyecto que haya tenido actividad significativa esta semana.

No clasifiques por correos o reuniones individuales. Agrupa por proyectos y logros.

Devuelve:
- Resumen ejecutivo de la semana.
- Top 5 victorias más importantes.
- Proyectos con mayor avance.
- Contribuciones de liderazgo y gestión.
- Logros técnicos y funcionales.
- Mensaje final motivador valorando el impacto de mi trabajo.

Prioriza siempre impacto, avance y valor generado frente a volumen de actividad.
```

![Formulario de creación de una programación con el cuadro de indicación remarcado](04-indicacion-texto.png){: w="1019" h="768" .shadow }
*El cuadro de indicación tiene scroll y admite un prompt tan largo como el de arriba, aunque en la captura solo se vea el arranque.*

La segunda parte es la programación en sí: cuándo empieza, con qué frecuencia se repite —diaria, semanal o mensual— y hasta cuándo.

![Sección de programación con fecha de inicio, frecuencia y fecha de fin remarcadas](05-programar-frecuencia.png){: w="900" h="743" .shadow }
*Para una frecuencia semanal eliges también el día concreto en el que se ejecuta.*

Justo debajo hay una casilla que yo marcaría, al menos al principio: **recibir un correo cuando la indicación se haya ejecutado**.

> No la veo imprescindible para siempre, pero sí al arrancar: te sirve para comprobar que la indicación se ejecuta de verdad y para acordarte de que la tienes programada mientras no forma parte de tu rutina. Una vez que la consulta del resultado ya sea rutina —por ejemplo, revisas el dashboard nada más llegar, sin que nadie te avise—, puedes desactivarla sin perder nada.
{: .prompt-tip }

![Casilla para recibir un correo electrónico cuando la indicación se haya ejecutado](06-notificacion-email.png){: w="900" h="743" .shadow }

## Pruébala a mano antes de dejarla sola

Con la programación ya creada, no hace falta esperar a su primera ejecución para saber si funciona: en el menú de los tres puntos de cada programación tienes **Ejecutar ahora**.

![Menú de los tres puntos de una programación con las opciones Ejecutar ahora, Editar programación, Pausar y Eliminar](09-gestionar-programacion.png){: w="1008" h="648" .shadow }
*El mismo menú te deja lanzarla a mano, editarla, pausarla o eliminarla.*

> Aquí "funciona" no es solo que se ejecute sin error: es que te devuelva lo que esperas. Y eso da por hecho un trabajo previo —partimos de que la indicación ya la llevas lanzando a mano de forma periódica y tienes comprobado que el resultado es el que quieres—. Programarla no sustituye ese rodaje, solo automatiza cuándo se dispara; si todavía no la tienes depurada, pruébala primero en un chat normal, no en la programación.
{: .prompt-tip }

En ese mismo menú tienes también **Editar programación**, para ajustarla cuando cambie lo que necesitas, y **Pausar** o **Eliminar**, para cuando deje de aportarte valor.

## El aviso por correo y el historial

Cuando llega la hora programada, Copilot ejecuta la indicación igual que si la hubieras escrito tú y, si activaste el aviso, te llega un correo como este:

![Correo de Microsoft Copilot avisando de que una indicación programada se ha completado](07-email-recibido.png){: w="1185" h="770" .shadow }
*El correo no trae el resultado en el cuerpo: enlaza directamente a la conversación para que la consultes en Copilot.*

Y hay otro punto a favor de esta funcionalidad: cada ejecución programada **no desaparece**, queda guardada como una conversación más en tu historial de chats, exactamente igual que si la hubieras lanzado a mano.

![Historial de chats de Copilot con las conversaciones generadas por las indicaciones programadas](08-historial-chats.png){: w="1652" h="912" .shadow }
*Las tres conversaciones generadas por mis indicaciones programadas, junto al resto del historial de chats.*

## Qué tengo programado yo

En mi caso tengo tres indicaciones activas, cada una resolviendo un hueco distinto del día o de la semana:

- **Planificación de la jornada**, cada mañana, para que Copilot revise reuniones, correos y tareas pendientes y me proponga un plan del día.
- **Imputación de la jornada de ayer**, para no dejar que se acumulen varios días de trabajo sin repartir entre clientes y proyectos.
- **Resumen de logros y victorias de la semana**, en positivo, cada viernes, para cerrar la semana viendo lo que sí ha salido bien y no solo lo que ha quedado pendiente.

El de la mañana y el de la imputación son un par de líneas; el de los viernes, como acabas de ver, es bastante más largo porque le pido que agrupe, priorice y valore el impacto, no que se limite a listar actividad. En los tres casos cambia lo mismo: ya no tengo que acordarme de lanzarlas, se ejecutan solas y me avisan cuando están listas.

## Mi recomendación práctica

1. **Programa solo lo que ya repites de memoria.** Si es un prompt que lanzas con el mismo texto más de una vez por semana, es candidato claro. Si cambia cada vez según lo que necesitas en el momento, no lo programes: perderías precisamente lo que le da valor, que es poder adaptarlo a la situación.
2. **Activa el aviso por correo al principio, no para siempre.** Te sirve para comprobar que la indicación se ejecuta y para acordarte de que la tienes programada; en cuanto consultar el resultado ya forme parte de tu rutina, puedes desactivarlo.
3. **Pruébala con Ejecutar ahora en cuanto la crees.** Así compruebas que devuelve lo que esperas antes de dejar que se dispare sola.
4. **Empieza por una sola indicación**, la que más repitas ahora mismo a mano. En cuanto veas el resultado en tu bandeja de entrada un par de veces sin haber movido un dedo, es fácil que te animes a programar la siguiente.
