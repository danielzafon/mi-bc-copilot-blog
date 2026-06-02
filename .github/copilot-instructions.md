# Instrucciones para crear publicaciones en BC y Copilot

## Objetivo del blog

Este blog publica contenido practico y reutilizable sobre:

- Microsoft Dynamics 365 Business Central y legado funcional de NAV (Navision)
- desarrollo AL y arquitectura de soluciones
- gestion de proyectos de implantacion, evolucion e integracion
- adopcion real de IA, GitHub Copilot y automatizacion aplicada

El contenido debe transmitir experiencia profesional, criterio y utilidad real para equipos que trabajan con Business Central.

## Perfil del autor

Al redactar contenido, ten en cuenta siempre este contexto:

- Daniel Zafon tiene casi 20 anos de experiencia en programacion y consultoria sobre NAV y Business Central
- trabaja en DynamizaTIC desde 2015
- combina vision funcional, tecnica y de entrega
- lidera proyectos reales de implantacion, evolucion e integracion
- tiene enfoque de gestion de proyectos alineado con PMP y metodologias agiles
- esta especializado en adopcion practica de IA y Copilot en entornos reales

El tono debe reflejar a un profesional senior que ha trabajado con clientes, negocio y equipos tecnicos.

## Audiencia principal

Las publicaciones deben ser utiles para:

- consultores funcionales y tecnicos de Business Central
- desarrolladores AL
- responsables de proyecto y lideres de implantacion
- equipos que quieren aplicar IA de forma realista y con impacto

## Tono y estilo

- escribir en espanol claro, natural y profesional
- priorizar claridad, experiencia y aplicabilidad por encima de marketing o opinion vacia
- hablar desde la practica: problemas reales, decisiones, trade-offs y resultados
- evitar exageraciones, hype o lenguaje grandilocuente
- evitar tono academico o excesivamente teorico
- usar primera persona cuando aporte credibilidad o contexto real
- mantener frases directas y faciles de leer

## Enfoque editorial recomendado

Cada publicacion debe cumplir la mayoria de estos criterios:

- partir de un problema real, una mejora concreta o un aprendizaje util
- explicar por que importa en un proyecto o en operacion diaria
- aterrizarlo en Business Central, AL, gestion o IA aplicada
- dejar una conclusion practica o una recomendacion accionable
- aportar criterio profesional, no solo describir una funcionalidad

## Tipos de post prioritarios

- mejoras utiles de Business Central que resuelven necesidades reales
- patrones y buenas practicas en desarrollo AL
- experiencias de implantacion, integracion o evolucion de soluciones
- lecciones de gestion de proyectos aplicadas a entornos ERP
- usos reales de GitHub Copilot e IA en tareas de analisis, desarrollo, calidad o entrega
- automatizaciones que mejoran productividad en equipos tecnicos

## Estructura recomendada para un post

Siempre que tenga sentido, usar esta estructura:

1. Titulo claro y especifico
2. Introduccion breve con contexto real
3. Problema, necesidad o hallazgo
4. Explicacion de la solucion, funcionalidad o enfoque
5. Impacto practico en proyectos, soporte o productividad
6. Cierre con conclusion o recomendacion

Si el tema es muy breve, se puede usar una version reducida, pero siempre debe quedar claro el valor del contenido.

## Reglas para redactar posts

- no escribir contenido generico que podria pertenecer a cualquier blog tecnico
- no perder el foco en Business Central, gestion o IA aplicada al trabajo real
- incluir contexto de negocio cuando ayude a entender el problema
- incluir contexto tecnico cuando ayude a ejecutar la solucion
- no convertir el post en una nota de version sin interpretacion
- no abusar de listas largas si un parrafo explica mejor la idea
- evitar repetir conceptos obvios o definiciones basicas innecesarias

## Reglas concretas del repositorio

- crear los posts en `_posts/`
- nombrar los archivos como `YYYY-MM-DD-titulo-del-post.md`
- usar solo extensiones `md` o `markdown`
- usar fechas no futuras para que Jekyll publique el contenido
- usar `+0200` en las fechas mientras se mantenga este criterio editorial
- mantener categorias y tags coherentes con el contenido
- usar hasta dos categorias como recomienda Chirpy
- escribir los tags siempre en minusculas
- usar rutas absolutas para imagenes, por ejemplo `/assets/img/posts/mi-imagen.png`
- no usar `relative_url` para imagenes dentro del markdown de posts
- guardar imagenes de posts en `assets/img/posts/`
- validar el sitio con `./tools/test.sh` antes de publicar cambios

## Front matter recomendado

Usar esta plantilla como base:

```yaml
---
title: Titulo del articulo
date: 2026-06-02 10:00:00 +0200
categories: [Business Central, Copilot]
tags: [business-central, copilot, al, productividad]
description: Resumen breve y concreto del valor del articulo.
image:
  path: /assets/img/posts/nombre-imagen.png
  alt: Descripcion clara de la imagen
---
```

Notas importantes de Chirpy:

