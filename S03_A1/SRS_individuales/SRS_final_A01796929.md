# Especificación de Requerimientos de Software (SRS): FairFix

**Versión:** 1.1 (incorpora la revisión técnica documentada en `revision_SRS.md`)
**Fecha:** 27/09/2026
**Estándar de referencia:** IEEE 830 (versión simplificada)
**Fuentes:** `proyecto_base.md`, `transcript_entrevista.md`, `requerimientos.md`, `revision_SRS.md`, `casos_de_uso.png`

---

## 1. Introducción

### 1.1 Propósito del documento

Este documento especifica los requerimientos de software de **FairFix**, una plataforma que conecta a personas que necesitan servicios de mantenimiento o reparación en su hogar con técnicos independientes verificados.

Está dirigido al equipo de desarrollo, al Product Owner y a quienes validarán el sistema. Sirve como base para el diseño, la implementación y las pruebas de aceptación.

### 1.2 Alcance del sistema

FairFix permitirá:

- Consultar perfiles de técnicos verificados (identidad, antecedentes y experiencia).
- Solicitar un servicio y recibir una cotización previa con un rango de precio de referencia.
- Dar seguimiento al servicio por estados.
- Autorizar de forma explícita, con evidencia fotográfica, cualquier costo adicional.
- Retener el pago hasta que el cliente confirme que el trabajo se realizó, con un proceso de disputa mediado por FairFix.
- Cerrar el servicio con un recibo digital y una calificación que alimenta la reputación del técnico.
- Usar un modo simplificado, pensado para adultos mayores y usuarios con poca experiencia digital.

**Fuera del alcance de esta versión:**

- El ajuste automático del rango de precio con datos de la plataforma. Queda contemplado como evolución futura (RNF-15).
- La verificación automática de documentos de técnicos. En esta versión la realiza el personal de FairFix (RD-09).
- El procesamiento directo de pagos con tarjeta, que se delega a una pasarela externa (RD-03).

### 1.3 Definiciones y acrónimos

| Término | Definición |
|---|---|
| **SRS** | *Software Requirements Specification*, especificación de requerimientos de software. |
| **RF** | Requerimiento funcional. |
| **RNF** | Requerimiento no funcional. |
| **RD** | Requerimiento de dominio. |
| **Cliente** | Persona que solicita un servicio de mantenimiento o reparación para su hogar. |
| **Técnico** | Trabajador independiente de un oficio (plomería, electricidad, gas, cerrajería, carpintería) registrado en FairFix. |
| **Técnico verificado** | Técnico cuya identidad, antecedentes y experiencia fueron revisados y aprobados por el personal de FairFix. |
| **Personal FairFix** | Equipo interno que verifica técnicos, administra el catálogo de precios, media dudas y disputas y atiende soporte. |
| **Rango de precio de referencia** | Precio mínimo y máximo habitual de un trabajo del catálogo en la ciudad. |
| **Cotización** | Precio que el técnico tiene registrado para un trabajo del catálogo (RF-27) y que el cliente ve antes de confirmar el servicio. |
| **Costo adicional** | Cargo extra que el técnico propone durante el servicio y que requiere la aprobación del cliente. |
| **Pago retenido** | Pago del cliente que se guarda y no se entrega al técnico hasta que el cliente confirma el trabajo o se cumple el plazo de liberación. |
| **Disputa** | Proceso que inicia el cliente cuando no está conforme con el trabajo. Congela el pago y lo resuelve el personal de FairFix. |
| **Tabulador** | Lista de precios por tipo de trabajo; el cobro es por trabajo, no por hora. |
| **Pasarela de pagos** | Servicio externo que procesa los pagos (por ejemplo, Mercado Pago o Stripe). |
| **INE** | Credencial para votar del Instituto Nacional Electoral, usada como identificación oficial en México. |
| **LFPDPPP** | Ley Federal de Protección de Datos Personales en Posesión de los Particulares (México). |
| **OXXO** | Cadena de tiendas de conveniencia donde se pueden realizar pagos en efectivo. |
| **Día (en plazos)** | Periodo de 24 horas naturales contado desde el evento que inicia el plazo. |
| **Paso (de contratación)** | Cada pantalla en la que el cliente debe tomar una decisión o capturar un dato, desde elegir la categoría hasta confirmar el pago. No incluye el inicio de sesión. |
| **Horario de atención** | De 8:00 a 20:00, hora local, todos los días (RNF-14). |

#### Estados del servicio

| Estado | Significado | Pasa a |
|---|---|---|
| **Solicitado** | El cliente eligió técnico y está pendiente el pago o la aceptación del técnico. | Confirmado, Cancelado |
| **Confirmado** | El pago fue recibido y el técnico aceptó la solicitud. | En camino, Cancelado |
| **En camino** | El técnico se dirige al domicilio. | En curso |
| **En curso** | El técnico está realizando el trabajo. | Costo adicional pendiente, Terminado |
| **Costo adicional pendiente** | Hay un costo adicional esperando respuesta del cliente. | En curso, Terminado |
| **Terminado** | El técnico marcó el trabajo como terminado; el pago sigue retenido. | Cerrado, En disputa |
| **En disputa** | El cliente reportó inconformidad; el pago está congelado. | En curso (nueva visita), Cerrado |
| **Cerrado** | El pago se liberó o la disputa se resolvió con reembolso. Estado final. | — |
| **Cancelado** | El servicio se canceló antes de iniciar el trabajo. Estado final. | — |

---

## 2. Descripción general

### 2.1 Perspectiva del producto

Hoy, contratar a un técnico del hogar depende de recomendaciones de conocidos o de grupos informales en redes sociales. El cliente no sabe cuánto debería costar el trabajo ni qué tan confiable es la persona que entra a su casa, y suele enfrentar cobros extra a mitad del servicio, sin justificación ni posibilidad real de negarse. Los técnicos honestos, por su parte, no tienen un canal formal para demostrar su experiencia.

