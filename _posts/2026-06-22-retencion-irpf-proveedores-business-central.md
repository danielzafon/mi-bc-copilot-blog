---
title: "IRPF de proveedores en Business Central: por fin un estándar"
date: 2026-06-22 10:00:00 +0200
categories: [Business Central, Finanzas]
tags: [business-central, irpf, retenciones, liquidacion, compras, cartera, preview]
description: Probamos la nueva funcionalidad estándar de Withholding Tax (en preview) para registrar el IRPF de proveedores en Business Central: configuración paso a paso, liquidación y, lo mejor, su compatibilidad con el módulo de cartera.
image:
  path: /assets/img/posts/retencion-irpf-proveedores/01-habilitar-funcionalidad.png
  alt: Habilitación de la funcionalidad de retenciones en la configuración de contabilidad
---

## Retenciones a proveedores: Una funcionalidad que llevábamos años pidiendo

Si algo hemos reclamado los que trabajamos con Business Central (y antes con NAV) en España, es una gestión nativa de la retención de impuestos a proveedores para poder registrar el IRPF correctamente. Durante mucho tiempo cada partner ha tirado de su propia solución para cubrir este hueco. Y curiosamente, casi todas parten de la misma solución original… pero eso ya es otra historia.

Microsoft ha publicado por fin una funcionalidad estándar de **Withholding Tax** que se puede usar precisamente para esto. Llega como novedad con la **versión 28** (release wave 1 de 2026); tienes la ficha del release plan aquí: [Calculate withholding taxes for vendors](https://learn.microsoft.com/en-us/dynamics365/release-plan/2026wave1/smb/dynamics365-business-central/calculate-withholding-taxes-vendors). Y la documentación oficial, aquí: [Withholding Tax en Business Central](https://learn.microsoft.com/es-es/dynamics365/business-central/finance-withholding-tax).

He querido probarla de principio a fin en un entorno de pruebas para ver hasta dónde llega, y la conclusión es mejor de lo que esperaba. Te cuento la configuración paso a paso y, al final, la sorpresa que me he llevado.

> Es una funcionalidad **en preview**. El propio sistema te avisa de que no la actives en un entorno productivo. Pruébala en un sandbox, no en producción.
{: .prompt-warning }

## Paso 1: Habilitar la funcionalidad

Lo primero es ir a la **configuración de contabilidad** y activar el campo **Enable Withholding Tax**. Al hacerlo, BC muestra un cuadro de confirmación advirtiendo de que la funcionalidad está en preview y recomendando probarla antes en un sandbox con una copia de los datos de producción. Confirmas y listo.

![Habilitar la funcionalidad de retenciones en la configuración de contabilidad](/assets/img/posts/retencion-irpf-proveedores/01-habilitar-funcionalidad.png){: w="1200" h="630" .shadow }
*Activación de la funcionalidad en la configuración de contabilidad, con el aviso de preview.*

## Paso 2: Crear el numerador en la configuración de compras

En la **configuración de compras** debemos crear y configurar un nuevo **numerador** específico para los movimientos de retención. Es el paso típico de cualquier funcionalidad de BC que genera su propia serie de documentos.

![Configuración del numerador de retenciones en compras](/assets/img/posts/retencion-irpf-proveedores/02-numerador-compras.png){: w="1200" h="630" .shadow }
*Nuevo numerador para las retenciones en la configuración de compras.*

## Paso 3: La lógica de grupos, igual que siempre

Aquí Business Central es fiel a su filosofía de siempre. Tenemos dos tipos de grupo que ya conocemos de configuraciones como el IVA:

| Tipo de grupo | Se asigna a | Responde a |
| --- | --- | --- |
| Grupo de **negocio** | El proveedor | El *quién* |
| Grupo de **registro** | Cuentas y productos | El *qué* y el *dónde contabiliza* |

En mi caso he creado un grupo de negocio llamado `PROF15` para asignarlo al proveedor. Para ese grupo he configurado el cruce con un grupo de registro `IRPF15`, donde indico la cuenta a la que se tiene que llevar la retención. Tienes el detalle de cómo montar esta configuración de registro en la documentación oficial: [Create a posting setup for withholding tax](https://learn.microsoft.com/es-es/dynamics365/business-central/finance-set-up-withholding-tax#create-a-posting-setup-for-withholding-tax).

![Grupo de negocio de retención PROF15](/assets/img/posts/retencion-irpf-proveedores/03-grupo-negocio-prof15.png){: w="1200" h="630" .shadow }
*Grupo de negocio `PROF15` para asignar al proveedor.*

![Cruce del grupo PROF15 con el grupo de registro IRPF15 y su cuenta contable](/assets/img/posts/retencion-irpf-proveedores/04-cruce-irpf15-cuenta.png){: w="1200" h="630" .shadow }
*Cruce de `PROF15` con el grupo de registro `IRPF15` y la cuenta donde se lleva la retención.*

> Es exactamente la misma mecánica que ya dominas con el IVA: cruzar grupo de negocio + grupo de registro para decidir qué cuenta se mueve. Si vienes de BC, no tienes curva de aprendizaje nueva.
{: .prompt-tip }

## Paso 4: Activar las retenciones en la ficha del proveedor

Para trabajar las retenciones en un proveedor concreto debemos **activar la funcionalidad en su ficha** e indicar el **grupo de negocio de retención**. En mi caso, `PROF15`.

![Activación de retenciones y grupo de negocio en la ficha del proveedor](/assets/img/posts/retencion-irpf-proveedores/05-ficha-proveedor.png){: w="1200" h="630" .shadow }
*Ficha del proveedor con la retención activada y el grupo de negocio `PROF15`.*

## Paso 5: Indicar el grupo de registro en cuentas y productos

Igual que hacemos con el IVA, el **grupo de registro** lo indicamos en la cuenta y/o en los productos. También podemos informarlo directamente en las líneas de la factura si nos interesa hacerlo a ese nivel.

![Grupos de retención en la ficha de la cuenta](/assets/img/posts/retencion-irpf-proveedores/06a-grupo-registro-cuenta.png){: w="1200" h="630" .shadow }
*En la ficha de la cuenta, los campos `Withholding Tax Bus. Post.` (`PROF15`) y `Withholding Tax Prod. Post.` (`IRPF15`) conviven con los grupos de IVA.*

![Grupo de registro de retención en la ficha del producto](/assets/img/posts/retencion-irpf-proveedores/06-grupo-registro-cuenta-producto.png){: w="1200" h="630" .shadow }
*Lo mismo a nivel de producto, en `Withholding Tax Prod. Posting Group`.*

## Paso 6: La factura ya arrastra el grupo

Al crear la factura, el sistema ya me ha **arrastrado automáticamente el grupo de registro** a la línea del documento. Es decir, la configuración previa hace su trabajo y no tengo que informar nada a mano en el día a día.

![Línea de factura con el grupo de registro de retención arrastrado](/assets/img/posts/retencion-irpf-proveedores/07-linea-factura.png){: w="1200" h="630" .shadow }
*La línea de la factura hereda el grupo de registro de retención de la configuración.*

## Paso 7: La nueva entidad "Withholding Tax Entry"

Al hacer la **vista previa** o **registrar** el documento, aparece una nueva entidad llamada **Withholding Tax Entry**. De momento, toda la funcionalidad aparece sin traducir, así que la verás en inglés.

Son movimientos muy similares a los del IVA, pero específicos de las retenciones. Quien esté acostumbrado a revisar los movimientos de IVA se va a encontrar como en casa.

![Movimientos de la entidad Withholding Tax Entry](/assets/img/posts/retencion-irpf-proveedores/08-withholding-tax-entry.png){: w="1200" h="630" .shadow }
*La nueva entidad `Withholding Tax Entry`, sin traducir todavía, con movimientos análogos a los de IVA.*

## El resultado: el asiento y la sorpresa con cartera

Si miramos el asiento contable, la retención se lleva correctamente a la cuenta que habíamos indicado en la configuración. Hasta aquí, lo esperable.

Y para mi sorpresa (no tenía mucha fe en que funcionase)… **¡es compatible con el módulo de cartera!** Descuenta el importe de la retención a la factura y genera los efectos correctamente.

![Vista previa del asiento con la retención y el efecto de cartera generado](/assets/img/posts/retencion-irpf-proveedores/09-asiento-retencion-cartera.png){: w="1200" h="630" .shadow }
*La retención va a su cuenta y el efecto de cartera se genera por el importe neto (1.000 + 210 de IVA − 150 de retención = 1.060).*

En la captura se ve perfectamente: la base de la compra, el IVA y la retención llevada a su cuenta. Y en la parte resaltada, la conversión de la factura con el **efecto de cartera por el importe neto**. Justo lo que necesitábamos para que la pieza encaje con la operativa real de pagos en España.

## La liquidación, calcada a la del IVA

Una vez registradas las facturas con retención, llega el momento de **liquidar**. Y aquí Microsoft vuelve a apoyarse en un patrón que ya tenemos interiorizado: el proceso de liquidación de retenciones es prácticamente igual al de la **liquidación de IVA**.

Al lanzarlo nos pide los datos de siempre —**periodo**, **nº de documento** y **fecha de registro**— pero añade dos campos propios:

- La **cuenta donde se van a liquidar las retenciones**.
- Una **cuenta de redondeo**.

Y ojo con esto último, porque es fácil pasarlo por alto:

> La **cuenta de redondeo es obligatoria**. Si la dejas en blanco, el proceso no continúa y da error. Tenla preparada en tu plan contable antes de lanzar la primera liquidación.
{: .prompt-warning }

![Pantalla Calc. and Post WHT Settlement con los campos de cuenta de liquidación y cuenta de redondeo](/assets/img/posts/retencion-irpf-proveedores/10-liquidacion.png){: w="1200" h="630" .shadow }
*El proceso `Calc. and Post WHT Settlement`: periodo, nº de documento y fecha de registro, más los campos resaltados `Settlement Account` y `Rounding G/L Account`.*

Tanto si haces la **vista previa** como si **registras** la liquidación, el sistema te muestra un **resumen muy similar al de la liquidación de IVA**: las bases, los importes de retención y las cuentas implicadas, con el mismo formato al que estamos acostumbrados. Es un detalle que se agradece, porque no obliga a aprender una pantalla nueva: quien ya liquida IVA cada periodo va a hacer esto exactamente igual.

En conjunto, refuerza la sensación de que la funcionalidad está bien encajada dentro de los flujos estándar de BC y no como un módulo aparte: misma lógica de grupos, mismos movimientos tipo IVA y ahora también la misma mecánica de liquidación.

## Conclusión

Que Microsoft incorpore una gestión estándar de retenciones es una muy buena noticia para el ecosistema español. Resuelve de forma nativa algo que llevábamos años cubriendo con desarrollos propios, y lo hace reutilizando la lógica de grupos que ya conocemos, sin reinventar nada.

Mi recomendación práctica:

- **Pruébala ya en un sandbox** para ir entendiendo el modelo y preparar la configuración de tus clientes.
- **No la lleves a producción** mientras siga en preview: el propio sistema te lo advierte.
- Vigila la **evolución y la traducción**, que de momento está toda en inglés.
- Valida tus **casos reales de cartera y pagos**, porque la compatibilidad que he visto es muy prometedora.

Cuando esto pase a disponibilidad general, va a simplificar bastante las implantaciones en España. Seguiré probando casos límite y te iré contando. Si ya la has probado y te has encontrado con algún comportamiento distinto, me encantará leerte.
