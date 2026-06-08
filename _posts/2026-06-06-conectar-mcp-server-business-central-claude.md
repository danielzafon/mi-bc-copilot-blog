---
title: "Conectar el MCP server de Business Central a Claude: lo que no viene en la documentación"
date: 2026-06-06 18:00:00 +0200
categories: [Business Central, IA]
tags: [business-central, mcp, claude, ia, integracion, entra-id]
description: Cómo conectar el MCP server de Business Central a Claude Cowork con mcp-remote, incluyendo los errores reales de OAuth y rutas de Windows que no están documentados.
image:
  path: /assets/img/posts/bc-mcp-claude/bc-mcp-claude.png
  alt: Diagrama de conexión entre Business Central y Claude mediante MCP
---

Una de las novedades más relevantes de Business Central versión 28 (2026 Release Wave 1) es que el MCP server pasa a disponibilidad general y se abre explícitamente a clientes no-Microsoft. La funcionalidad, que hasta ahora solo estaba disponible para Copilot Studio, ahora está habilitada por defecto para todos los usuarios y es compatible con cualquier cliente que implemente el protocolo MCP: Claude, Cursor, herramientas propias o cualquier agente de IA que entienda el estándar.

Esta semana lo he puesto en marcha con Claude Cowork y el camino ha sido más accidentado de lo que sugiere la documentación oficial. Aquí dejo el proceso completo, con los problemas reales y cómo resolverlos.

## Qué es el MCP server de Business Central

MCP (Model Context Protocol) es un estándar abierto que permite a los modelos de lenguaje conectarse a fuentes de datos y herramientas externas de forma estructurada. Microsoft ha publicado un MCP server para Business Central disponible en `https://mcp.businesscentral.dynamics.com` que expone entidades de BC como herramientas invocables desde cualquier cliente compatible.

La promesa es clara: en lugar de construir una API o un conector personalizado, configuras el servidor MCP, apuntas tu agente y empiezas a consultar clientes, pedidos o facturas desde el chat.

En la práctica, la configuración no es trivial. Especialmente si usas un cliente no-Microsoft como Claude.

## El problema con mcp-remote y Microsoft OAuth

El cliente habitual para conectar MCPs remotos a aplicaciones de escritorio es `mcp-remote`, un proxy que actúa como intermediario entre el cliente local (Claude, Cursor, etc.) y el servidor MCP remoto.

El problema aparece en el primer intento de conexión:

```
Error: Incompatible auth server: does not support dynamic client registration
```

El MCP server de Business Central usa OAuth 2.0 con Microsoft Entra ID como servidor de autorización. `mcp-remote`, por defecto, intenta registrar el cliente dinámicamente (Dynamic Client Registration, DCR) antes de iniciar el flujo OAuth. Microsoft Entra ID no soporta DCR, y la conexión falla siempre, independientemente de la versión de `mcp-remote` que uses.