FairFix es un producto nuevo e independiente que formaliza esta transacción. Se relaciona con los siguientes sistemas externos:

| Sistema externo | Uso |
|---|---|
| **Pasarela de pagos** (Mercado Pago, Stripe o similar) | Cobro al cliente, retención, liberación al técnico y reembolsos. |
| **WhatsApp** | Envío de códigos de inicio de sesión y de avisos importantes del servicio. |
| **Canal telefónico** | Soporte con personas, creación de solicitudes por teléfono y respaldo cuando la aplicación falla. |

### 2.2 Funciones del producto

Las funciones principales se agrupan en tres áreas, representadas en el diagrama de casos de uso (`casos_de_uso.png`):

**Contratación y seguimiento (Cliente)**
- Iniciar sesión con número de teléfono y código por WhatsApp, y registrar su nombre y dirección.
- Consultar el perfil público de los técnicos.
- Solicitar un servicio y consultar la cotización con su rango de referencia.
- Pagar el servicio (el pago queda retenido).
- Consultar el estado del servicio.
- Responder costos adicionales y resolver dudas con mediación de FairFix.
- Cancelar un servicio antes de que el técnico vaya en camino.
- Confirmar el trabajo realizado o reportar una inconformidad.
- Calificar y reseñar al técnico.
- Solicitar ayuda a una persona desde cualquier pantalla.

**Servicio del técnico (Técnico)**
- Registrarse y acreditar identidad y experiencia.
- Registrar su disponibilidad y los trabajos del catálogo que ofrece, con su cotización, y justificar precios fuera de rango.
- Aceptar o rechazar solicitudes y actualizar el estado del servicio.
- Registrar costos adicionales con evidencia fotográfica.
- Consultar sus pagos retenidos, liberados y congelados.

**Administración (Personal FairFix)**
- Verificar, suspender y reactivar técnicos.
- Administrar el catálogo de precios de referencia y las promociones.
- Mediar dudas y disputas.
- Crear solicitudes de servicio por teléfono.
- Consultar indicadores de éxito.

![Diagrama de casos de uso](casos_de_uso.png)

> **Nota:** el diagrama corresponde a la versión 1.0 y no incluye RF-26 a RF-32, agregados en la revisión técnica.

### 2.3 Características del usuario

| Usuario | Descripción | Implicaciones para el sistema |
|---|---|---|
| **Cliente general** | Persona que necesita una reparación en su hogar, con frecuencia de forma urgente. | Flujo de contratación rápido y precios claros desde el inicio. |
| **Cliente adulto mayor / con poca experiencia digital** | Usa el celular principalmente para WhatsApp, olvida contraseñas, se equivoca con botones pequeños y se pone nervioso con límites de tiempo. Es el usuario más vulnerable a precios abusivos. | Modo simplificado: texto y botones grandes, pocos pasos, lenguaje cotidiano, acceso sin contraseña, avisos por WhatsApp y ayuda humana siempre disponible. |
| **Técnico independiente** | Trabajador de un oficio, a menudo sin certificación formal porque aprendió de manera empírica. Busca clientes de forma formal y cobrar a tiempo. | Registro que acepte experiencia comprobable sin certificación, flujo para cotizar y registrar costos adicionales, y pago puntual. |
| **Personal FairFix** | Equipo interno reducido que verifica, media y da soporte de forma manual. | Herramientas de revisión de documentos, mediación de disputas, administración del catálogo e indicadores. |

### 2.4 Restricciones

- **Legales:** cumplimiento de la LFPDPPP y publicación de un aviso de privacidad (RD-01). La retención del pago está sujeta a revisión legal antes de operar (RD-02).
- **Pagos:** todos los pagos pasan por una pasarela externa; FairFix no almacena datos de tarjetas ni hay manejo de dinero directo entre cliente y técnico (RD-03, RNF-07).
- **Operación:** el soporte con personas funciona de 8:00 a 20:00, no 24 horas (RNF-14). La verificación de técnicos y la mediación de disputas son manuales (RD-09).
- **Privacidad:** acceso restringido a la dirección del cliente y a los documentos de los técnicos; las fotos de evidencia se eliminan tras un periodo de retención (RNF-08, RNF-09, RNF-10).
- **Negocio:** cobro por trabajo según tabulador, no por hora (RD-04). El rango de precio inicial se construye de forma manual con investigación de mercado local (RD-05).
- **Seguridad del trabajo:** si el cliente rechaza un costo adicional, el técnico debe dejar la instalación funcionando o, por lo menos, segura (RD-07).

### 2.5 Suposiciones y pendientes de validar

La revisión técnica (`revision_SRS.md`) propuso valores concretos para que los requerimientos sean verificables. Los siguientes supuestos deben confirmarse con el Product Owner antes del desarrollo:

| # | Supuesto | Requerimientos afectados |
|---|---|---|
| S-01 | Las fotos de evidencia se conservan 6 meses después del cierre del servicio (o de la resolución de su disputa). | RNF-10 |
| S-02 | La disponibilidad objetivo del flujo de servicio en curso es de 99.5 % mensual en horario de atención. | RNF-12 |
| S-03 | Los costos adicionales aceptados se cobran con el mismo método de pago; con OXXO o transferencia se genera una referencia de pago y esa parte se retiene hasta recibirla. | RF-09 |
| S-04 | El cliente puede cancelar sin costo mientras el servicio no esté "En camino"; después, la política de cancelación queda por definir. | RF-29 |
| S-05 | El descuento de las promociones lo absorbe FairFix; el técnico recibe el monto completo de su cotización. | RF-24 |
| S-06 | Si WhatsApp no está disponible, el código de inicio de sesión se envía por SMS. | RF-16 |
| S-07 | La suspensión de técnicos la decide manualmente el personal de FairFix; los umbrales de quejas o calificación que la disparan quedan por definir. | RF-30 |
| S-08 | FairFix cobra una comisión por servicio; el porcentaje y el momento de descontarla no se han definido. | RF-12, RF-32 |
| S-09 | El código de inicio de sesión vence a los 10 minutos y se bloquea el número 15 minutos tras 5 códigos incorrectos. | RF-16 |
| S-10 | Las medidas de accesibilidad y rendimiento (18 pt, 48 × 48 dp, 3 segundos, 80 % de éxito) son valores de referencia de la industria propuestos por la revisión. | RNF-01, RNF-16, RNF-21 |
| S-11 | La edad del cliente es un dato opcional que solo se usa para medir el indicador de contrataciones sin soporte en adultos mayores. | RF-25, RF-26 |

