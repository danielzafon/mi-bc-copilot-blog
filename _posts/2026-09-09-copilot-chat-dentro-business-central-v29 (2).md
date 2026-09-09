---
title: "Copilot Chat por fin dentro de Business Central"
date: 2026-09-09 00:00:00 +0200
categories: [Business Central, Copilot]
tags: [business-central, copilot, asistente-virtual, novedades]
description: La vista previa pública de la 2026 release wave 2 (Update 29.0) trae un chat de Microsoft Copilot rediseñado e integrado en Business Central; así es el cambio y qué implica para partners y usuarios.
image:
  path: 01-copilot-chat-nuevo-v29.png
  alt: Nuevo panel de chat de Microsoft Copilot integrado en Business Central v29
media_subpath: /assets/img/posts/copilot-chat-dentro-business-central-v29/
---

Microsoft anunció el 3 de septiembre que la vista previa pública de la 2026 release wave 2 (Update 29.0) de Business Central ya se está desplegando a nivel global, lo que da a los partners la primera oportunidad de explorar lo que viene antes de la disponibilidad general. De toda la lista de novedades, hay una que me ha llamado la atención por encima del resto: el chat de Microsoft Copilot pasa a integrarse directamente en Business Central, sustituyendo al panel que conocíamos desde la v28.

## El problema del chat "heredado"

Si has usado Copilot en Business Central hasta ahora, conoces el panel: un cuadro lateral con tres accesos ("Buscar", "Explicar y guiar", "Preguntar") que funcionaba, pero que se sentía como un añadido más que como parte de la experiencia. No compartía ni el aspecto ni los patrones de interacción del resto del ecosistema Microsoft Copilot, así que cada vez que un usuario saltaba de Outlook o Teams a Business Central tenía que reaprender cómo pedirle algo al asistente.

Esto no es un problema menor en proyectos de adopción de IA: cuanta más fricción hay entre aplicaciones, menos constante es el uso, y más cuesta que el equipo del cliente interiorice Copilot como una herramienta de trabajo diaria en lugar de una curiosidad puntual.

## Qué cambia con la v29

La nueva experiencia de chat moderniza el panel, simplifica la navegación y adopta los mismos patrones de interacción que ya se usan en el resto de Microsoft Copilot. En la práctica, el chat dentro de Business Central deja de ser una pieza aislada y pasa a comportarse como una extensión más del asistente que el usuario ya conoce de otras aplicaciones de Microsoft 365.

![Antes: panel de chat heredado en Business Central v28](02-copilot-chat-anterior-v28.png){: w="1821" h="909" .shadow }
_El panel de chat que conocíamos hasta la v28_

![Después: nueva experiencia de chat de Microsoft Copilot en Business Central v29](01-copilot-chat-nuevo-v29.png){: w="1853" h="852" .shadow }
_La nueva experiencia de chat de Microsoft Copilot en v29_

El cambio visual es evidente, pero lo relevante es que ya no hace falta reaprender nada: el Copilot que ya usas en Outlook o en Teams se comporta igual dentro de Business Central.

> Cuando escribí esto por primera vez tuve que montar un sandbox de Estados Unidos para verlo, porque en mi entorno de España el chat nuevo todavía no aparecía. Unos días después ya se ha desplegado también aquí, y la captura de arriba es de mi propio BC29 en español. Si a ti todavía no te sale, dale unos días: el despliegue va por regiones y no llega a todas a la vez.
{: .prompt-tip }

Para mí es un salto de calidad muy grande, y no es un cambio aislado: va en línea con la unificación que Microsoft lleva tiempo empujando para que el chat de Copilot sea el mismo, con la misma cara y el mismo comportamiento, en todas sus herramientas.

Y no se queda solo en el aspecto: en el menú superior del chat aparecen capacidades que hasta ahora solo tenías en el Copilot de Microsoft 365 fuera de Business Central. Puedes elegir el ámbito de búsqueda entre tu organización o la web (los iconos de maletín y globo), y hay un selector de modelo con tres opciones: Automático, GPT y Claude.

![Menú del chat de Microsoft Copilot en Business Central v29 con el selector de modelo y el ámbito de búsqueda](03-copilot-chat-menu-modelo.png){: w="729" h="742" .shadow }
_Selector de modelo (Automático, GPT, Claude) y ámbito de búsqueda (organización o web) en el menú del chat_

A esto se suman opciones que hasta ahora no existían dentro de Business Central: abrir la conversación en la propia aplicación de M365 Copilot, consultar páginas recientes, programar mensajes o enviar comentarios directamente desde el chat. Ya no es una versión reducida de Copilot metida a presión en Business Central; es el mismo Copilot, con todo lo que trae, aplicado a tus datos.

