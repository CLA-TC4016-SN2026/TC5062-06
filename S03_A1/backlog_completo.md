# Backlog del producto — FairFix

**Fuente:** `SRS_equipo.md`
**Fecha:** 3 de octubre de 2026
**Versión:** ajustada por el equipo (ver `ajustes_backlog.md`)

## Convenciones

- **Historia:** Como [tipo de usuario], quiero [acción], para [beneficio].
- **Criterios de aceptación:** cada uno tiene un ID `HU-XX-CA-N` y, entre paréntesis, los criterios del SRS de los que sale (`RF-XX-AC-N`). El criterio del SRS es el canónico; el de la historia es su resumen para planear.
- **Estimación:** story points en escala Fibonacci (1, 2, 3, 5, 8, 13). Es una estimación inicial relativa: HU-08 (cambiar el estado de un servicio) se tomó como referencia de 3 puntos.
- **Prioridad:** Alta = necesaria para el flujo básico de un servicio; Media = importante, entra después del flujo básico; Baja = se hace si queda tiempo en las 12 semanas.
- **IDs con sufijo:** las historias que resultaron de dividir otra conservan el número original con sufijo (`HU-02a`, `HU-02b`, `HU-11a`, `HU-11b`) para no renumerar el resto.
- **Alcance del backlog:** 18 historias que cubren el flujo completo de un servicio (18 de los 27 RF del SRS). Los RF que quedaron fuera de este backlog inicial están listados al final.

## Épicas

| Épica | Nombre | Qué agrupa | RF del SRS | Historias | Puntos |
|---|---|---|---|---|---|
| EP-01 | Cuentas y técnicos verificados | Acceso por rol, validación de técnicos y su oferta de trabajos | RF-01, RF-03, RF-17 | HU-01, HU-02a, HU-02b, HU-03 | 18 |
| EP-02 | Solicitud, cotización y contratación | Catálogo, solicitud, perfiles, cotización previa y contratación | RF-02, RF-04, RF-05, RF-06, RF-16 | HU-04 a HU-07 | 18 |
| EP-03 | Ejecución del servicio | Estados, notificaciones y costos adicionales | RF-07, RF-08, RF-09 | HU-08 a HU-10 | 24 |
| EP-04 | Pago, cierre y reputación | Pago retenido, liberación, inconformidad, recibo y calificación | RF-10, RF-11, RF-13, RF-14, RF-20 | HU-11a, HU-11b, HU-12 a HU-14 | 32 |
| EP-05 | Accesibilidad y ayuda | Modo simplificado y ayuda con personas | RF-15, RF-21 | HU-15 a HU-16 | 13 |
| | **Total** | | 18 RF | **18 historias** | **105** |

---

## EP-01: Cuentas y técnicos verificados

### HU-01: Inicio de sesión sin contraseña
**Como** cliente, **quiero** registrarme e iniciar sesión con mi número de teléfono y un código que me llega por WhatsApp, **para** entrar a la plataforma sin tener que recordar una contraseña.

- **Prioridad:** Alta · **Estimación:** 5 · **Origen:** RF-01
- **HU-01-CA-1** (RF-01-AC-1, RF-01-AC-5): **Dado que** ingresé mi teléfono y recibí un código, **cuando** escribo el código correcto antes de 10 minutos, **entonces** entro a mi cuenta sin que se me pida contraseña; si el teléfono es nuevo, se me piden nombre y dirección para crear la cuenta.
- **HU-01-CA-2** (RF-01-AC-6, RF-01-AC-8): **Dado que** recibí un código, **cuando** escribo uno incorrecto o vencido, **entonces** no entro y puedo pedir otro; tras 5 errores seguidos el teléfono se bloquea 15 minutos y se me ofrece "Necesito ayuda".
- **HU-01-CA-3** (RF-01-AC-9): **Dado que** el código se envió por WhatsApp, **cuando** pasan 2 minutos sin confirmación de entrega, **entonces** lo recibo por SMS.
- **HU-01-CA-4** (RF-01-AC-7): **Dado que** tengo sesión como cliente, **cuando** intento usar una función de técnico o de administrador, **entonces** el sistema responde 403 y no cambia ningún dato.