---

## 3. Requerimientos específicos

### 3.1 Requerimientos funcionales

Cada requerimiento funcional incluye sus criterios de aceptación en formato *Dado que / cuando / entonces*, con un identificador único `RF-XX-AC-N`.

#### RF-01: Registro de técnico con documentos
El sistema debe permitir que un técnico se registre cargando INE, una selfie, comprobante de domicilio y carta de no antecedentes penales.

- **RF-01-AC-1:** **Dado que** un técnico está llenando su registro y no ha cargado alguno de los cuatro documentos, **cuando** intenta enviar la solicitud, **entonces** el sistema no la envía y le indica qué documento falta.
- **RF-01-AC-2:** **Dado que** un técnico cargó los cuatro documentos, **cuando** envía la solicitud, **entonces** la solicitud queda en estado "En revisión" y el técnico no aparece en las búsquedas de los clientes.

#### RF-02: Acreditación de experiencia
El sistema debe permitir que el técnico acredite su experiencia con una certificación o, en su lugar, con fotos de trabajos anteriores y al menos dos referencias de clientes.

- **RF-02-AC-1:** **Dado que** un técnico cargó una certificación de su oficio, **cuando** envía su registro, **entonces** el sistema acepta la experiencia como acreditada sin pedir fotos ni referencias.
- **RF-02-AC-2:** **Dado que** un técnico no tiene certificación, **cuando** envía su registro con al menos una foto de un trabajo anterior y dos referencias con nombre y teléfono, **entonces** el sistema acepta la experiencia como acreditada.
- **RF-02-AC-3:** **Dado que** un técnico no tiene certificación, **cuando** intenta enviar su registro con menos de dos referencias o sin fotos de trabajos, **entonces** el sistema no lo envía y le indica qué le falta.

#### RF-03: Verificación de técnicos
El sistema debe permitir que el personal de FairFix revise y apruebe o rechace las solicitudes de registro de técnicos.

- **RF-03-AC-1:** **Dado que** existe una solicitud de técnico "En revisión", **cuando** el personal de FairFix la abre, **entonces** puede ver todos los documentos, fotos y referencias cargados.
- **RF-03-AC-2:** **Dado que** el personal de FairFix revisó una solicitud, **cuando** la aprueba, **entonces** el técnico pasa a estado "Verificado" y aparece en las búsquedas de los clientes.
- **RF-03-AC-3:** **Dado que** el personal de FairFix revisó una solicitud, **cuando** la rechaza, **entonces** el sistema exige capturar un motivo y se lo notifica al técnico.

#### RF-04: Solicitud de servicio
El sistema debe permitir al cliente solicitar un servicio eligiendo la categoría del problema mediante íconos, el trabajo del catálogo, la fecha y hora de la visita y el técnico (con su foto, calificación y cotización), y confirmar la solicitud.

- **RF-04-AC-1:** **Dado que** el cliente inició sesión, **cuando** entra a solicitar un servicio, **entonces** ve las categorías de servicio, cada una con su ícono.
- **RF-04-AC-2:** **Dado que** el cliente eligió categoría, trabajo, fecha y hora, **cuando** el sistema le muestra los técnicos, **entonces** solo aparecen técnicos verificados, no suspendidos, que ofrecen ese trabajo y tienen disponible ese horario, cada uno con su foto, calificación promedio y cotización.
- **RF-04-AC-3:** **Dado que** el cliente eligió categoría, trabajo, fecha, hora y técnico, **cuando** confirma la solicitud, **entonces** el sistema registra el servicio en estado "Solicitado" con esos datos y la dirección del cliente, y le muestra una confirmación.
- **RF-04-AC-4:** **Dado que** no hay técnicos disponibles para el trabajo y horario elegidos, **cuando** el sistema realiza la búsqueda, **entonces** muestra un mensaje que lo indica y ofrece elegir otro horario o solicitar ayuda.

#### RF-05: Catálogo de precios de referencia
El sistema debe mantener un catálogo de trabajos comunes con un rango de precio de referencia para cada uno, administrable por el personal de FairFix.

- **RF-05-AC-1:** **Dado que** el personal de FairFix está en el catálogo, **cuando** crea o edita un trabajo con precio mínimo y máximo, **entonces** el sistema lo guarda y el nuevo rango se usa en las cotizaciones siguientes.
- **RF-05-AC-2:** **Dado que** el personal de FairFix está capturando un trabajo, **cuando** el precio mínimo es mayor que el máximo, **entonces** el sistema no lo guarda y muestra un error.
- **RF-05-AC-3:** **Dado que** un trabajo está activo en el catálogo, **cuando** el personal lo desactiva, **entonces** deja de aparecer como opción para nuevas solicitudes.

#### RF-06: Cotización con rango de referencia
El sistema debe mostrar al cliente la cotización del técnico junto con el rango de precio de referencia del trabajo.

- **RF-06-AC-1:** **Dado que** un técnico tiene registrada su cotización para un trabajo del catálogo, **cuando** el cliente consulta a ese técnico durante la solicitud, **entonces** ve el precio del técnico y el texto "normalmente cuesta entre $X y $Y" con el rango de ese trabajo.
- **RF-06-AC-2:** **Dado que** el personal de FairFix modificó el rango de un trabajo, **cuando** un cliente consulta una cotización de ese trabajo, **entonces** ve el rango vigente, no el anterior.