## Licencias: quién ve qué

La función está habilitada para todos los usuarios con licencia de Business Central, sin coste adicional. Pero hay una capa extra para quien tiene licencia de Microsoft Copilot: acceso a Work IQ y a capacidades ampliadas del chat. Además, el chat de Microsoft Copilot también queda incluido para los clientes empresariales de Microsoft 365 que cumplan los requisitos de licencia correspondientes.

Para un proyecto de implantación esto tiene una lectura práctica: el salto de experiencia lo nota todo el mundo desde el primer día, pero el valor añadido (Work IQ, capacidades ampliadas) solo se materializa si el cliente ya tiene o contrata licencia de Copilot. Es un buen argumento para revisar, en la fase de discovery, si conviene incluir esa licencia en el alcance del proyecto.

## Cómo probarlo ya

Para ver la nueva experiencia necesitas un entorno de acceso anticipado con la versión 29, y conviene que sea una compilación reciente: recién aprovisionada o actualizada con la versión de plataforma 29.0.53497 o posterior. Si el entorno cumple esa condición, el chat nuevo sustituye automáticamente al heredado, sin configuración adicional.

Otra cosa a tener en cuenta: que el chat tenga la interfaz nueva no significa que todo funcione a la primera. En mi entorno de España, al pedirle "Muéstrame mis pedidos de venta abiertos más urgentes" (una de las sugerencias de la propia pantalla de bienvenida), Copilot me contestó que no podía consultar los datos porque la capacidad Chat estaba desactivada, y me remitió a la página **Capacidades de Copilot y agente** para activarla.

![Copilot en Business Central pidiendo activar la capacidad Chat, que en la página Capacidades de Copilot y agente ya aparece como Activo](04-copilot-chat-capacidad-desactivada.png){: w="1871" h="860" .shadow }
_Copilot pide activar la capacidad Chat... que en esa misma página ya figura como Activo_

El problema es que, una vez en esa página, Chat ya aparece como **Activo**: no hay ningún interruptor pendiente ni nada que un administrador pueda hacer para "reactivarla". Todo apunta a un fallo, no a un paso de configuración real que se te esté escapando.

La confirmación llegó al probar lo mismo en un sandbox de Estados Unidos: ahí la pregunta funciona sin problemas, con razonamiento en varios pasos, la tabla de resultados y hasta sugerencias de seguimiento.

![Copilot en Business Central respondiendo correctamente a "Top 5 clientes por ingresos" en un sandbox de Estados Unidos, con tabla de resultados y preguntas de seguimiento](05-copilot-chat-funciona-usa.png){: w="2425" h="1187" .shadow }
_En un sandbox de Estados Unidos la misma pregunta sí funciona, con razonamiento y tabla de resultados incluidos_

De paso, algo que me ha gustado mucho de este ejemplo: las tablas que genera el chat son interactivas. Cada nombre de cliente es un enlace, y al pinchar en Adatum Corporation se abre directamente su ficha, sin salir del flujo de la conversación.

![Resultado del chat de Copilot con el cliente Adatum Corporation como enlace en la tabla, y su ficha de cliente abierta al pinchar](06-copilot-chat-tabla-interactiva.png){: w="2399" h="1201" .shadow }
_Cada registro de la tabla es un enlace directo a su ficha correspondiente_

> Si el chat te pide activar una capacidad que en Capacidades de Copilot y agente ya figura como Activo, prueba primero en un sandbox de otra región antes de sospechar de tu configuración: a mí me funcionó sin problemas en uno de Estados Unidos, así que todo indica que es un fallo específico de la preview en español.
{: .prompt-warning }

> El proyecto sigue en desarrollo activo. Microsoft ha sido explícito en que la experiencia todavía se está afinando, así que es esperable ver ajustes de comportamiento y de interfaz entre esta vista previa y la disponibilidad general.
{: .prompt-info }

## Conclusión y recomendación

Si trabajas con clientes que ya usan Copilot en otras aplicaciones de Microsoft 365, te recomiendo activar esta vista previa en un entorno de pruebas cuanto antes. Es la mejor forma de llegar a la disponibilidad general con criterio propio sobre qué comunicar al cliente, en lugar de descubrir el cambio el mismo día que se publica.

> **Documentación de Microsoft**
> - [Detalles de la función en vista previa: Copilot and agents; enable unified Copilot Chat experience consistent with Microsoft Copilot](https://learn.microsoft.com/es-es/dynamics365/business-central/dev-itpro/whatsnew/preview-feature-details?wt.mc_id=DX-MVP-5004336#copilot-and-agents-enable-unified-copilot-chat-experience-consistent-with-microsoft-copilot)
{: .prompt-info }
