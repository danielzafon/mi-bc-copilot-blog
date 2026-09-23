---
title: "Verifactu en Business Central: configuración del modo integrado paso a paso"
date: 2026-09-21 19:00:00 +0200
categories: [Business Central, Funcional]
tags: [business-central, verifactu, facturacion-electronica, e-documentos, aeat, sii, localizacion-espana]
description: Cómo configurar el modo integrado de Verifactu en Business Central; servicio de documentos electrónicos, flujo de trabajo, perfil de envío y factura con QR, y qué controlar en los campos del XML.
image:
  path: 01-portada.png
  alt: Ficha del servicio de documentos electrónicos VERIFACTU en Business Central con el formato de documento y la integración de servicio remarcados
media_subpath: /assets/img/posts/verifactu-business-central-modo-integrado/
---

Con la nueva versión de septiembre (la 28.5 de la 2026 release wave 1) por fin tenemos el Verifactu integrado de forma nativa en Business Central. Lo he configurado para analizar las tareas y el coste de implantación en nuestros clientes.

Y el calendario aprieta. Según la nota informativa de la Agencia Tributaria, las entidades que tributan por el Impuesto sobre Sociedades tienen que haber adaptado sus sistemas de facturación antes del 1 de enero de 2027, y el resto de obligados antes del 1 de julio de 2027. Con esas fechas encima, lo sensato es dedicar ya un rato a ver cómo se comporta Business Central, y no dejarlo para diciembre.

Business Central ofrece dos caminos para Verifactu. El primero es integrarlo con un servicio externo mediante el conector de B2BRouter, que necesita credenciales propias y tiene coste: se paga por cada documento enviado. El segundo es el **modo integrado**: es Business Central quien firma con tu certificado y envía los registros a la AEAT a través del framework de documentos electrónicos, sin intermediarios.

Este artículo recorre el modo integrado tal y como lo he montado en un sandbox: qué hay que instalar y configurar, en qué orden, qué ves cuando registras una factura y dónde conviene fijarse antes de llevarlo a un cliente.

## Lo primero: instalar la extensión

El modo integrado de Verifactu no viene instalado por defecto. Antes de nada, busca la extensión **Document Registration in Spain**, de Microsoft, e instálala desde AppSource: en **Administración de extensiones**, con la acción **Galería de AppSource**. Sin instalarla, no tendrás disponible lo que configuramos en los pasos siguientes.

![Administración de extensiones con la extensión Document Registration in Spain de Microsoft, versión 28.5, instalada y remarcada](00-extension-document-registration-spain.png){: w="1815" h="395" .shadow }
*La extensión «Document Registration in Spain», de Microsoft, ya instalada. En mi sandbox es la versión 28.5.*