### HU-02a: Enviar documentos y acreditar experiencia
**Como** técnico, **quiero** enviar mis documentos y acreditar mi experiencia aunque no tenga certificación, **para** que la plataforma me valide y pueda recibir solicitudes.

- **Prioridad:** Alta · **Estimación:** 5 · **Origen:** RF-01, RF-03
- **HU-02a-CA-1** (RF-03-AC-2): **Dado que** me falta alguno de los cuatro documentos (INE, selfie, comprobante de domicilio, carta de no antecedentes), **cuando** intento enviarlos a revisión, **entonces** el sistema no los envía y me dice cuál falta.
- **HU-02a-CA-2** (RF-03-AC-3, RF-03-AC-8, RF-03-AC-9): **Dado que** cargué los cuatro documentos, **cuando** acredito mi experiencia con una certificación, o con al menos una foto de un trabajo anterior y dos referencias, **entonces** mi estado cambia a "en revisión".
- **HU-02a-CA-3** (RF-03-AC-1): **Dado que** mi estado es "sin validar", "en revisión" o "rechazado", **cuando** un cliente consulta técnicos, **entonces** no aparezco en los resultados.

### HU-02b: Validar a un técnico
**Como** administrador, **quiero** revisar los documentos de un técnico y aprobarlo o rechazarlo con un motivo, **para** que solo personas verificadas entren a las casas de los clientes.

- **Prioridad:** Alta · **Estimación:** 3 · **Origen:** RF-03
- **HU-02b-CA-1** (RF-03-AC-11, RF-03-AC-4): **Dado que** un técnico está "en revisión", **cuando** abro su solicitud y la apruebo, **entonces** veo todos sus documentos, fotos y referencias, y su estado cambia a "validado".
- **HU-02b-CA-2** (RF-03-AC-5, RF-03-AC-6): **Dado que** un técnico está "en revisión", **cuando** lo rechazo, **entonces** el sistema me exige un motivo, el técnico lo recibe y puede volver a enviar documentos.
- **HU-02b-CA-3** (RF-03-AC-7): **Dado que** un usuario no es administrador, **cuando** intenta aprobar o rechazar documentos, **entonces** el sistema responde 403.

### HU-03: Disponibilidad y cotizaciones del técnico
**Como** técnico validado, **quiero** registrar mis horarios y el precio que cobro por cada trabajo del catálogo, **para** aparecer solo en las solicitudes que puedo atender y que el cliente conozca mi precio desde el inicio.

- **Prioridad:** Alta · **Estimación:** 5 · **Origen:** RF-17
- **HU-03-CA-1** (RF-17-AC-1): **Dado que** registré mis días y horarios, **cuando** un cliente consulta técnicos, **entonces** solo aparezco para ventanas dentro de mi disponibilidad.
- **HU-03-CA-2** (RF-17-AC-2): **Dado que** elijo un trabajo del catálogo, **cuando** guardo un precio dentro del rango de referencia, **entonces** la cotización se guarda sin pedirme justificación.
- **HU-03-CA-3** (RF-17-AC-3, RF-17-AC-4): **Dado que** capturo un precio fuera del rango, **cuando** intento guardarlo sin justificación, **entonces** el sistema no lo guarda; con justificación lo guarda y lo marca como fuera de rango.
- **HU-03-CA-4** (RF-17-AC-5): **Dado que** cambio una cotización, **cuando** la guardo, **entonces** el nuevo precio solo aplica a solicitudes creadas después.

---

## EP-02: Solicitud, cotización y contratación

### HU-04: Catálogo de trabajos y precios de referencia
**Como** administrador, **quiero** crear, editar y desactivar los trabajos del catálogo con su rango de precio y su costo de visita, **para** que los clientes tengan una referencia de cuánto debería costar cada trabajo.

- **Prioridad:** Alta · **Estimación:** 3 · **Origen:** RF-16
- **HU-04-CA-1** (RF-16-AC-1, RF-16-AC-2): **Dado que** tengo sesión de administrador, **cuando** guardo un trabajo, **entonces** el sistema solo lo acepta si el mínimo es mayor a cero y el máximo no pasa de 1.3 veces el mínimo.
- **HU-04-CA-2** (RF-16-AC-3, RF-16-AC-4): **Dado que** un trabajo tiene solicitudes o servicios existentes, **cuando** lo desactivo o cambio su rango, **entonces** deja de ofrecerse en solicitudes nuevas y los servicios ya contratados conservan sus valores.
- **HU-04-CA-3** (RF-16-AC-5): **Dado que** un usuario no es administrador, **cuando** intenta modificar el catálogo, **entonces** el sistema responde 403.