#### RF-07: Justificación de precio fuera de rango
El sistema debe exigir al técnico una justificación cuando su cotización quede fuera del rango de referencia, y mostrar al cliente un aviso con esa justificación.

- **RF-07-AC-1:** **Dado que** un técnico captura un precio fuera del rango de referencia, **cuando** intenta guardar la cotización sin justificación, **entonces** el sistema no la guarda y le pide explicar el motivo.
- **RF-07-AC-2:** **Dado que** un técnico registró una cotización fuera de rango con justificación, **cuando** el cliente la consulta, **entonces** ve un aviso de que el precio está fuera de lo normal junto con la justificación.
- **RF-07-AC-3:** **Dado que** un técnico captura un precio dentro del rango, **cuando** guarda la cotización, **entonces** el sistema no le pide justificación.

#### RF-08: Registro de costo adicional
El sistema debe permitir al técnico registrar un costo adicional durante el servicio con una o dos fotos, una descripción y el monto.

- **RF-08-AC-1:** **Dado que** el servicio está "En curso", **cuando** el técnico registra un costo adicional con una o dos fotos, descripción y monto, **entonces** el sistema lo guarda y el servicio pasa a "Costo adicional pendiente".
- **RF-08-AC-2:** **Dado que** el técnico está registrando un costo adicional, **cuando** intenta enviarlo sin foto, sin descripción o sin monto, o con más de dos fotos, **entonces** el sistema no lo envía y le indica el error.
- **RF-08-AC-3:** **Dado que** el servicio no está "En curso", **cuando** el técnico intenta registrar un costo adicional, **entonces** el sistema no se lo permite.

#### RF-09: Notificación y respuesta a costo adicional
El sistema debe notificar al cliente cada costo adicional con las fotos, el monto nuevo y el total actualizado, y ofrecerle las opciones "Aceptar", "Rechazar" y "Tengo dudas".

- **RF-09-AC-1:** **Dado que** el técnico registró un costo adicional, **cuando** el cliente abre la notificación, **entonces** ve las fotos, la descripción, el monto adicional, el total actualizado, los botones "Aceptar", "Rechazar" y "Tengo dudas", y el aviso "si no respondes, este costo no se cobra".
- **RF-09-AC-2:** **Dado que** el cliente está viendo un costo adicional, **cuando** pulsa "Aceptar", **entonces** el total del servicio se actualiza con el monto adicional, el monto se cobra y queda retenido según el supuesto S-03, el servicio regresa a "En curso" y se notifica al técnico.
- **RF-09-AC-3:** **Dado que** el cliente está viendo un costo adicional, **cuando** pulsa "Rechazar", **entonces** el total del servicio se mantiene sin ese monto, el servicio regresa a "En curso" y se notifica al técnico que debe terminar solo lo acordado y dejar la instalación funcionando o segura (RD-07).

#### RF-10: Canal de dudas con mediación
La opción "Tengo dudas" debe abrir un canal de comunicación entre el cliente y el técnico, con una persona del equipo de FairFix como mediadora.

- **RF-10-AC-1:** **Dado que** el cliente está viendo un costo adicional, **cuando** pulsa "Tengo dudas", **entonces** se abre una conversación entre el cliente, el técnico y una persona de FairFix.
- **RF-10-AC-2:** **Dado que** hay una conversación de dudas abierta, **cuando** el cliente todavía no acepta ni rechaza, **entonces** el costo adicional permanece pendiente y el cliente puede aceptarlo o rechazarlo desde la conversación.
- **RF-10-AC-3:** **Dado que** el cliente pulsa "Tengo dudas" fuera del horario de atención, **cuando** se abre la conversación, **entonces** el sistema le informa que la mediación de FairFix está disponible de 8:00 a 20:00, mantiene la conversación con el técnico y el costo adicional permanece pendiente.

#### RF-11: Costo adicional sin respuesta
El sistema debe tratar como rechazado todo costo adicional que siga sin respuesta del cliente cuando el técnico marque el servicio como "Terminado", y no debe cobrar ningún trabajo extra que no haya sido aprobado.

- **RF-11-AC-1:** **Dado que** un costo adicional sigue sin respuesta, **cuando** el técnico marca el servicio como "Terminado", **entonces** el sistema lo registra como rechazado, lo excluye del total y se lo notifica al cliente y al técnico.
- **RF-11-AC-2:** **Dado que** un servicio tiene costos adicionales rechazados o sin respuesta, **cuando** se calcula el monto a cobrar, **entonces** el total solo incluye el monto original y los costos adicionales aprobados.

#### RF-12: Retención del pago
El sistema debe retener el pago del servicio hasta que el cliente confirme que el trabajo quedó bien.

- **RF-12-AC-1:** **Dado que** el cliente pagó el servicio, **cuando** el técnico marca el trabajo como terminado, **entonces** el pago sigue retenido y no se entrega al técnico.
- **RF-12-AC-2:** **Dado que** el trabajo está terminado y el pago retenido, **cuando** el cliente confirma que quedó bien, **entonces** el pago se libera al técnico.

#### RF-13: Recordatorio y liberación automática
El sistema debe enviar un recordatorio por WhatsApp al cliente si no confirma el trabajo en 48 horas desde que el servicio pasa a "Terminado", y liberar el pago al técnico automáticamente a las 72 horas sin respuesta.

