---
title: "Copilot Chat por fin dentro de Business Central"
date: 2026-09-09 10:00:00 +0200
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

![Después: nueva experiencia de chat de Microsoft Copilot en Business Central v29](01-copilot-chat-nuevo-v29.png){: w="1875" h="895" .shadow }
_La nueva experiencia de chat de Microsoft Copilot en v29_

El cambio visual es evidente, pero lo relevante es que ya no hace falta reaprender nada: los mismos patrones de Copilot que usas en Outlook o en Teams se aplican ahora a los datos y procesos de Business Central.

> Para verlo con mis propios ojos he tenido que montar un sandbox de Estados Unidos: en mi entorno de España, al menos de momento, el chat nuevo todavía no aparece. Si te pasa lo mismo, no des por hecho que tienes algo mal configurado; es cuestión de que el despliegue llegue a tu región.
{: .prompt-tip }

Para mí es un salto de calidad muy grande, y no es un cambio aislado: va en línea con la unificación que Microsoft lleva tiempo empujando para que el chat de Copilot sea el mismo, con la misma cara y el mismo comportamiento, en todas sus herramientas.

## Licencias: quién ve qué

La función está habilitada para todos los usuarios con licencia de Business Central, sin coste adicional. Pero hay una capa extra para quien tiene licencia de Microsoft Copilot: acceso a Work IQ y a capacidades ampliadas del chat. Además, el chat de Microsoft Copilot también queda incluido para los clientes empresariales de Microsoft 365 que cumplan los requisitos de licencia correspondientes.

Para un proyecto de implantación esto tiene una lectura práctica: el salto de experiencia lo nota todo el mundo desde el primer día, pero el valor añadido (Work IQ, capacidades ampliadas) solo se materializa si el cliente ya tiene o contrata licencia de Copilot. Es un buen argumento para revisar, en la fase de discovery, si conviene incluir esa licencia en el alcance del proyecto.

## Cómo probarlo ya

Para ver la nueva experiencia necesitas un entorno de acceso anticipado con la versión 29, y conviene que sea una compilación reciente: recién aprovisionada o actualizada con la versión de plataforma 29.0.53497 o posterior. Si el entorno cumple esa condición, el chat nuevo sustituye automáticamente al heredado, sin configuración adicional.

> El proyecto sigue en desarrollo activo. Microsoft ha sido explícito en que la experiencia todavía se está afinando, así que es esperable ver ajustes de comportamiento y de interfaz entre esta vista previa y la disponibilidad general.
{: .prompt-info }

Si trabajas con clientes que ya usan Copilot en otras aplicaciones de Microsoft 365, te recomiendo activar esta vista previa en un entorno de pruebas cuanto antes. Es la mejor forma de llegar a la disponibilidad general con criterio propio sobre qué comunicar al cliente, en lugar de descubrir el cambio el mismo día que se publica.