### HU-05: Crear una solicitud de servicio
**Como** cliente, **quiero** elegir la categoría y el trabajo, describir mi problema y adjuntar fotos, **para** que un técnico entienda qué necesito antes de venir.

- **Prioridad:** Alta · **Estimación:** 5 · **Origen:** RF-02
- **HU-05-CA-1** (RF-02-AC-10, RF-02-AC-1): **Dado que** tengo sesión, **cuando** elijo una categoría, un trabajo activo y escribo una descripción, **entonces** la solicitud se crea en estado "solicitado" y aparece en mi historial.
- **HU-05-CA-2** (RF-02-AC-2): **Dado que** estoy creando una solicitud, **cuando** omito el trabajo o la descripción, **entonces** el sistema no la crea y nombra el dato faltante.
- **HU-05-CA-3** (RF-02-AC-4, RF-02-AC-7): **Dado que** voy a adjuntar fotos, **cuando** abro la carga de fotos, **entonces** veo el aviso de fotografiar solo la falla y no puedo adjuntar más de 5.
- **HU-05-CA-4** (RF-02-AC-5): **Dado que** marco la solicitud como urgente, **cuando** la envío, **entonces** los técnicos disponibles reciben la notificación en máximo 1 minuto.

### HU-06: Consultar técnicos y cotización previa
**Como** cliente, **quiero** ver los técnicos disponibles con su perfil, su precio y el rango normal del trabajo, **para** elegir con confianza y saber si el precio es justo antes de contratar.

- **Prioridad:** Alta · **Estimación:** 5 · **Origen:** RF-04, RF-05
- **HU-06-CA-1** (RF-04-AC-1, RF-04-AC-2): **Dado que** hay técnicos disponibles para mi solicitud, **cuando** los consulto, **entonces** cada uno muestra foto, nombre, marca "Verificado", especialidad, experiencia, calificación (o "Nuevo"), trabajos completados, cotización y comentarios recientes.
- **HU-06-CA-2** (RF-05-AC-1): **Dado que** elegí ver la cotización de un técnico, **cuando** se muestra, **entonces** veo por separado su precio, el texto "normalmente cuesta entre $X y $Y", la comisión y el total.
- **HU-06-CA-3** (RF-05-AC-4): **Dado que** el precio del técnico está fuera del rango, **cuando** veo la cotización, **entonces** aparece un aviso con su justificación.
- **HU-06-CA-4** (RF-04-AC-4, RF-04-AC-6): **Dado que** no hay técnicos disponibles, **cuando** consulto, **entonces** veo un mensaje que lo indica y puedo elegir otro horario o pedir ayuda; en ningún perfil veo documentos, domicilio ni teléfono del técnico.

### HU-07: Contratar a un técnico
**Como** cliente, **quiero** elegir un técnico y una ventana de horario y que él acepte o rechace, **para** dejar el servicio agendado con el precio que ya vi.

- **Prioridad:** Alta · **Estimación:** 5 · **Origen:** RF-06
- **HU-07-CA-1** (RF-06-AC-1): **Dado que** mi solicitud está "solicitado", **cuando** elijo un técnico disponible y confirmo la ventana de horario, **entonces** el técnico recibe la notificación y veo "pendiente de respuesta del técnico".
- **HU-07-CA-2** (RF-06-AC-3, RF-06-AC-4, RF-06-AC-8): **Dado que** el técnico acepta, **cuando** lo hace, **entonces** el monto es exactamente la cotización que vi, se me pide autorizar el pago y el acuerdo inicial queda guardado sin poder modificarse.
- **HU-07-CA-3** (RF-06-AC-5, RF-06-AC-7): **Dado que** el técnico rechaza, o no responde una solicitud urgente en 10 minutos, **cuando** eso ocurre, **entonces** la solicitud regresa a "solicitado" y se me ofrecen técnicos alternativos.

---

## EP-03: Ejecución del servicio