- **RF-13-AC-1:** **Dado que** el servicio pasó a "Terminado", **cuando** pasan 48 horas sin que el cliente confirme ni reporte inconformidad, **entonces** el sistema envía un recordatorio por WhatsApp que indica cuándo se liberará el pago.
- **RF-13-AC-2:** **Dado que** el servicio pasó a "Terminado", **cuando** pasan 72 horas sin que el cliente confirme ni reporte inconformidad, **entonces** el pago se libera automáticamente al técnico y el servicio pasa a "Cerrado".
- **RF-13-AC-3:** **Dado que** el cliente reportó inconformidad antes de las 72 horas, **cuando** se cumple el plazo, **entonces** el pago no se libera.
- **RF-13-AC-4:** **Dado que** el servicio pasó a "Terminado", **cuando** el cliente consulta el servicio, **entonces** ve la fecha y hora en que se liberará el pago si no responde.

#### RF-14: Reporte de inconformidad
El sistema debe permitir al cliente reportar inconformidad con el trabajo adjuntando fotos, lo que congela el pago.

- **RF-14-AC-1:** **Dado que** el servicio está "Terminado" y no han pasado 72 horas, **cuando** el cliente envía un reporte de inconformidad con al menos una foto, **entonces** el pago queda congelado, el servicio pasa a "En disputa" y se notifica al técnico y al personal de FairFix.
- **RF-14-AC-2:** **Dado que** el cliente está llenando un reporte de inconformidad, **cuando** intenta enviarlo sin fotos, **entonces** el sistema no lo envía y le pide adjuntar al menos una.
- **RF-14-AC-3:** **Dado que** el servicio ya está "Cerrado", **cuando** el cliente intenta reportar inconformidad desde la aplicación, **entonces** el sistema no crea la disputa y le ofrece el botón "Necesito ayuda".

#### RF-15: Resolución de disputas
El sistema debe permitir al personal de FairFix resolver una disputa con una de tres resoluciones: nueva visita del técnico sin costo, reembolso parcial o reembolso total.

- **RF-15-AC-1:** **Dado que** un servicio está "En disputa", **cuando** el personal de FairFix intenta cerrarla, **entonces** el sistema le exige elegir una de las tres resoluciones.
- **RF-15-AC-2:** **Dado que** el personal eligió reembolso parcial o total, **cuando** confirma la resolución, **entonces** el reembolso se procesa a través de la pasarela de pagos y el cliente y el técnico reciben la resolución.
- **RF-15-AC-3:** **Dado que** el personal eligió nueva visita sin costo, **cuando** confirma la resolución, **entonces** se programa la nueva visita, el pago sigue retenido y el cliente y el técnico reciben la resolución.
- **RF-15-AC-4:** **Dado que** el personal eligió reembolso parcial, **cuando** captura el monto, **entonces** el sistema solo acepta un monto mayor que cero y menor que el total pagado, y libera al técnico la diferencia.

#### RF-16: Inicio de sesión sin contraseña
El sistema debe permitir iniciar sesión con número de teléfono y un código enviado por WhatsApp, sin contraseña.

- **RF-16-AC-1:** **Dado que** el usuario está en la pantalla de inicio, **cuando** ingresa un número de teléfono válido, **entonces** recibe un código por WhatsApp y en ningún momento se le pide contraseña.
- **RF-16-AC-2:** **Dado que** el usuario recibió el código, **cuando** lo ingresa correctamente, **entonces** inicia sesión.
- **RF-16-AC-3:** **Dado que** el usuario recibió el código, **cuando** ingresa un código incorrecto, **entonces** no inicia sesión y puede solicitar un nuevo código.
- **RF-16-AC-4:** **Dado que** el usuario recibió un código, **cuando** lo ingresa después de 10 minutos, **entonces** el código ya no es válido y puede solicitar uno nuevo.
- **RF-16-AC-5:** **Dado que** un número acumuló 5 códigos incorrectos seguidos, **cuando** intenta ingresar otro, **entonces** el sistema bloquea el inicio de sesión de ese número durante 15 minutos y le ofrece el botón "Necesito ayuda".

#### RF-17: Avisos por WhatsApp
El sistema debe enviar por WhatsApp los avisos importantes del servicio: servicio confirmado, técnico en camino, costo adicional por aprobar, trabajo terminado (pide confirmar), recordatorio de confirmación (RF-13), pago liberado, disputa abierta y resolución de disputa.

- **RF-17-AC-1:** **Dado que** el servicio está confirmado, **cuando** el técnico marca que va en camino, **entonces** el cliente recibe el aviso "el técnico va en camino" por WhatsApp.
- **RF-17-AC-2:** **Dado que** el servicio está en curso, **cuando** el técnico registra un costo adicional, **entonces** el cliente recibe por WhatsApp el aviso de que debe aprobarlo.
- **RF-17-AC-3:** **Dado que** un servicio cambia a cualquiera de los eventos listados, **cuando** el cambio se registra, **entonces** el cliente recibe el aviso correspondiente por WhatsApp en menos de 1 minuto (RNF-17).

#### RF-18: Botón "Necesito ayuda"
El sistema debe mostrar en todas las pantallas un botón "Necesito ayuda" que comunique al usuario con una persona por llamada o por WhatsApp.

- **RF-18-AC-1:** **Dado que** el usuario está en cualquier pantalla de la aplicación, **cuando** la pantalla se carga, **entonces** el botón "Necesito ayuda" está visible.
- **RF-18-AC-2:** **Dado que** el usuario pulsó "Necesito ayuda" dentro del horario de atención, **cuando** elige llamada o WhatsApp, **entonces** el sistema lo comunica con una persona de FairFix por el canal elegido.
- **RF-18-AC-3:** **Dado que** el usuario pulsó "Necesito ayuda" fuera del horario de atención, **cuando** se abre la ayuda, **entonces** el sistema le muestra el horario de atención y le permite dejar un mensaje por WhatsApp que se atiende al inicio del siguiente horario.

#### RF-19: Solicitud por teléfono
El sistema debe permitir que un asesor telefónico cree una solicitud de servicio a nombre del cliente.