Si quieres ver qué hace por dentro, el código está en el repositorio de Microsoft: [BCApps > ES > EDocumentFormats > DocumentRegistration](https://github.com/microsoft/BCApps/tree/main/src/Apps/ES/EDocumentFormats/DocumentRegistration). Más adelante recurrimos a él para ver cómo se decide uno de los campos del XML.

## Antes de empezar

La configuración de Verifactu en sí (la página **Verifactu Setup**) se reduce a subir el certificado electrónico cualificado en el campo **Certificate Code** y activar **Enabled**. Lo importante está en lo que rodea a esa página, y la documentación de Microsoft lo recoge:

- En la ficha de la empresa, el CIF/NIF y el nombre deben coincidir con los del certificado, y no pueden faltar el código postal ni los datos de la pestaña **Pagos**.
- En los códigos postales debe estar definida la zona horaria.
- En **Mis ajustes**, la fecha de trabajo tiene que ser la de hoy y la región `Spanish (Spain, International Sort)`, porque la AEAT no acepta fechas superiores a la actual.

> Verifactu y SII **no pueden estar activos a la vez**, y Business Central lo impide con el propio interruptor. Si la empresa ya tiene el SII activado, asegúrate de que sabe qué modalidad le corresponde antes de tocar nada.
{: .prompt-warning }

## 1. El servicio de documentos electrónicos

En **Servicios de documentos electrónicos** se crea un servicio nuevo con estos valores:

- **Código** y **Descripción**: `VERIFACTU`.
- **Formato de documento**: `Verifactu`.
- **Integración de servicio**: `Servicio de Verifactu`.

![Ficha del servicio de documentos electrónicos VERIFACTU con formato de documento e integración de servicio remarcados](01-portada.png){: w="1730" h="831" .shadow }
*El servicio se define con el formato Verifactu y su propia integración de servicio.*

Desde la acción **Configure documentos para exportar** puedes ver qué tipos de documento admite el servicio. Merece la pena mirarlo: no son solo facturas de venta, también entran los abonos y los documentos de interés.

![Tipos de documento de origen admitidos por el servicio de documentos electrónicos: facturas y abonos de venta y documentos de interés](02-tipos-documento-admitidos.png){: w="1292" h="392" .shadow }
*Tipos de documento de origen admitidos por el servicio.*

## 2. El flujo de trabajo que dispara el envío

El servicio por sí solo no hace nada. Quien lo pone en marcha es un flujo de trabajo de clase `EDOC`. Si ya tenías uno activo para documentos electrónicos, la documentación de Microsoft indica desactivarlo antes de crear el nuevo. En mi sandbox no había ninguno, solo el de aprobación de pedidos de compra.

Se crea un flujo nuevo con código y descripción `VERIFACTU`, clase `EDOC`, y dos pasos. El primero reacciona a que se cree el documento electrónico:

![Flujo de trabajo VERIFACTU con el evento Documento electrónico creado remarcado](03-flujo-evento-creado.png){: w="1752" h="586" .shadow }
*Primer paso: cuando se crea el documento electrónico.*

La respuesta de ese paso es exportar el documento con la configuración del servicio `VERIFACTU`:

![Respuesta Exportar documento electrónico usando la configuración VERIFACTU](04-flujo-respuesta-exportar.png){: w="978" h="514" .shadow }
*La primera respuesta genera el fichero a partir del servicio.*

El segundo paso reacciona a que ya se haya exportado, y su respuesta es enviarlo mediante el servicio:

![Respuesta Enviar documento electrónico mediante el servicio VERIFACTU](05-flujo-respuesta-enviar.png){: w="961" h="496" .shadow }
*La segunda respuesta lo envía a la AEAT a través del servicio.*

> La segunda línea tiene que quedar **indentada** bajo la primera (acción **Aumentar sangría**), tal y como aparece en la documentación de Microsoft. Es fácil pasarlo por alto.
{: .prompt-tip }

![Flujo con la segunda línea indentada mediante Aumentar sangría](06-flujo-sangria.png){: w="1787" h="550" .shadow }
*Con la sangría aplicada, el paso de envío depende del de exportación.*

Solo queda activar el flujo:

![Flujo de trabajo VERIFACTU con el interruptor Activado marcado y los dos pasos configurados](07-flujo-activado.png){: w="1749" h="574" .shadow }
*Flujo activado, con los dos pasos: exportar y enviar.*

## 3. El perfil de envío de documentos

Falta decirle a Business Central qué clientes deben pasar por este flujo. Se hace con un perfil de envío de documentos:

- **Código** `VERIFACTU` y descripción `Verifactu`.
- **Documento electrónico**: `Flujo de trabajo de documentos electrónicos`.
- **Flujo de trabajo de documentos electrónicos**: `VERIFACTU`.

![Perfil de envío de documentos VERIFACTU con el documento electrónico apuntando al flujo de trabajo VERIFACTU](08-perfil-envio-verifactu.png){: w="1326" h="520" .shadow }
*El perfil apunta al flujo de trabajo que acabamos de crear.*

A partir de aquí tienes dos opciones. Si trabajas con varios perfiles de envío, asígnalo en la ficha de cada cliente al que corresponda:

![Ficha de cliente con el campo Perfil de envío de documentos remarcado](09-perfil-cliente.png){: w="1364" h="820" .shadow }
*Asignación del perfil en la ficha del cliente.*

Si no usas perfiles de envío, márcalo como **Predeterminado** y se aplicará a todos los clientes:

![Perfil VERIFACTU con el interruptor Predeterminado activado](10-perfil-predeterminado.png){: w="1302" h="540" .shadow }
*Como perfil predeterminado, afecta a todos los clientes sin perfil propio.*

> Fíjate en las opciones de envío del perfil: impresora, correo electrónico y disco vienen en `No`. Si tus clientes ya reciben la factura por correo mediante otro perfil, comprueba que al cambiarles el perfil (o al poner este como predeterminado) no pierden ese envío.
{: .prompt-info }

## 4. La factura con el código QR

Microsoft ha añadido un diseño de factura de venta con el QR incluido: **Factura de venta estándar - E-Document (Word)**, dentro de la extensión E-Document Core y sobre el informe 1306. Conviene comprobar que, en la selección de informes de ventas, para el uso **Factura**, sigue siendo ese el informe seleccionado.

![Diseños de informe del informe 1306 con el diseño E-Document remarcado](11-diseno-informe-qr.png){: w="1854" h="522" .shadow }
*El diseño de E-Document incluye la compatibilidad con el código QR.*

Si tu cliente usa un diseño propio, el QR no aparecerá solo: tendrás que añadirlo en su impresión. Usa el diseño estándar de E-Document como referencia: te ahorra tener que buscar por dónde empezar.

![Factura impresa con el bloque E-Doc QR Code remarcado](12-factura-con-qr.png){: w="611" h="844" .shadow }
*Factura con el bloque «E-Doc QR Code». En la captura he difuminado los datos del cliente y desordenado el QR para que no sea válido.*

## 5. Qué pasa cuando registras una factura

Conforme registras documentos, se van enviando a Verifactu mediante el servicio de documentos electrónicos. No hay que hacer nada más.

### El documento electrónico

Desde la factura registrada, en **Relacionado > Documento electrónico > Abierto**, accedes al documento generado.

![Histórico de facturas de venta con el menú Relacionado, Documento electrónico, Abierto remarcado](13-factura-registrada-documento-electronico.png){: w="1365" h="855" .shadow }
*Acceso al documento electrónico desde la factura registrada.*

Ahí están los datos que importan: el estado del documento (`Procesado`), el estado del servicio (`Compensado`), la fecha de compensación, el hash de Verifactu y el identificador de envío. Desde la misma ficha puedes **Enviar documento**, **Volver a crear documento** o **Cancelar documento electrónico**.

![Ficha del documento electrónico en estado Procesado y servicio Compensado, con las acciones de envío remarcadas](14-documento-electronico.png){: w="1755" h="834" .shadow }
*El documento electrónico con su información de compensación y las acciones disponibles.*

### Si el envío falla

En mi caso, la primera vez dio error. Al ser un sandbox, no tenía activadas las llamadas HTTP y el sistema las bloqueó. Bastó con activarlas y reenviar desde **Enviar documento**.

Para ver qué pasó, en **Relacionado > Registros** aparece cada transacción del documento. En mi prueba se ve todo el recorrido: creado, exportado, error de envío y, tras reenviar, compensado.

![Registros de documentos electrónicos con los estados Creado, Exportado, Error de envío y Compensado, y la acción Exportar archivo remarcada](15-registros-documento.png){: w="1756" h="459" .shadow }
*El log muestra en qué fase se quedó el documento y permite exportar el XML.*

### El XML

Desde esos mismos registros, con **Exportar archivo**, puedes descargar el XML generado. Su estructura es muy parecida a la del SII: cabecera con el obligado a la emisión, identificación de la factura, desglose por tipo impositivo y, en Verifactu, el encadenamiento de registros y los datos del sistema informático, que aquí identifica a Business Central.

![XML del registro de alta con TipoFactura, ClaveRegimen y CalificacionOperacion remarcados](16-xml-campos.png){: w="1103" h="928" .shadow }
*XML exportado; los tres campos remarcados son los que conviene controlar.*

### Los listados

Los documentos electrónicos también se pueden consultar directamente desde el listado **Documentos electrónicos**:

![Listado de documentos electrónicos con dos facturas de venta en estado Procesado y servicio VERIFACTU compensado](17-lista-documentos-electronicos.png){: w="1768" h="836" .shadow }
*Listado de documentos electrónicos.*

Y a medida que se envían aparecen en **Documentos de Verifactu**, con el hash, la fecha de registro, el identificador de envío y el estado de envío. Desde esta pantalla, al menos de momento, no puedes hacer nada más que abrir el documento electrónico que lo generó.

![Listado de documentos de Verifactu con hash, identificador de envío y estado de envío Correcto](18-documentos-verifactu.png){: w="1788" h="822" .shadow }
*Documentos de Verifactu: es una vista de consulta.*

## Los campos del XML que debes controlar

Revisando el XML, hay tres campos a los que conviene prestar atención:

| Campo del XML | Valor por defecto | De dónde sale |
|---|---|---|
| `TipoFactura` | `F1` (factura) | Campo **Tipo de factura** de la factura |
| `ClaveRegimen` | `01` (régimen general) | Campo **Cód. de esquema especial** de la factura |
| `CalificacionOperacion` | `S1` o `N1` | El código de la extensión, según el identificador de IVA |

Como en el SII, una factura sale por defecto como `F1` con clave de régimen `01`. Si tu operación es distinta, lo indicas en la propia factura, en el bloque **Información SII**:

![Factura de venta con el bloque Información SII, Cód. de esquema especial 01 General y Tipo de factura F1 remarcados](19-factura-tipo-esquema.png){: w="1419" h="842" .shadow }
*Los dos campos de la factura que alimentan `ClaveRegimen` y `TipoFactura`.*

O bien dejas los cruces preparados de antemano en la configuración de grupos de registro de IVA, con **Código de esquema especial de ventas** y de compra. Son los mismos campos que se usan en el SII.

![Configuración de grupos de registro IVA con las columnas de código de esquema especial remarcadas](20-config-iva-esquema.png){: w="1780" h="787" .shadow }
*Las columnas de esquema especial, vacías en esta base de datos de pruebas.*

El tercero, `CalificacionOperacion`, no se informa a mano. Lo decide la extensión con un procedimiento muy simple, que puedes consultar en el [código de la extensión en GitHub](https://github.com/microsoft/BCApps/tree/main/src/Apps/ES/EDocumentFormats/DocumentRegistration):

```al
local procedure GetVATIdentifier(VATIdentifier: Code[20]): Text
begin
    if VATIdentifier = '' then
        exit('N1')
    else
        exit('S1');
end;
```

Si el identificador de IVA llega vacío, la operación se califica como `N1`; si viene informado, como `S1`. Es un detalle fácil de pasar por alto, y conviene conocerlo antes de que un cliente te pregunte por qué una operación sale con una calificación que no esperaba.

## Qué implica implantarlo en un cliente

Si lo miras como proyecto, la implantación se reduce a un puñado de tareas. Unas son iguales en todos los clientes y otras dependen de cómo trabaje cada uno, y ahí es donde varía el esfuerzo:

| Tarea | Qué puede complicarla |
|---|---|
| Confirmar con el cliente si le corresponde SII o Verifactu | El cliente no lo tiene claro o no lo ha consultado con su asesor. |
| Instalar la extensión **Document Registration in Spain** | Nada; es igual en todos los clientes. |
| Subir el certificado electrónico cualificado y revisar los datos de la empresa | El certificado no está a mano o el CIF/NIF y el nombre no coinciden con los de la ficha. |
| Crear el servicio de documentos electrónicos y el flujo de trabajo | Nada; es la parte más repetible entre clientes. |
| Configurar el perfil de envío de documentos | El cliente ya trabaja con varios perfiles o envía las facturas por correo. |
| Añadir el QR a la factura | El cliente usa diseños de factura propios, que es lo habitual. |
| Preparar los cruces de esquema especial en la configuración de IVA | El cliente trabaja con muchos tipos de IVA o con operaciones que no son una factura general. |
| Probar el envío con casos reales | Hay abonos, otras claves de régimen o clientes sin identificador de IVA. |

En cuanto al coste, con el modo integrado no hay un servicio externo de por medio, así que no entra en juego el pago por documento enviado del conector de B2BRouter.

Y en cuanto al esfuerzo, la diferencia principal entre clientes está en dos cosas. La primera es si hay que incluir el QR en su documento: con el diseño estándar de E-Document no hay que hacer nada, pero es poco común que un cliente use la impresión estándar, así que lo habitual es tener que añadirlo a su diseño propio. La segunda es la variedad y complejidad de los tipos de IVA con los que trabaje, que marca cuántos cruces de esquema especial hay que preparar y probar. Las horas concretas te las dará tu propio sandbox: montarlo una vez con los casos reales de un cliente es la mejor forma de estimar el resto.

> **Por experiencia:** es recomendable dejar claro al cliente qué nos proporciona Microsoft y qué trabajo tenemos que hacer nosotros. Que Verifactu llegue integrado de forma nativa no significa que esté listo para usar: ponerlo en marcha conlleva trabajo por nuestra parte.
{: .prompt-tip }

## Mi recomendación práctica

1. **Confirma que el cliente tiene claro si debe acogerse al SII o a Verifactu.** No es una decisión del consultor: cada empresa tiene que saber cuál le corresponde antes de que toques nada. El SII sigue siendo el camino de quien ya está obligado a él, ya sea por volumen de facturación o por estar en el régimen de devolución mensual de IVA.
2. **Prueba con algo más que una factura estándar.** Una venta nacional con IVA general sale bien casi sin tocar nada. Los casos que conviene probar son los abonos, las operaciones con otra clave de régimen y las que llevan un identificador de IVA vacío.
3. **Rellena los cruces de esquema especial en la configuración de IVA antes de facturar en real.** Es el mismo trabajo que ya conoces del SII, y mejor dejarlo hecho que corregirlo después.
4. **Revisa los diseños de factura propios de cada cliente.** Sin el bloque del QR, la factura impresa o por correo no lleva la verificación.
5. **Elige con criterio entre perfil predeterminado y perfil por cliente.** Si lo pones como predeterminado, comprueba que nadie pierde el envío por correo que tenía.
6. **Cuando falle un envío, ve directo a Registros.** Ahí ves en qué fase se quedó, y desde la ficha puedes reenviar. En un sandbox, empieza comprobando que las llamadas HTTP están permitidas.
7. **Aprovecha el margen para probar.** La propia AEAT permite, hasta las fechas límite, dejar de remitir registros de prueba y facturar con otros sistemas. Ese tiempo es para descubrir estas cosas sin presión, no para esperar.

## Referencias

- Microsoft Learn: [Enable embedded VERI*FACTU mode in Spain](https://learn.microsoft.com/en-us/dynamics365/business-central/localfunctionality/spain/enable-real-time-invoice-reporting).
- Microsoft Learn: [VERI*FACTU with external service integration](https://learn.microsoft.com/en-us/dynamics365/business-central/localfunctionality/spain/verifactu-setup).
- Microsoft Learn: [Update 28.5 for Business Central 2026 release wave 1](https://learn.microsoft.com/en-us/dynamics365/business-central/dev-itpro/whatsnew/whatsnew-update-28-5).
- GitHub: [Código de la extensión Document Registration in Spain (microsoft/BCApps)](https://github.com/microsoft/BCApps/tree/main/src/Apps/ES/EDocumentFormats/DocumentRegistration).
- Agencia Tributaria: [Nota informativa sobre la ampliación del plazo de adaptación de los sistemas informáticos de facturación](https://sede.agenciatributaria.gob.es/Sede/iva/sistemas-informaticos-facturacion-verifactu/nota-informativa-ampliacion-plazo-adaptacion-facturacion.html).