### HU-08: Actualizar el estado del servicio
**Como** técnico, **quiero** marcar con uno o dos toques que voy en camino, que estoy trabajando y que terminé, **para** que el cliente vea el avance de su servicio sin tener que llamarme.

- **Prioridad:** Alta · **Estimación:** 3 · **Origen:** RF-07, RNF-02
- **HU-08-CA-1** (RF-07-AC-1, RF-07-AC-2): **Dado que** soy el técnico asignado, **cuando** cambio el servicio al estado inmediato siguiente, **entonces** el cambio se guarda; si intento saltar o retroceder un estado, se rechaza.
- **HU-08-CA-2** (RF-07-AC-3, RF-07-AC-4): **Dado que** el servicio está "terminado" o es de otro técnico, **cuando** intento marcarlo "confirmado" o cambiar su estado, **entonces** el sistema responde 403.
- **HU-08-CA-3** (RF-07-AC-5, RF-07-AC-6): **Dado que** soy el técnico asignado, **cuando** cambio el estado del servicio, **entonces** el cambio queda registrado con fecha, hora y usuario, y el cliente dueño del servicio ve el nuevo estado y la lista de cambios al consultarlo.

### HU-09: Avisos por WhatsApp con respaldo por SMS
**Como** cliente, **quiero** recibir por WhatsApp cada cambio importante de mi servicio, **para** enterarme sin tener que abrir la aplicación.

- **Prioridad:** Alta · **Estimación:** 13 · **Origen:** RF-08
- **HU-09-CA-1** (RF-08-AC-1, RF-08-AC-4): **Dado que** mi servicio cambia de estado, **cuando** el cambio se registra, **entonces** recibo una notificación en la aplicación y un WhatsApp con el identificador del servicio, el trabajo, el nuevo estado y la fecha y hora.
- **HU-09-CA-2** (RF-08-AC-2, RF-08-AC-3): **Dado que** se envió el WhatsApp, **cuando** pasan 2 minutos sin confirmación de entrega, **entonces** recibo un SMS con el mismo contenido; si se confirma antes, no se envía SMS.
- **HU-09-CA-3** (RF-08-AC-5, RF-08-AC-6): **Dado que** hay un costo adicional por aprobar, un recordatorio, un pago liberado o una resolución, **cuando** el evento se registra, **entonces** el aviso llega en menos de 1 minuto e incluye el teléfono de respaldo de FairFix.

### HU-10: Autorizar costos adicionales con evidencia
**Como** cliente, **quiero** ver fotos y explicación de cualquier costo extra y decidir si lo acepto, **para** que nadie me cobre algo que no autoricé.

- **Prioridad:** Alta · **Estimación:** 8 · **Origen:** RF-09
- **HU-10-CA-1** (RF-09-AC-1, RF-09-AC-2): **Dado que** el servicio está "en proceso", **cuando** el técnico envía un costo adicional, **entonces** el sistema solo lo acepta con monto mayor a cero, descripción y de 1 a 5 fotos.
- **HU-10-CA-2** (RF-09-AC-7, RF-09-AC-3): **Dado que** recibí un costo adicional, **cuando** lo abro, **entonces** veo fotos, descripción, monto, total actualizado, las opciones "Aceptar", "Rechazar" y "Tengo dudas" y el aviso "si no respondes, este costo no se cobra"; si acepto, el pago retenido aumenta en ese monto.
- **HU-10-CA-3** (RF-09-AC-4, RF-09-AC-6): **Dado que** rechazo el costo adicional, **cuando** lo hago, **entonces** el total no cambia y el técnico termina lo acordado; si declara que no puede completarlo, el servicio se cierra, él recibe solo el costo de visita y el resto se me reembolsa.
- **HU-10-CA-4** (RF-09-AC-5, RF-09-AC-10): **Dado que** no respondí un costo adicional, **cuando** el técnico marca "terminado", **entonces** el costo queda rechazado y no entra en el total.
- **HU-10-CA-5** (RF-09-AC-9): **Dado que** el técnico corrige un error propio, **cuando** lo registra, **entonces** se guarda sin monto y no se me presenta como cobro.

---

## EP-04: Pago, cierre y reputación