- **RF-19-AC-1:** **Dado que** un cliente llamó a FairFix, **cuando** el asesor captura categoría, fecha, hora y técnico, **entonces** el sistema registra la solicitud a nombre del cliente.
- **RF-19-AC-2:** **Dado que** el asesor registró la solicitud, **cuando** se guarda, **entonces** el cliente recibe la confirmación por WhatsApp.

#### RF-20: Métodos de pago
El sistema debe aceptar pagos con tarjeta registrada, en OXXO o por transferencia.

- **RF-20-AC-1:** **Dado que** el cliente está pagando un servicio, **cuando** llega a la pantalla de pago, **entonces** puede elegir tarjeta registrada, OXXO o transferencia.
- **RF-20-AC-2:** **Dado que** el cliente eligió un método de pago y el técnico aceptó la solicitud (RF-28), **cuando** la pasarela reporta el pago como recibido, **entonces** el servicio pasa a "Confirmado" y el pago queda retenido.
- **RF-20-AC-3:** **Dado que** el cliente eligió OXXO o transferencia, **cuando** la pasarela todavía no reporta el pago, **entonces** el servicio no se confirma.

#### RF-21: Calificaciones y reseñas
El sistema debe permitir al cliente calificar y reseñar al técnico al cerrar el servicio, y mostrar esas calificaciones y reseñas en el perfil del técnico.

- **RF-21-AC-1:** **Dado que** el servicio está "Cerrado", **cuando** el cliente envía una calificación de 1 a 5 estrellas y, opcionalmente, una reseña de texto, **entonces** el sistema las guarda y actualiza el promedio del técnico.
- **RF-21-AC-2:** **Dado que** el cliente ya calificó un servicio, **cuando** intenta calificarlo de nuevo, **entonces** el sistema no se lo permite.
- **RF-21-AC-3:** **Dado que** un técnico tiene calificaciones, **cuando** un cliente consulta su perfil, **entonces** ve la calificación promedio y las reseñas.

#### RF-22: Estado del servicio
El sistema debe mostrar al cliente el estado actual del servicio, según los estados definidos en la sección 1.3.

- **RF-22-AC-1:** **Dado que** el cliente tiene un servicio, **cuando** lo consulta, **entonces** ve su estado actual, que es uno de los definidos en la sección 1.3.
- **RF-22-AC-2:** **Dado que** el cliente está viendo un servicio, **cuando** el técnico o el sistema cambian su estado, **entonces** el nuevo estado se muestra al cliente.
- **RF-22-AC-3:** **Dado que** un servicio está en un estado, **cuando** se intenta pasar a un estado que no aparece en la columna "Pasa a" de la sección 1.3, **entonces** el sistema rechaza el cambio.

#### RF-23: Recibo digital
El sistema debe generar un recibo digital al cerrar el servicio.

- **RF-23-AC-1:** **Dado que** el pago del servicio se liberó al técnico, **cuando** el servicio se cierra, **entonces** el sistema genera un recibo con técnico, trabajo, fecha, monto original, costos adicionales aprobados y total pagado.
- **RF-23-AC-2:** **Dado que** existe un recibo, **cuando** el cliente consulta su historial de servicios, **entonces** puede abrir el recibo de cada servicio cerrado.

#### RF-24: Promociones
El sistema debe permitir aplicar precios de introducción o promociones.

- **RF-24-AC-1:** **Dado que** el personal de FairFix creó una promoción con descuento y vigencia, **cuando** un cliente paga un servicio dentro de la vigencia, **entonces** el descuento se refleja en el total que ve y paga.
- **RF-24-AC-2:** **Dado que** una promoción ya venció, **cuando** un cliente paga un servicio, **entonces** el descuento no se aplica.
- **RF-24-AC-3:** **Dado que** un cliente pagó un servicio con promoción, **cuando** se libera el pago al técnico, **entonces** el técnico recibe el monto completo de su cotización (supuesto S-05).

#### RF-25: Indicadores de éxito
El sistema debe generar los indicadores de éxito: tasa de recontratación, costos adicionales rechazados o en disputa, diferencia entre el precio final y el rango de referencia, contrataciones completadas sin llamar a soporte, permanencia de técnicos y tiempo de pago a técnicos.

- **RF-25-AC-1:** **Dado que** el personal de FairFix está en el módulo de indicadores, **cuando** elige un periodo, **entonces** el sistema muestra los seis indicadores calculados para ese periodo con las fórmulas de la tabla siguiente.
- **RF-25-AC-2:** **Dado que** existe un conjunto de servicios de prueba con resultados conocidos, **cuando** se calculan los indicadores, **entonces** cada valor coincide con el calculado a mano con las mismas fórmulas.

| Indicador | Fórmula (en el periodo elegido) |
|---|---|
| Tasa de recontratación | Clientes con 2 o más servicios cerrados ÷ clientes con al menos 1 servicio cerrado. |
| Costos adicionales rechazados o en disputa | (Costos adicionales rechazados + servicios que pasaron a "En disputa") ÷ servicios cerrados. |
| Diferencia contra el rango de referencia | Promedio de (total pagado − precio máximo del rango) ÷ precio máximo del rango, solo en servicios cuyo total superó el rango. |
| Contrataciones sin soporte | Servicios cerrados en los que el cliente no usó "Necesito ayuda" ni RF-19 ÷ servicios cerrados. Se reporta también solo para clientes de 60 años o más, si el cliente registró su edad (RF-26). |
| Permanencia de técnicos | Técnicos verificados con al menos un servicio en el periodo ÷ técnicos verificados al inicio del periodo. |
| Tiempo de pago a técnicos | Promedio de horas entre "Terminado" y la liberación del pago. |

#### RF-26: Registro y perfil del cliente
El sistema debe permitir al cliente registrarse con su número de teléfono y capturar su nombre y la dirección donde se hará el servicio.

