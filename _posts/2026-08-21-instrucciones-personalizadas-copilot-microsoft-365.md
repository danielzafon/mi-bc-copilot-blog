---
title: "Personalización de Microsoft 365 Copilot: cuatro palancas para que deje de responder en genérico"
date: 2026-08-21 10:00:00 +0200
categories: [Copilot, Productividad]
tags: [copilot, microsoft-365, personalizacion, productividad, ia]
description: Copilot no tiene por qué responder igual a todo el mundo; instrucciones personalizadas, perfil de trabajo, recuerdos guardados e historial de chats son las cuatro palancas que lo cambian. Qué hace cada una y cuál merece la pena activar de verdad.
image:
  path: 01-portada.png
  alt: Panel de personalización de Microsoft 365 Copilot con instrucciones personalizadas, perfil de trabajo, recuerdos guardados e historial de chats
media_subpath: /assets/img/posts/instrucciones-personalizadas-copilot-microsoft-365/
---

Por defecto, Microsoft 365 Copilot responde igual a cualquier usuario del tenant. Mismo tono, mismo formato, cero contexto sobre quién eres, qué haces o cómo te gusta recibir la información. Eso cambia bastante cuando entras en **Configuración > Personalización** y activas las cuatro palancas que Copilot pone a tu disposición: instrucciones personalizadas, perfil de trabajo, recuerdos guardados e historial de chats.

La mayoría de los usuarios con los que hablo no ha entrado nunca en ese panel. No es una funcionalidad escondida —está a un par de clics—, pero como no aparece en medio de una tarea, nadie la busca. Y es una pena, porque configurarla bien es la diferencia entre un asistente que te da respuestas correctas pero anodinas y uno que ya sabe qué esperas de él antes de que se lo pidas.

## Dónde se configura

Se llega desde el menú de opciones, arriba a la derecha de la ventana de Copilot, en la entrada **Configuración**.

![Menú de opciones de Microsoft 365 Copilot con la entrada a Configuración remarcada](02-menu-configuracion.png){: w="1672" h="790" .shadow }
*El acceso a la configuración de Copilot está en el menú de los tres puntos, no en un ajuste de la propia conversación.*

Dentro, la pestaña **Personalización** agrupa los cuatro bloques que voy a repasar, cada uno con su propio interruptor y su propio enlace de detalle:

![Panel de personalización de Microsoft 365 Copilot con instrucciones personalizadas, perfil de trabajo, recuerdos guardados e historial de chats](01-portada.png){: w="991" h="712" .shadow }
*Cuatro bloques independientes, cada uno con su interruptor: puedes activar unos y desactivar otros según lo que te interese.*

## Instrucciones personalizadas

Es la palanca más directa de las cuatro: un cuadro de texto libre donde le dices a Copilot cómo quieres que sean sus respuestas, y que se aplica a **todas** las conversaciones, no solo a la que tienes abierta.

![Pantalla de instrucciones personalizadas de Copilot con el cuadro de texto y sugerencias predefinidas](03-instrucciones-personalizadas.png){: w="841" h="691" .shadow }
*Además de escribir tu propio texto, hay sugerencias predefinidas que se añaden con un clic: dar comentarios sinceros, priorizar a tu jefe/a, destacar decisiones en las actas, ceñirse a fuentes internas...*

Lo interesante no es solo el espacio para escribir lo que quieras, sino las sugerencias que Microsoft ofrece de partida: *"proporcionar comentarios sinceros"*, *"usa un lenguaje claro y sencillo"*, *"destaca las decisiones en las actas de la reunión"*, *"usa viñetas para los resúmenes"*. Son un buen punto de partida porque apuntan a lo que de verdad marca la diferencia: no tanto el tema sobre el que te ayuda Copilot, sino **el formato y el criterio** con el que responde.

Mi recomendación aquí es ser concreto y verificable. "Sé más útil" no sirve de instrucción porque Copilot no tiene forma de comprobar si lo está cumpliendo. "Cuando resumas una reunión, separa siempre decisiones tomadas de próximas acciones" sí, porque es una instrucción que se puede seguir de forma consistente.

## Perfil de trabajo

Este bloque no lo rellenas tú: Copilot toma el **puesto y el administrador de tu cuenta de Microsoft 365** directamente de tu perfil corporativo.

![Datos del perfil de trabajo con el puesto extraído automáticamente de la cuenta de Microsoft 365](04-perfil-trabajo.png){: w="879" h="691" .shadow }
*El dato viene de tu perfil de Microsoft 365; si algo no es correcto, se corrige ahí, no en la configuración de Copilot.*