### HU-11a: Pagar y retener el pago
**Como** cliente, **quiero** pagar con tarjeta, transferencia o en OXXO al contratar, **para** dejar el servicio asegurado sin que el técnico reciba el dinero todavía.

- **Prioridad:** Alta · **Estimación:** 8 · **Origen:** RF-10, RNF-04, RNF-08
- **HU-11a-CA-1** (RF-10-AC-1, RF-10-AC-5): **Dado que** el técnico aceptó, **cuando** autorizo el pago con tarjeta, transferencia u OXXO y la pasarela confirma, **entonces** queda retenido el monto más la comisión y el servicio pasa a "aceptado"; cualquier otro método se rechaza.
- **HU-11a-CA-2** (RF-10-AC-2, RF-10-AC-7): **Dado que** la pasarela rechaza el pago o todavía no lo reporta como recibido, **cuando** consulto el servicio, **entonces** sigue en "solicitado", no hay cobro registrado y veo el error o la referencia pendiente.
- **HU-11a-CA-3** (RNF-08): **Dado que** una operación de cobro se envía dos veces, **cuando** se procesa, **entonces** se registra un solo cargo.

### HU-11b: Confirmar el trabajo y liberar el pago
**Como** cliente, **quiero** confirmar que el trabajo quedó bien para que se le pague al técnico, **para** que el dinero solo salga cuando yo esté conforme.

- **Prioridad:** Alta · **Estimación:** 5 · **Origen:** RF-10
- **HU-11b-CA-1** (RF-10-AC-3): **Dado que** el servicio está "terminado" sin inconformidad, **cuando** lo confirmo, **entonces** pasa a "confirmado" y se libera al técnico el monto más los costos adicionales aprobados; la comisión queda para la plataforma.
- **HU-11b-CA-2** (RF-10-AC-6): **Dado que** el servicio "terminado" es de otro cliente, **cuando** un usuario distinto del dueño intenta confirmarlo, **entonces** el sistema responde 403 y no libera el pago.
- **HU-11b-CA-3** (RF-10-AC-4): **Dado que** el servicio está "terminado", **cuando** no lo confirmo ni reporto inconformidad y no han pasado 72 horas, **entonces** el pago sigue retenido.

### HU-12: Recordatorio y liberación automática del pago
**Como** técnico, **quiero** que el pago se libere solo si el cliente no responde en 72 horas, **para** no quedarme sin cobrar un trabajo terminado.

- **Prioridad:** Media · **Estimación:** 3 · **Origen:** RF-20
- **Dependencia:** validar con el stakeholder el plazo de 72 horas (decisión abierta A-3 del SRS). No debe entrar a un sprint antes de esa validación.
- **HU-12-CA-1** (RF-20-AC-1, RF-20-AC-4): **Dado que** el servicio pasó a "terminado", **cuando** pasan 48 horas sin respuesta del cliente, **entonces** el cliente recibe un recordatorio con la fecha y hora de liberación, que también ve al consultar el servicio.
- **HU-12-CA-2** (RF-20-AC-2): **Dado que** pasaron 72 horas sin confirmación ni inconformidad, **cuando** se cumple el plazo, **entonces** el servicio pasa a "confirmado", se libera el pago y se genera el recibo.
- **HU-12-CA-3** (RF-20-AC-3): **Dado que** el cliente reportó una inconformidad antes de las 72 horas, **cuando** se cumple el plazo, **entonces** el pago no se libera.

### HU-13: Reportar y resolver una inconformidad
**Como** cliente, **quiero** reportar con fotos que el trabajo no quedó bien antes de que se libere el pago, **para** que una persona de FairFix revise mi caso y decida qué procede.

- **Prioridad:** Media · **Estimación:** 8 · **Origen:** RF-11
- **HU-13-CA-1** (RF-11-AC-1, RF-11-AC-2): **Dado que** mi servicio está "terminado", **cuando** reporto una inconformidad con descripción y al menos una foto, **entonces** pasa a "en revisión" y el pago sigue retenido; sin foto o sin descripción el reporte no se registra.
- **HU-13-CA-2** (RF-11-AC-11, RF-11-AC-5, RF-11-AC-6, RF-11-AC-7, RF-11-AC-10): **Dado que** un caso está "en revisión", **cuando** el administrador lo cierra, **entonces** debe elegir una de cuatro resoluciones (liberar al técnico, nueva visita sin costo, reembolso parcial o reembolso total) y el dinero se mueve según la elegida.
- **HU-13-CA-3** (RF-11-AC-8, RF-11-AC-9): **Dado que** se guardó una resolución, **cuando** queda registrada, **entonces** el técnico y yo recibimos el resultado; un usuario que no es administrador no puede resolver el caso (403).