- **RF-26-AC-1:** **Dado que** un número de teléfono no está registrado, **cuando** el usuario valida el código de RF-16, **entonces** el sistema le pide nombre y dirección antes de solicitar su primer servicio.
- **RF-26-AC-2:** **Dado que** el cliente tiene una dirección registrada, **cuando** solicita un servicio, **entonces** puede usar esa dirección o capturar otra.
- **RF-26-AC-3:** **Dado que** el cliente está en su perfil, **cuando** edita su nombre, dirección o edad (opcional), **entonces** el sistema guarda los cambios.

#### RF-27: Cotizaciones del técnico por trabajo
El sistema debe permitir al técnico elegir qué trabajos del catálogo ofrece y registrar su cotización para cada uno.

- **RF-27-AC-1:** **Dado que** el técnico está verificado, **cuando** elige un trabajo del catálogo y captura su precio dentro del rango, **entonces** el sistema guarda la cotización y el técnico aparece para ese trabajo en RF-04.
- **RF-27-AC-2:** **Dado que** el técnico captura un precio fuera del rango, **cuando** guarda la cotización, **entonces** aplica RF-07 (justificación obligatoria).
- **RF-27-AC-3:** **Dado que** el técnico cambia su cotización, **cuando** la guarda, **entonces** el nuevo precio solo aplica a solicitudes creadas después del cambio.

#### RF-28: Gestión del servicio por el técnico
El sistema debe permitir al técnico registrar su disponibilidad, aceptar o rechazar solicitudes y actualizar el estado del servicio ("En camino", "En curso", "Terminado").

- **RF-28-AC-1:** **Dado que** el técnico está verificado, **cuando** registra los días y horarios en que trabaja, **entonces** solo aparece en RF-04 para esos horarios.
- **RF-28-AC-2:** **Dado que** el técnico tiene una solicitud en estado "Solicitado", **cuando** la acepta, **entonces** el sistema se lo notifica al cliente para que complete el pago.
- **RF-28-AC-3:** **Dado que** el técnico tiene una solicitud en estado "Solicitado", **cuando** la rechaza, **entonces** el servicio pasa a "Cancelado", se reembolsa cualquier pago recibido y se avisa al cliente para que elija otro técnico.
- **RF-28-AC-4:** **Dado que** el servicio está "Confirmado", **cuando** el técnico marca "En camino", "En curso" o "Terminado" en ese orden, **entonces** el estado se actualiza y se notifica al cliente (RF-17).

#### RF-29: Cancelación del servicio
El sistema debe permitir al cliente cancelar un servicio antes de que el técnico esté "En camino".

- **RF-29-AC-1:** **Dado que** el servicio está "Solicitado" o "Confirmado", **cuando** el cliente lo cancela, **entonces** el servicio pasa a "Cancelado", se reembolsa el pago completo y se notifica al técnico (supuesto S-04).
- **RF-29-AC-2:** **Dado que** el servicio está "En camino" o en un estado posterior, **cuando** el cliente intenta cancelarlo, **entonces** el sistema no lo permite desde la aplicación y le ofrece el botón "Necesito ayuda".

#### RF-30: Suspensión de técnicos
El sistema debe permitir al personal de FairFix suspender y reactivar técnicos, registrando el motivo.

- **RF-30-AC-1:** **Dado que** un técnico está verificado, **cuando** el personal lo suspende capturando un motivo, **entonces** el técnico deja de aparecer en RF-04 y recibe el motivo.
- **RF-30-AC-2:** **Dado que** un técnico suspendido tiene servicios "Confirmado" pendientes, **cuando** se suspende, **entonces** esos servicios se cancelan con reembolso completo y se avisa a los clientes.
- **RF-30-AC-3:** **Dado que** un técnico está suspendido, **cuando** el personal lo reactiva, **entonces** vuelve a aparecer en RF-04.

#### RF-31: Perfil público del técnico
El sistema debe mostrar a los clientes el perfil de cada técnico verificado.

- **RF-31-AC-1:** **Dado que** un técnico está verificado, **cuando** un cliente abre su perfil, **entonces** ve su foto, nombre, oficios, la marca "Verificado", sus cotizaciones, su calificación promedio y sus reseñas.
- **RF-31-AC-2:** **Dado que** un cliente abre el perfil de un técnico, **cuando** se muestra, **entonces** no aparecen sus documentos, domicilio ni referencias (RNF-09).

#### RF-32: Pagos del técnico
El sistema debe permitir al técnico consultar sus pagos retenidos, liberados y congelados.

- **RF-32-AC-1:** **Dado que** el técnico tiene servicios cobrados, **cuando** consulta sus pagos, **entonces** ve por servicio el monto, el estado del pago (retenido, liberado o congelado) y la fecha estimada o real de liberación.

### 3.2 Requerimientos no funcionales