El matiz importante es que **no hay validación en Copilot**: si tu puesto está mal en el directorio corporativo, o lleva meses sin actualizarse tras un cambio de rol, Copilot va a personalizar sus respuestas con ese dato incorrecto sin avisarte de nada.

> Si vas a apoyarte en esta palanca —y sobre todo si vas a usar sugerencias como "prioriza a mi jefe/a" en las instrucciones personalizadas—, revisa antes de que el perfil de Microsoft 365 esté al día.
{: .prompt-warning }

## Recuerdos guardados

Es la palanca más silenciosa y, para mí, la más interesante: Copilot va infiriendo preferencias de tus conversaciones y las guarda como recuerdos, sin que tengas que pedírselo.

![Lista de recuerdos guardados por Copilot a partir de conversaciones anteriores](05-recuerdos-guardados.png){: w="869" h="701" .shadow }
*Cada línea es una inferencia sacada de una conversación real: preferencias de formato, reglas de negocio propias e incluso comandos personalizados.*

En la lista se ven tres tipos de recuerdos, y conviene distinguirlos porque no todos aportan lo mismo:

- **Preferencias de formato**, como mostrar el detalle de tareas con columnas en un estilo concreto.
- **Reglas de negocio propias del usuario**, como qué debe considerarse al imputar la jornada diaria o cómo tratar las llamadas no programadas de Teams.
- **Comandos personalizados**, frases concretas ("dame mis tareas del día", "ejecuta este") que Copilot ha aprendido a interpretar como disparadores de una acción determinada.

Este último tipo es el que más valor tiene en el día a día: conviertes frases cortas en atajos hacia flujos de trabajo que, de otra forma, tendrías que explicar desde cero cada vez.

La otra cara es que estos recuerdos se acumulan solos y con el tiempo pueden quedar desfasados —una regla que aplicaba a un proyecto que ya cerraste, un comando que ya no usas—.

> El botón **"Eliminar todos los recuerdos"** está para eso —no hay opción de borrarlos uno a uno, es todo o nada—: conviene revisarlos de vez en cuando, no solo dejarlos crecer.
{: .prompt-tip }

## Historial de chats y búsqueda en la web

Nos quedan dos palancas más rápidas de explicar, aunque no tienen captura propia en este repaso porque están un peldaño más abajo en la configuración.

El **historial de chats** —marcado como *(Frontier)*, es decir, todavía en fase experimental dentro del programa Frontier de Microsoft 365 Copilot— permite que Copilot use conversaciones anteriores completas, no solo los recuerdos ya destilados, para personalizar una respuesta nueva. Es un nivel de contexto más amplio que los recuerdos guardados, pero también menos controlado: tú no decides qué se queda, como sí haces al escribir una instrucción o al borrar un recuerdo puntual.

La **búsqueda en la web**, que se activa o desactiva desde la pestaña **Fuentes** de la misma configuración, decide si Copilot puede salir a internet para dar respuestas más actuales o se queda limitado a tus datos corporativos y a lo que ya sabe. No es personalización en sentido estricto, pero forma parte del mismo panel de control sobre qué puede usar Copilot para responderte.

## Mi recomendación práctica

Si no has tocado nunca esta configuración, este es el orden que yo seguiría:

1. **Empieza por las instrucciones personalizadas.** Es la palanca de mayor control y menor esfuerzo: cinco minutos escribiendo dos o tres instrucciones concretas cambian el tono y el formato de todas tus respuestas a partir de ahí.
2. **Revisa que el perfil de trabajo esté correcto** en tu cuenta de Microsoft 365 antes de apoyarte en él, porque Copilot lo da por bueno sin cuestionarlo.
3. **Deja que los recuerdos se construyan solos, pero revísalos cada cierto tiempo.** Son los que más aportan a medio plazo —sobre todo los comandos personalizados—, pero también los que más ruido acumulan si no los limpias.
4. **Activa historial de chats y búsqueda web con criterio, no por defecto.** Son las dos palancas que menos controlas tú y las que más contexto —y más datos— ponen en juego; en un entorno corporativo, eso entra de lleno en la conversación de gobernanza de la IA que ya tratamos por aquí. Antes de activarlas sin más, vale la pena saber qué datos entran y quién los ve.

Ninguna de las cuatro palancas es complicada de configurar. Lo único que hace falta es entrar una vez, dedicarle diez minutos y no dar por hecho que Copilot ya sabe quién eres.
