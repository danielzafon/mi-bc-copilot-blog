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

> Dashboard de logros y victorias de la semana. Revisa mis reuniones, chats, correos electrónicos, documentos, tareas y grabaciones de esta semana.

![Formulario de creación de una programación con el cuadro de indicación remarcado](04-indicacion-texto.png){: w="1019" h="768" .shadow }
*El cuadro de indicación admite el mismo nivel de detalle que un prompt escrito a mano en el chat.*

La segunda parte es la programación en sí: cuándo empieza, con qué frecuencia se repite —diaria, semanal o mensual— y hasta cuándo.

![Sección de programación con fecha de inicio, frecuencia y fecha de fin remarcadas](05-programar-frecuencia.png){: w="900" h="743" .shadow }
*Para una frecuencia semanal eliges también el día concreto en el que se ejecuta.*

Justo debajo hay una casilla que yo marcaría, al menos al principio: **recibir un correo cuando la indicación se haya ejecutado**.

> No la veo imprescindible para siempre, pero sí al arrancar: te sirve para comprobar que la indicación se ejecuta de verdad y para acordarte de que la tienes programada mientras no forma parte de tu rutina. Una vez que la consulta del resultado ya sea rutina —por ejemplo, revisas el dashboard nada más llegar, sin que nadie te avise—, puedes desactivarla sin perder nada.
{: .prompt-tip }

![Casilla para recibir un correo electrónico cuando la indicación se haya ejecutado](06-notificacion-email.png){: w="900" h="743" .shadow }

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

Ninguna de las tres es especialmente compleja como prompt suelto. Lo que cambia es que ahora no tengo que acordarme de lanzarlas: se ejecutan solas y me avisan cuando están listas.

## Mi recomendación práctica

1. **Programa solo lo que ya repites de memoria.** Si es un prompt que lanzas con el mismo texto más de una vez por semana, es candidato claro. Si cambia cada vez según lo que necesitas en el momento, no lo programes: perderías precisamente lo que le da valor, que es poder adaptarlo a la situación.
2. **Activa el aviso por correo al principio, no para siempre.** Te sirve para comprobar que la indicación se ejecuta y para acordarte de que la tienes programada; en cuanto consultar el resultado ya forme parte de tu rutina, puedes desactivarlo.
3. **Empieza por una sola indicación**, la que más repitas ahora mismo a mano. En cuanto veas el resultado en tu bandeja de entrada un par de veces sin haber movido un dedo, es fácil que te animes a programar la siguiente.