| ID | Descripción | Categoría | Criterio de verificación |
|---|---|---|---|
| RNF-01 | La interfaz debe usar texto de al menos 18 pt, botones con área táctil de al menos 48 × 48 dp separados por al menos 8 dp, y una sola acción principal por pantalla. | Usabilidad | Inspección de las pantallas contra las medidas indicadas. |
| RNF-02 | La contratación de un servicio debe completarse en 5 pasos como máximo (ver "Paso" en la sección 1.3). | Usabilidad | Recorrido del flujo de RF-04 a RF-20 contando las pantallas. |
| RNF-03 | El usuario debe poder regresar a un paso anterior sin perder los datos ya capturados. | Usabilidad | Prueba: capturar datos, retroceder y avanzar; los datos se conservan. |
| RNF-04 | Los textos deben usar lenguaje cotidiano, sin términos técnicos como "pago en custodia" o "incidencia". | Usabilidad | Revisión de todos los textos contra una lista de términos prohibidos mantenida por FairFix, sin coincidencias. |
| RNF-05 | Después de cada acción, el sistema debe mostrar una confirmación que diga qué ocurrió (por ejemplo, "Listo, Juan llega mañana a las 10"). | Usabilidad | Cada acción que cambia datos muestra un mensaje de confirmación con el resultado. |
| RNF-06 | El sistema no debe mostrar al cliente temporizadores ni cuentas regresivas, y la sesión no debe expirar por inactividad mientras el cliente tenga un servicio activo. | Usabilidad | Inspección de pantallas; prueba de inactividad de 60 minutos con un servicio activo sin cierre de sesión. |
| RNF-07 | El sistema no debe almacenar datos de tarjetas; los pagos se procesan en una pasarela externa. | Seguridad | Revisión de la base de datos y los registros: no contienen números de tarjeta. |
| RNF-08 | El técnico solo debe ver la dirección exacta del cliente desde que el servicio está "Confirmado" hasta que pasa a "Terminado", y durante una nueva visita ordenada en una disputa. | Seguridad / privacidad | Prueba de acceso a la dirección en cada estado del servicio. |
| RNF-09 | Los documentos de los técnicos solo deben ser accesibles para el personal con rol de verificación. | Seguridad / privacidad | Prueba de acceso con cada rol; solo el rol de verificación los abre. |
| RNF-10 | Las fotos de evidencia deben eliminarse 6 meses después del cierre del servicio o de la resolución de su disputa (supuesto S-01). | Privacidad | Prueba con fechas simuladas: la foto existe a los 5 meses y no existe a los 6. |
| RNF-11 | Los eventos de cada servicio (cotización, fotos, aprobaciones y cambios de estado) deben registrarse en un historial que no se pueda modificar ni borrar, con fecha, hora y usuario. | Seguridad / auditabilidad | Intentar editar o borrar un evento con cualquier rol: el sistema lo impide. |
| RNF-12 | El flujo de un servicio en curso (RF-08 a RF-11) debe tener una disponibilidad de al menos 99.5 % mensual en horario de atención (supuesto S-02). | Disponibilidad | Monitoreo mensual del tiempo en servicio. |
| RNF-13 | Debe existir un teléfono de respaldo, atendido en horario de atención, para servicios en curso cuando la aplicación falle, y el número debe aparecer en los avisos de WhatsApp del servicio. | Disponibilidad | Verificar que el número aparece en los avisos y que la llamada es atendida en horario de atención. |
| RNF-14 | El soporte con personas debe operar de 8:00 a 20:00, hora local, todos los días. | Operación | Verificación del horario del canal de soporte. |
| RNF-15 | El cálculo del rango de precio debe estar separado del resto del sistema para poder sustituirlo más adelante por uno basado en datos de los servicios realizados en FairFix. | Escalabilidad / mantenibilidad | Revisión de diseño: el cálculo del rango es un componente independiente. |
| RNF-16 | Las pantallas de la aplicación deben cargar en menos de 3 segundos en el 95 % de los casos con una conexión 4G. | Rendimiento | Prueba de carga con red 4G simulada. |
| RNF-17 | Los avisos de WhatsApp (RF-17) y los códigos de inicio de sesión (RF-16) deben enviarse en menos de 1 minuto desde el evento que los genera. | Rendimiento | Medición del tiempo entre el evento y el envío. |
| RNF-18 | Toda la comunicación debe viajar cifrada (HTTPS/TLS) y los documentos de técnicos y fotos de evidencia deben almacenarse cifrados. | Seguridad | Revisión de configuración y prueba de acceso directo al almacenamiento. |
| RNF-19 | Cada usuario debe tener un rol (cliente, técnico, verificación, mediación, soporte, administración) y solo acceder a las funciones de su rol. | Seguridad | Pruebas de acceso por rol a cada función. |
| RNF-20 | La aplicación debe funcionar en Android e iOS en sus dos últimas versiones principales y publicarse en Play Store y App Store. | Portabilidad | Pruebas en dispositivos de cada versión. |
| RNF-21 | En una prueba de usabilidad con al menos 5 adultos mayores de 60 años, al menos el 80 % debe completar una contratación sin ayuda. | Usabilidad | Prueba de usabilidad moderada antes del lanzamiento. |

### 3.3 Requerimientos de dominio

| ID | Descripción |
|---|---|
| RD-01 | FairFix debe cumplir la Ley Federal de Protección de Datos Personales en Posesión de los Particulares y publicar un aviso de privacidad. |
| RD-02 | La retención del pago del cliente está sujeta a revisión legal antes de operar. |
| RD-03 | Los pagos deben procesarse a través de un proveedor externo (por ejemplo, Mercado Pago o Stripe); no hay manejo de dinero directo entre cliente y técnico. |
| RD-04 | Los servicios se cobran por trabajo realizado según un tabulador, no por hora. |
| RD-05 | El rango de precio inicial lo define el equipo de FairFix con investigación del mercado local (consulta a técnicos y precios de material en ferreterías). |
| RD-06 | Un técnico puede cotizar fuera del rango de referencia solo si lo justifica. |
| RD-07 | Si el cliente rechaza un costo adicional, el técnico solo cobra lo acordado inicialmente y debe dejar la instalación funcionando o, por lo menos, segura. |
| RD-08 | La falta de respuesta a un costo adicional cuenta como rechazo; la falta de respuesta a la confirmación del trabajo cuenta como aceptación. Como esta diferencia puede confundir al cliente, el sistema debe indicarla en cada caso: al pedir aprobar un costo adicional ("si no respondes, no se cobra") y al pedir confirmar el trabajo ("si no respondes, el pago se libera el [fecha]") (RF-13-AC-4). |
| RD-09 | La verificación de técnicos y la mediación de disputas las realiza personal de FairFix, no un proceso automático. |
| RD-10 | Un técnico puede acreditar su experiencia sin certificación formal, porque muchos aprendieron el oficio de manera empírica. |
| RD-11 | Las categorías de servicio incluyen al menos plomería, electricidad, gas, cerrajería y carpintería. |