### HU-14: Recibo, historial y calificación
**Como** cliente, **quiero** recibir un recibo desglosado, consultar mis servicios anteriores y calificar al técnico, **para** tener comprobante de lo que pagué y ayudar a otros clientes a elegir.

- **Prioridad:** Alta · **Estimación:** 8 · **Origen:** RF-13, RF-14
- **HU-14-CA-1** (RF-13-AC-1, RF-13-AC-7): **Dado que** mi servicio pasa a "confirmado", **cuando** se registra, **entonces** se genera un recibo con folio único y el desglose (monto inicial, adicionales aprobados, comisión, descuento, total), y el total coincide exactamente con lo cobrado.
- **HU-14-CA-2** (RF-14-AC-1, RF-14-AC-3, RF-14-AC-5): **Dado que** tengo servicios registrados, **cuando** consulto mi historial, **entonces** veo cada uno con trabajo, fecha, técnico, estado y monto, y puedo descargar en PDF el recibo de los confirmados; los de otro cliente responden 403.
- **HU-14-CA-3** (RF-13-AC-2, RF-13-AC-3): **Dado que** mi servicio está "confirmado", **cuando** envío una calificación entera de 1 a 5 con o sin comentario de hasta 500 caracteres, **entonces** se guarda; fuera de esos límites se rechaza.
- **HU-14-CA-4** (RF-13-AC-4, RF-13-AC-5): **Dado que** ya califiqué el servicio, o no está "confirmado", o no es mío, **cuando** intento calificarlo, **entonces** el sistema lo rechaza.
- **HU-14-CA-5** (RF-13-AC-6): **Dado que** se guarda una calificación nueva, **cuando** se consulta el perfil del técnico, **entonces** su promedio es la media de todas sus calificaciones y se actualiza en menos de 5 segundos.

---

## EP-05: Accesibilidad y ayuda

### HU-15: Modo simplificado
**Como** adulto mayor con poca experiencia digital, **quiero** una versión de la aplicación con letras grandes y pocos pasos, **para** pedir un servicio yo solo sin equivocarme.

- **Prioridad:** Alta · **Estimación:** 8 · **Origen:** RF-15, RNF-01, RNF-11
- **HU-15-CA-1** (RF-15-AC-1, RF-15-AC-2): **Dado que** activé el modo simplificado, **cuando** cierro sesión y vuelvo a entrar, **entonces** sigue activo, y crear una solicitud me toma 3 pantallas como máximo.
- **HU-15-CA-2** (RNF-01): **Dado que** estoy en modo simplificado, **cuando** veo cualquier pantalla, **entonces** el texto mide al menos 18 pt, los botones al menos 48×48 px y hay una sola acción principal.
- **HU-15-CA-3** (RF-15-AC-6): **Dado que** voy a autorizar un pago, **cuando** llego al último paso, **entonces** veo en una sola pantalla el trabajo, el técnico, el horario y el total, y no se cobra hasta que confirmo.
- **HU-15-CA-4** (RF-15-AC-3, RF-15-AC-4): **Dado que** un familiar solicita a mi nombre, **cuando** indica mi nombre y teléfono como contacto, **entonces** los avisos me llegan a mí sin que yo necesite una cuenta; si omite alguno de los dos datos, la solicitud no se crea.

### HU-16: Ayuda de una persona y canal de dudas
**Como** cliente, **quiero** un botón "Necesito ayuda" en todas las pantallas y la opción "Tengo dudas" ante un costo adicional, **para** hablar con una persona cuando no entiendo algo.

