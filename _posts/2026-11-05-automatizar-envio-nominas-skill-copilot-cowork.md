---
title: "Caso de uso de Copilot Cowork: una skill para dividir y enviar nóminas"
date: 2026-11-05 10:00:00 +0200
categories: [Copilot, Cowork]
tags: [copilot, cowork, skills, casos-de-uso, automatizacion, microsoft-365, productividad]
description: Caso real de una skill de Copilot Cowork que divide el PDF mensual de nóminas, renombra cada archivo y envía los correos solo tras revisión humana; qué funcionó, qué falló y qué reglas añadimos.
image:
  path: 01-portada.png
  alt: Antes y después; a la izquierda, dividir y comprobar nóminas a mano en 45 minutos; a la derecha, Copilot Cowork divide el PDF y envía cada nómina por correo en menos de 5 minutos con una skill creada en 30 minutos y reutilizable
media_subpath: /assets/img/posts/automatizar-envio-nominas-skill-copilot-cowork/
---

Te cuento un pequeño caso de uso de Copilot Cowork: de los que no salen en las demos, pero ahorran trabajo real cada mes.

Hace unos días una compañera de administración me pidió ayuda con una tarea mensual. La gestoría le manda **un único PDF con todas las nóminas de la plantilla** y, hasta ahora, ella tenía que partirlo a mano en un PDF por empleado, renombrar cada archivo y enviar un correo a cada persona con su nómina adjunta.

Le llevaba unos 45 minutos y, al ser todo manual, dejaba margen para errores: una página que se queda en el archivo equivocado, un nombre mal escrito, un correo que sale sin adjunto o, peor, con la nómina de otra persona.

Lo resolvimos en media hora, en una llamada de Teams con mi compañera compartiendo su pantalla. En ese tiempo creamos una skill de **Copilot Cowork**, la probamos con datos reales (enviamos las nóminas del mes), corregimos lo que falló y la dejamos lista para enviar las nóminas de los próximos meses. Y el caso ilustra algo que vale para cualquier automatización: la IA aporta valor cuando el proceso está bien definido y las reglas de control están escritas, no cuando se le deja improvisar.

## Qué es una skill en Copilot Cowork

Copilot Cowork es el agente de Microsoft 365 Copilot que ejecuta tareas en tu nombre: redacta documentos, envía correos, organiza reuniones o trabaja con archivos de OneDrive, y **pide aprobación antes de cada acción**.

Las skills son conjuntos de instrucciones reutilizables que le dicen a Cowork *cómo* se hace una tarea concreta. Microsoft trae algunas de serie, pero lo interesante es que puedes escribir las tuyas: un fichero `SKILL.md` en una carpeta de OneDrive, sin Copilot Studio ni paquete de aplicación. Cowork carga cada una cuando la petición encaja con su descripción.

Lo mejor es que no hace falta escribir ese fichero a mano. Cowork incluye una skill para crear skills, así que todo se hace en lenguaje natural: le describimos el proceso, le fuimos indicando los ajustes y ella se encargó de redactar las instrucciones. Por eso pudimos tenerla lista en una sola llamada.

> Cowork requiere licencia de Microsoft 365 Copilot y que el administrador lo habilite. Comprueba la disponibilidad en tu tenant antes de plantearte un caso así.
{: .prompt-info }

## El proceso que había que cubrir

Antes de escribir una línea, describimos el proceso tal y como lo hacía ella:

1. Recibir el PDF del mes de la gestoría.
2. Separar cada nómina e identificar a quién pertenece.
3. Nombrar cada archivo con un patrón fijo y guardarlo en una carpeta del mes.
4. Buscar el correo de cada empleado.
5. Enviar el correo con el PDF adjunto.

El PDF de la gestoría tiene una estructura estable: cada página es una nómina, el nombre aparece como `APELLIDOS, NOMBRE` en la primera línea y el DNI/NIE está en la línea del trabajador, justo después de la fecha de antigüedad. El periodo figura como rango (`01 OCT 26 a 31 OCT 26`). Con eso la skill puede generar:

```text
Nominas_10.2026/
├── <DNI>_2026_10.pdf
├── <DNI>_2026_10.pdf
└── ...
```

## Cómo está diseñada la skill

El punto de partida es sencillo: la usuaria abre Cowork, adjunta en el chat el PDF que le ha enviado la gestoría y pide "divide el PDF de nóminas". No hay carpetas de entrada ni flujos que vigilen un buzón; el archivo llega con la propia petición.

A partir de ahí la skill sigue siete pasos: tomar el archivo adjunto, dividir, verificar, guardar en la carpeta del mes, mostrar el resumen, preguntar si se envía y enviar. Lo relevante no es la división del PDF, que es la parte fácil, sino **las reglas que rodean cada paso**.

### Dividir sin adivinar

- Un empleado puede tener **varias páginas** (por ejemplo, una segunda hoja por algún concepto adicional). Se agrupan por DNI en un único PDF, respetando el orden.
- El nombre a veces viene **truncado** por el ancho del campo. No se corrige a ojo: se reporta.
- Si no se puede leer el periodo, **no se escribe ningún archivo** y se pregunta el mes y año. Nunca se deduce.
- Las páginas sin DNI legible no se asignan a nadie; se listan para revisión manual.

### Verificar que todo cuadra

Antes de seguir, la skill comprueba una regla simple:

```text
páginas asignadas + páginas sin DNI = páginas totales del PDF
```

Si no cuadra, se para. Es una verificación trivial, pero es exactamente la que un humano deja de hacer el quinto mes.

### Resolver los correos con criterio