- `layout: post` no hace falta porque el repo ya lo define por defecto
- `description` es recomendable porque Chirpy la usa en listados, feed y cabecera del post
- si no se especifica `author`, Chirpy toma por defecto `social.name` y `social.links` desde `_config.yml`
- si algun dia se necesita autor explicito, es preferible centralizarlo en `_data/authors.yml`

## Opciones utiles de front matter en Chirpy

Usar solo cuando tenga sentido:

- `toc: false` para desactivar el indice lateral en posts cortos
- `comments: false` para desactivar comentarios en un post concreto
- `pin: true` para fijar un post en la parte superior del home
- `mermaid: true` si el articulo incluye diagramas Mermaid
- `math: true` si el articulo incluye formulas matematicas
- `media_subpath: /assets/img/posts/nombre-del-post/` si un articulo tiene varias imagenes y conviene simplificar rutas

## Reglas para imagenes en Chirpy

- la imagen de portada debe ir en `image.path`
- para portada, idealmente usar proporcion cercana a `1200 x 630`
- toda imagen debe tener texto alternativo claro
- cuando sea posible, indicar ancho y alto para evitar saltos visuales
- en imagenes SVG, al menos indicar ancho para que se rendericen correctamente
- si una imagen necesita leyenda, colocar una linea en cursiva justo debajo
- usar `{: .shadow }` en capturas si mejora la presentacion visual
- usar `{: .left }`, `{: .right }` o `{: .normal }` solo cuando realmente ayude a la lectura
- si se usa posicion con clases de alineacion, no anadir caption debajo

Ejemplo recomendado para imagen normal:

```md
![Descripcion de la imagen](/assets/img/posts/mi-captura.png){: w="1200" h="630" .shadow }
```

Ejemplo con leyenda:

```md
![Descripcion de la imagen](/assets/img/posts/mi-captura.png)
_Texto de la leyenda_
```

## Reglas para sintaxis enriquecida

- usar bloques de codigo con lenguaje explicito, por ejemplo `yaml`, `al`, `powershell`, `json`, `mermaid`
- no usar `{% highlight %}`; Chirpy espera bloques Markdown normales con triple comilla invertida
- si un snippet Liquid debe mostrarse como texto, envolverlo con `{% raw %}` y `{% endraw %}`
- para notas destacadas, usar prompts de Chirpy con blockquote y clase `prompt-info`, `prompt-tip`, `prompt-warning` o `prompt-danger`
- usar codigo inline solo para comandos, nombres de campos, variables, rutas o valores tecnicos concretos
- usar tablas cuando haya comparativas, diferencias funcionales o decisiones entre opciones
- usar footnotes solo cuando aporten contexto complementario y no para informacion principal
- resaltar rutas de archivo con la clase `filepath` cuando tenga sentido en ejemplos tecnicos

## Reglas de tipografia para este blog

- priorizar encabezados `##` y `###`; evitar profundidad innecesaria de secciones
- usar listas cuando mejoren claridad, pero no convertir todo en bullets si un parrafo explica mejor la idea
- usar blockquotes normales para citas o ideas destacadas, y prompts de Chirpy para avisos accionables
- evitar bloques de texto demasiado largos; dividir por secciones cortas y faciles de escanear
- en posts tecnicos, alternar texto explicativo con ejemplos, capturas o bloques de codigo
- si se comparan opciones o enfoques, preferir tablas claras antes que parrafos ambiguos

Ejemplo de filepath:

```md
`_posts/2026-06-02-mi-articulo.md`{: .filepath}
```

Ejemplo de tabla:

```md
| Opcion | Cuándo usarla |
| --- | --- |
| Copilot Chat | Analisis, redaccion y apoyo contextual |
| Copilot en editor | Cambios rapidos sobre codigo o texto |
```

Ejemplo de footnote:

```md
Esta funcionalidad cambio en versiones recientes.[^nota]

[^nota]: Conviene validar siempre el comportamiento en el entorno del cliente.
```

Ejemplo de prompt:

```md
> Este cambio conviene validarlo antes de publicar.
{: .prompt-info }
```

## Criterio editorial adicional para este blog

- no activar `pin: true` salvo que el contenido represente una pieza clave del blog
- usar `toc: false` en articulos muy cortos para evitar ruido visual
- si un post habla de una funcionalidad de Business Central, intentar incluir impacto real en operacion, soporte o productividad
- si un post habla de IA o Copilot, bajar siempre la idea a un caso de uso real y no a una demo generica
- si un post tiene pasos o instrucciones, combinar narrativa breve con listas ordenadas para que sea facil de seguir
- si un post es mas reflexivo o de experiencia, mantener una estructura limpia sin sobrecargarlo con elementos visuales innecesarios

## Checklist antes de publicar

- el titulo deja claro el tema y su utilidad
- el contenido refleja experiencia real y criterio profesional
- el texto suena a Daniel Zafon, no a texto promocional generico
- el archivo sigue la convencion `YYYY-MM-DD-titulo-del-post.md`
- las categorias no exceden dos elementos y los tags estan en minusculas
- la portada, si existe, usa `image.path` e `image.alt`
- la tipografia ayuda a leer: encabezados claros, listas utiles y codigo bien formateado
- las imagenes cargan con rutas absolutas correctas
- la fecha es publicable
- el build local pasa correctamente
