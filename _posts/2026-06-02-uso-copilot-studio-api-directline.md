---
title: Copilot Studio por API con Direct Line para enriquecer productos por EAN o SKU
date: 2026-06-02 18:00:00 +0200
categories: [Business Central, Copilot]
tags: [copilot-studio, directline, api, json, ean, sku, integracion]
description: Cómo exponer un agente de Copilot Studio por API con Direct Line para completar fichas de producto y devolver datos estructurados en JSON.
image:
  path: crear-conversacion-directline.png
  alt: Ejemplo de llamada API para crear una conversación en Direct Line
media_subpath: /assets/img/posts/copilot-studio-directline/
---

Esta semana, un compañero me planteó una necesidad muy concreta en un proyecto interno: completar automáticamente información de producto (descripción, peso, tamaño, volumen y atributos) a partir de su EAN o SKU.

A partir de esa necesidad diseñé un enfoque práctico: crear un agente en Copilot Studio con acceso a Internet, darle instrucciones claras de extracción y normalización, y devolver la respuesta en JSON estructurado para que la aplicación pudiera consumirla sin fricción.

## El problema real

En muchos proyectos, la información de producto llega incompleta o con formatos inconsistentes.
Cuando ese dato impacta en catálogo, ventas, logística o analítica, cada campo faltante termina generando trabajo manual y errores acumulados.

El objetivo era resolver justo eso:

- entrada simple: EAN o SKU
- salida útil para integración: JSON con campos normalizados
- posibilidad de automatizar desde aplicación, sin pasar por la interfaz manual del agente

## Enfoque de solución

El flujo que aplicamos con Direct Line fue este:

1. Crear una conversación
2. Enviar el mensaje con el EAN o SKU
3. Esperar unos segundos y leer actividades hasta localizar la respuesta del bot
4. Parsear el JSON devuelto y mapearlo al modelo de datos interno

## Paso 1: crear conversación

Se inicia una conversación con `POST /v3/directline/conversations` usando el secreto de Direct Line en `Authorization: Bearer`.

```http
POST https://europe.directline.botframework.com/v3/directline/conversations
Authorization: Bearer {DIRECT_LINE_SECRET}
Content-Type: application/json
```

La respuesta devuelve, entre otros datos, `conversationId`, un `token` temporal y un `streamUrl` (WebSocket).

> En producción, evita usar el secreto en el cliente: genera un token efímero en el backend con `POST /v3/directline/tokens/generate` y reparte solo ese token. El secreto debe quedarse en el servidor.
{: .prompt-warning }

![Crear conversación en Direct Line](crear-conversacion-directline.png){: w="1200" .shadow }
*Creación de conversación y obtención de conversationId/token.*

## Paso 2: enviar mensaje al agente

Con el `conversationId` (y el `token` obtenido en el paso anterior) se envía la consulta al agente:

```http
POST https://europe.directline.botframework.com/v3/directline/conversations/{conversationId}/activities
Authorization: Bearer {TOKEN_O_SECRET}
Content-Type: application/json

{
  "type": "message",
  "from": { "id": "postman-user" },
  "text": "Producto Unique Pink Collagen 300g con EAN 5012345678900"
}
```

![Enviar mensaje al agente](enviar-mensaje-directline.png){: w="1200" .shadow }
*Envío del mensaje a la conversación activa.*

## Paso 3: leer respuesta y recuperar el JSON

Tras enviar el mensaje, no siempre hay respuesta inmediata. Conviene esperar unos segundos y consultar actividades:

```http
GET https://europe.directline.botframework.com/v3/directline/conversations/{conversationId}/activities
Authorization: Bearer {TOKEN_O_SECRET}
```

Para no releer en cada sondeo los mensajes ya procesados, usa el parámetro `watermark` con el último valor recibido:

```http
GET https://europe.directline.botframework.com/v3/directline/conversations/{conversationId}/activities?watermark={ultimoWatermark}
```

En la colección de actividades hay que localizar el mensaje del rol `bot` y extraer su `text`.
En nuestro caso, ese `text` contiene el JSON estructurado que después parseamos para integrarlo en la aplicación.

> Si quieres evitar el sondeo (polling), puedes conectarte al `streamUrl` por WebSocket que devuelve el Paso 1 y recibir las actividades en tiempo real.
{: .prompt-tip }

![Leer respuesta del agente](leer-respuesta-directline.png){: w="1200" .shadow }
*Recuperación de actividades y lectura del mensaje del bot con salida estructurada.*

## Recomendaciones prácticas

Para llevarlo a producción con seguridad y estabilidad, me funcionó bien aplicar estas reglas:

- no exponer secretos ni tokens en logs
- generar el token en el backend y no compartir nunca el secreto con el cliente
- fijar timeout y política de reintentos para la lectura de actividades
- validar el esquema JSON antes de persistir datos
- guardar trazas técnicas (sin datos sensibles) para depuración
- versionar el prompt/instrucciones del agente para controlar cambios de comportamiento

> En integraciones de este tipo, la clave no es solo que el agente responda, sino que responda siempre con una estructura predecible.
{: .prompt-info }

## Conclusiones

La combinación Copilot Studio + Direct Line permite pasar de un agente útil en pruebas a un componente integrable por API dentro de un flujo de negocio real.

En este caso, convertir una consulta por EAN o SKU en un JSON estructurado nos permitió reducir trabajo manual, acelerar la carga de fichas y mejorar la calidad del dato de producto desde el origen.

Si vas a implementar este patrón en tu entorno, te recomiendo empezar con pocos campos obligatorios, asegurar la calidad del JSON de salida y ampliar el modelo de forma iterativa.