La búsqueda del correo va en cascada: primero por nombre completo en el directorio, después por nombre y primer apellido y, como último recurso, en la tabla de usuarios. Además, se aplican dos filtros:

- **Solo direcciones del dominio de la empresa.** Se descartan contactos implícitos y cualquier dirección personal, aunque aparezca en el historial de correo.
- **Coincidencias por apodo** (Antonio → Toni) se asignan, pero se marcan como incidencia para revisarlas.

### El humano decide el envío

Ninguna nómina sale sin que la usuaria vea antes un resumen con esta tabla:

| Nº | Archivo | Nombre completo | DNI/NIE | Email destinatario |
|----|---------|-----------------|---------|--------------------|

Debajo van las incidencias: empleados con varias páginas, nombres truncados, páginas sin DNI, correos ausentes o ambiguos y coincidencias por apodo. Solo después aparece la pregunta "Enviar las nóminas / No enviar". Si responde con otra cosa, se le contesta, se vuelve a mostrar el resumen y se pregunta de nuevo. Una respuesta vacía o cancelada equivale a no enviar.

En el resumen **no aparecen importes**: solo nombre, documento y archivo. La información salarial no tiene por qué pasar por el chat.

Hay un matiz práctico: Cowork pide aprobación antes de cada acción, también antes de cada correo. Con toda la plantilla, eso supone aprobar las nóminas una a una después de haber confirmado ya el resumen. En nuestro caso, después de revisar los primeros envíos, le indicamos que enviara siempre los correos cuando el destinatario fuera de la organización. Así la decisión se toma una sola vez, sobre el resumen completo, y encaja con el filtro de la skill, que solo admite direcciones del dominio de la empresa.

## Lo que falló en las primeras ejecuciones

Esta es la parte que más me interesa contar, porque la skill que funciona hoy no es la primera versión. Hubo tres ajustes, todos dentro de esa misma media hora y aprendidos ejecutándola con datos reales.

**1. Un correo salió sin el PDF.** El parámetro de adjuntos que usaba la primera versión no adjuntó el archivo en un envío, y la herramienta no dio error. La skill ahora fija el parámetro exacto que sí funciona y añade una doble comprobación: antes de enviar, que el archivo existe; después de enviar, que el resultado confirma un adjunto. Si falta, **se detienen todos los envíos pendientes** y se informa de qué salió y qué no.

**2. El resumen quedaba escondido.** En algunas ejecuciones el agente metía el resumen solo dentro del texto de la pregunta de confirmación, donde apenas se lee. Ahora la skill exige escribir la tabla como texto visible en la respuesta y, a continuación, hacer la pregunta. Aunque se pida "divide y envía" en un solo mensaje, el resumen se muestra siempre.

**3. Los apodos.** Hay personas cuyo nombre en el directorio no coincide con el de la nómina. Antes quedaban sin correo; ahora se asignan, pero se marcan como incidencia.

Además, el envío se hace por lotes de unos quince correos, porque cada ejecución de script tiene un límite de tiempo. Si un lote se corta, se reenvían solo los pendientes y nunca se duplica uno ya confirmado.

> Cuando una skill toca datos personales o salariales, cada fallo que veas en pruebas se convierte en una regla escrita. Es más fiable que confiar en que el modelo "ya lo hará mejor la próxima vez".
{: .prompt-tip }

## Qué aporta en la práctica

- **El trabajo mecánico desaparece**: dividir, renombrar, archivar y redactar correos.
- **El control se queda en la persona**: revisa una tabla, resuelve las incidencias y decide el envío.
- **El proceso queda documentado**: la skill describe cómo se hace la tarea, de modo que si otra persona tiene que cubrirla, el procedimiento ya existe.
- **No hace falta perfil técnico**: basta con adjuntar el PDF en el chat y escribir "divide el PDF de nóminas" o "envía la nómina a <persona>".

Lo llamativo es dónde se iban esos 45 minutos: no tanto en dividir y enviar como en **comprobar**. Primero, que las nóminas estuvieran bien divididas; después, que cada correo llevara la nómina correcta. Ahora esas comprobaciones las hace la skill (el cuadre de páginas, la asignación de cada archivo a su destinatario y la verificación del adjunto en cada envío) y a mi compañera le queda revisar un único resumen antes de confirmar. Resultado: carga el PDF y en **menos de 5 minutos** están enviadas todas las nóminas.

En cuanto al coste, la skill `/cost` indica que la ejecución completa, incluido el envío de los correos, consumió **150 Copilot Credits**. Si quieres entender cómo funciona este modelo de consumo, lo expliqué en [pago por uso en Copilot Cowork](https://danielzafon.github.io/mi-bc-copilot-blog/posts/pago-por-uso-copilot-cowork-creditos/).

## Mi recomendación práctica

- **Empieza describiendo el proceso real**, no la solución. Las reglas salen de cómo lo hace hoy quien lo ejecuta.
- **Define cuándo no se usa la skill.** La nuestra deja fuera unir o editar PDFs genéricos, calcular salarios o enviar correos que no sean nóminas.
- **Pon la verificación antes de la acción irreversible.** Dividir y archivar se puede repetir; un correo enviado a la persona equivocada no.
- **Nunca dejes que adivine.** Periodo ilegible, página sin DNI o correo ambiguo: se pregunta o se reporta.
- **Prueba con datos reales y vuelve a la skill tras cada fallo.** Los tres ajustes que te he contado salieron así.

Copilot Cowork no sustituye el criterio de quien conoce el proceso. Lo que hace bien es ejecutarlo de forma repetible cuando ese criterio está escrito, y en tareas como esta, con datos sensibles y un envío que no tiene vuelta atrás, eso es justo lo que necesitas.
