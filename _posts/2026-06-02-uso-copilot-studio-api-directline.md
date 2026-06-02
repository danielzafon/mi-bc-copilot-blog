---
title: Copilot Studio por API con Direct Line para enriquecer productos por EAN o SKU
date: 2026-06-02 18:00:00 +0200
categories: [Business Central, Copilot]
tags: [copilot-studio, directline, api, json, ean, sku, integracion]
description: Como exponer un agente de Copilot Studio por API con Direct Line para completar fichas de producto y devolver datos estructurados en JSON.
image:
  path: /assets/img/posts/copilot-studio-directline/crear-conversacion-directline.png
  alt: Ejemplo de llamada API para crear una conversacion en Direct Line
media_subpath: /assets/img/posts/copilot-studio-directline/
---

Esta semana, un companero me planteo una necesidad muy concreta en un proyecto interno: completar automaticamente informacion de producto (descripcion, peso, tamano, volumen y atributos) a partir de su EAN o SKU.

A partir de esa necesidad disene un enfoque practico: crear un agente en Copilot Studio con acceso a Internet, darle instrucciones claras de extraccion y normalizacion, y devolver la respuesta en JSON estructurado para que la aplicacion pudiera consumirla sin friccion.

## El problema real

En muchos proyectos, la informacion de producto llega incompleta o con formatos inconsistentes.
Cuando ese dato impacta en catalogo, ventas, logistica o analitica, cada campo faltante termina generando trabajo manual y errores acumulados.

El objetivo era resolver justo eso:

- entrada simple: EAN o SKU
- salida util para integracion: JSON con campos normalizados
- posibilidad de automatizar desde aplicacion, sin pasar por interfaz manual del agente

## Enfoque de solucion

El flujo que aplicamos con Direct Line fue este:

1. Crear una conversacion
2. Enviar el mensaje con el EAN o SKU
3. Esperar unos segundos y leer actividades hasta localizar la respuesta del bot
4. Parsear el JSON devuelto y mapearlo al modelo de datos interno

## Paso 1: crear conversacion

Se inicia una conversacion con `POST /v3/directline/conversations` usando el secreto de Direct Line en `Authorization: Bearer`.

```http
POST https://europe.directline.botframework.com/v3/directline/conversations
Authorization: Bearer {DIRECT_LINE_SECRET}
Content-Type: application/json
```

La respuesta devuelve, entre otros datos, `conversationId` y un `token` temporal.

![Crear conversacion en Direct Line](crear-conversacion-directline.png){: w="1200" .shadow }
_Creacion de conversacion y obtencion de conversationId/token._

## Paso 2: enviar mensaje al agente

Con el `conversationId` se envia la consulta al agente:

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
_Envio del mensaje a la conversacion activa._

## Paso 3: leer respuesta y recuperar el JSON

Tras enviar el mensaje, no siempre hay respuesta inmediata. Conviene esperar unos segundos y consultar actividades:

```http
GET https://europe.directline.botframework.com/v3/directline/conversations/{conversationId}/activities
Authorization: Bearer {TOKEN_O_SECRET}
```

En la coleccion de actividades hay que localizar el mensaje del rol `bot` y extraer su `text`.
En nuestro caso, ese `text` contiene el JSON estructurado que despues parseamos para integrarlo en la aplicacion.

![Leer respuesta del agente](leer-respuesta-directline.png){: w="1200" .shadow }
_Recuperacion de actividades y lectura del mensaje del bot con salida estructurada._

## Recomendaciones practicas

Para llevarlo a produccion con seguridad y estabilidad, me funciono bien aplicar estas reglas:

- no exponer secretos ni tokens en logs
- fijar timeout y politica de reintentos para lectura de actividades
- validar esquema JSON antes de persistir datos
- guardar trazas tecnicas (sin datos sensibles) para depuracion
- versionar el prompt/instrucciones del agente para controlar cambios de comportamiento

> En integraciones de este tipo, la clave no es solo que el agente responda, sino que responda siempre con una estructura predecible.
{: .prompt-info }

## Conclusiones

La combinacion Copilot Studio + Direct Line permite pasar de un agente util en pruebas a un componente integrable por API dentro de un flujo de negocio real.

En este caso, convertir una consulta por EAN o SKU en un JSON estructurado nos permitio reducir trabajo manual, acelerar la carga de fichas y mejorar la calidad del dato de producto desde el origen.

Si vas a implementar este patron en tu entorno, te recomiendo empezar con pocos campos obligatorios, asegurar la calidad del JSON de salida y ampliar el modelo de forma iterativa.