- **Prioridad:** Media · **Estimación:** 5 · **Origen:** RF-21
- **HU-16-CA-1** (RF-21-AC-1, RF-21-AC-2, RF-21-AC-3): **Dado que** estoy en cualquier pantalla, **cuando** pulso "Necesito ayuda", **entonces** dentro del horario de atención me comunico por llamada o WhatsApp con una persona, y fuera de horario veo el horario y puedo dejar un mensaje.
- **HU-16-CA-2** (RF-21-AC-4, RF-21-AC-5): **Dado que** estoy viendo un costo adicional, **cuando** pulso "Tengo dudas", **entonces** se abre una conversación con el técnico y un administrador, el costo sigue pendiente y puedo aceptarlo o rechazarlo desde ahí.
- **HU-16-CA-3** (RF-21-AC-7): **Dado que** hay una conversación de dudas, **cuando** la consulto, **entonces** no veo el teléfono del técnico ni él ve el mío.

---

## Resumen del backlog

| ID | Historia | Épica | Prioridad | Puntos |
|---|---|---|---|---|
| HU-01 | Inicio de sesión sin contraseña | EP-01 | Alta | 5 |
| HU-02a | Enviar documentos y acreditar experiencia | EP-01 | Alta | 5 |
| HU-02b | Validar a un técnico | EP-01 | Alta | 3 |
| HU-03 | Disponibilidad y cotizaciones del técnico | EP-01 | Alta | 5 |
| HU-04 | Catálogo de trabajos y precios de referencia | EP-02 | Alta | 3 |
| HU-05 | Crear una solicitud de servicio | EP-02 | Alta | 5 |
| HU-06 | Consultar técnicos y cotización previa | EP-02 | Alta | 5 |
| HU-07 | Contratar a un técnico | EP-02 | Alta | 5 |
| HU-08 | Actualizar el estado del servicio | EP-03 | Alta | 3 |
| HU-09 | Avisos por WhatsApp con respaldo por SMS | EP-03 | Alta | 13 |
| HU-10 | Autorizar costos adicionales con evidencia | EP-03 | Alta | 8 |
| HU-11a | Pagar y retener el pago | EP-04 | Alta | 8 |
| HU-11b | Confirmar el trabajo y liberar el pago | EP-04 | Alta | 5 |
| HU-12 | Recordatorio y liberación automática del pago | EP-04 | Media | 3 |
| HU-13 | Reportar y resolver una inconformidad | EP-04 | Media | 8 |
| HU-14 | Recibo, historial y calificación | EP-04 | Alta | 8 |
| HU-15 | Modo simplificado | EP-05 | Alta | 8 |
| HU-16 | Ayuda de una persona y canal de dudas | EP-05 | Media | 5 |

| Prioridad | Historias | Puntos |
|---|---|---|
| Alta | 15 | 89 |
| Media | 3 | 16 |
| Baja | 0 | 0 |
| **Total** | **18** | **105** |

## Orden sugerido por dependencias

1. HU-01, HU-04 — acceso y catálogo; todo lo demás depende de ellos.
2. HU-02a, HU-02b, HU-03 — técnicos validados con cotizaciones.
3. HU-05, HU-06, HU-07 — solicitud, consulta y contratación.
4. HU-11a — retención del pago (la de mayor riesgo técnico por la pasarela).
5. HU-08, HU-09, HU-10 — ejecución, avisos y costos adicionales.
6. HU-11b, HU-14 — confirmación con liberación, recibo y calificación.
7. HU-15 — modo simplificado sobre el flujo ya construido.
8. HU-12, HU-13, HU-16 — historias de prioridad Media: liberación automática, inconformidad y ayuda.

## RF del SRS fuera de este backlog inicial

Estos requerimientos siguen en el SRS; no tienen historia todavía y entrarían en una siguiente versión del backlog.

| RF | Tema | Prioridad en el SRS |
|---|---|---|
| RF-12 | Retraso, cancelación o inasistencia del técnico | Media |
| RF-18 | Trabajos especiales y visita de diagnóstico (y trabajos parametrizables de RF-02 y RF-16) | Media |
| RF-19 | Cancelación por el cliente | Media |
| RF-23 | Suspensión y reactivación de técnicos | Media |
| RF-24 | Pagos del técnico | Media |
| RF-22 | Solicitud asistida por teléfono | Baja |
| RF-25 | Garantía posterior al servicio | Baja |
| RF-26 | Promociones | Baja |
| RF-27 | Indicadores de éxito | Baja |