La [documentación oficial de Microsoft](https://learn.microsoft.com/en-us/dynamics365/business-central/dev-itpro/ai/use-mcp-server-non-microsoft) indica que hay que registrar una aplicación en Entra ID, pero no explica cómo hacer que `mcp-remote` use esa aplicación en lugar de intentar el registro dinámico.

## La solución: registrar la app y usar el flag correcto

### Registrar la aplicación en Entra ID

En el portal de Azure, bajo **Entra ID > Registros de aplicaciones**, crea una nueva aplicación:

1. Dale un nombre descriptivo, por ejemplo `BC MCP - Claude`.
2. Copia el **Application (client) ID** que se genera.
3. En **Autenticación**, añade una plataforma de tipo **Aplicaciones móviles y de escritorio** con la URI de redirección `http://localhost:3334/oauth/callback`.
4. En **Permisos de API**, añade permisos delegados de **Dynamics 365 Business Central**: `user_impersonation` y `Financials.ReadWrite.All`.
5. Concede el consentimiento de administrador.

### El flag que falta en la documentación

`mcp-remote` tiene un flag llamado `--static-oauth-client-info` que permite indicar un cliente ya registrado y saltarse el paso de DCR. No es `--client-id` (que existe pero no resuelve el problema) sino este otro, menos conocido.

Con él, la conexión fluye correctamente: `mcp-remote` detecta el cliente preregistrado, abre el navegador para que el usuario se autentique con su cuenta de Microsoft y guarda el token en `~/.mcp-auth/` para sesiones posteriores.

> Hay que fijar el puerto del callback OAuth con el segundo argumento numérico de `mcp-remote`. Si no se fija, el puerto se selecciona aleatoriamente en cada sesión y la redirect URI no coincide con la registrada en Azure.
{: .prompt-warning }

## El problema adicional en Windows: rutas con espacios

Una vez resuelta la autenticación, aparece un segundo problema específico de Windows. Cuando Claude Cowork arranca `mcp-remote` a través de `npx`, construye el comando internamente con `cmd.exe /C` y la ruta al ejecutable de Node. Si Node está instalado en `C:\Program Files\nodejs\`, la ruta contiene un espacio y el comando se rompe:

```
"C:\Program" no se reconoce como un comando interno o externo
```

La causa es que Cowork no entrecomilla correctamente la ruta al invocar `npx`. El resultado es que el MCP arranca pero se cierra inmediatamente con `Server disconnected`.

La solución es instalar `mcp-remote` de forma global para que esté disponible como comando directamente, sin depender de la ruta de `npx`:

```powershell
npm install -g mcp-remote
```

Con esto, en la configuración se usa `mcp-remote` como comando en lugar de `npx`, y el problema desaparece.

## Configuración final que funciona

Esta es la entrada que hay que añadir en `%APPDATA%\Claude\claude_desktop_config.json`:

```json
"businesscentral": {
  "command": "mcp-remote",
  "args": [
    "https://mcp.businesscentral.dynamics.com",
    "3334",
    "--header", "TenantId:<TU-TENANT-ID>",
    "--header", "EnvironmentName:<NOMBRE-ENTORNO>",
    "--header", "Company:<NOMBRE-EMPRESA>",
    "--header", "ConfigurationName:<NOMBRE-CONFIGURACION>",
    "--static-oauth-client-info", "{\"client_id\":\"<APP-CLIENT-ID>\"}"
  ]
}
```

Los parámetros en los headers controlan a qué entorno y empresa de BC se conecta el servidor, y `ConfigurationName` determina qué configuración MCP se usa dentro de BC, es decir, qué entidades se exponen como herramientas.

La primera vez que Cowork arranca con esta configuración, `mcp-remote` abre el navegador para autenticar. A partir de ahí, el token se reutiliza y la conexión es transparente.

> Antes de reiniciar Cowork, conviene autenticar manualmente desde la terminal con el mismo comando para verificar que el flujo OAuth completo funciona y que el token queda guardado en `~/.mcp-auth/`.
{: .prompt-tip }

## Qué se puede hacer una vez conectado

El MCP server de Business Central expone las entidades que hayas configurado en la página **Configuraciones del servidor MCP** dentro de BC. En mi caso, con una configuración de prueba, tenía disponible la herramienta `List_Customers`, que permite consultar la lista de clientes con filtros, ordenación y selección de campos mediante OData.

El resultado es directo: puedes preguntarle a Claude quién tiene el mayor saldo pendiente, filtrar clientes por país o exportar los datos a un Excel, todo sin salir del chat.

![Consulta de clientes con mayor saldo pendiente desde Claude conectado a Business Central](/assets/img/posts/bc-mcp-claude/bc-mcp-claude-clientes-saldo-pendiente.png){: w="1365" h="768" .shadow }

El límite está en lo que configures en BC. Si solo expones clientes, solo tendrás clientes. Para añadir pedidos, facturas o artículos, hay que ampliar la configuración del servidor en BC.

## Conclusión

El MCP server de Business Central es una herramienta real y funcional, pero la puesta en marcha con clientes no-Microsoft requiere resolver dos problemas que la documentación oficial no cubre bien: el flag correcto de `mcp-remote` para evitar el DCR de Entra ID, y el problema de rutas con espacios en Windows.

Una vez superados estos pasos, la integración funciona de forma estable. El valor práctico depende de cuántas entidades expongas en la configuración del servidor MCP dentro de BC. Empieza con pocas y bien definidas, valida que el acceso es el esperado y amplía de forma controlada.

Para equipos que ya trabajan con Claude o herramientas similares en su día a día, este conector abre una vía interesante para consultar datos de BC de forma conversacional sin necesidad de construir nada a medida.
