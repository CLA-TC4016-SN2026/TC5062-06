# Especificación de Requerimientos de Software (SRS)

**Proyecto:** Plataforma de servicios para el hogar con técnicos verificados
**Versión:** 1.1 (final, con validación parcial del stakeholder real)
**Fecha:** septiembre de 2026
**Autor:** Raul Adrian Delgado Rodriguez, A01246414
**Documentos base:** `proyecto_base.md`, `revision_SRS.md`, `casos_de_uso.puml`, `casos_de_uso.png`
**Estructura:** IEEE 830 simplificada

**Convención:** un valor marcado con **(P)** es una propuesta del equipo que no salió de la entrevista con el cliente y debe validarse. Las decisiones abiertas están en el Anexo A.

---

# 1. Introducción

## 1.1 Propósito del documento

Este documento especifica los requerimientos funcionales, no funcionales y de dominio de la plataforma de servicios para el hogar con técnicos verificados, en su primera versión (12 semanas). Está dirigido a tres lectores: quien diseñe e implemente el sistema (cada requerimiento indica los casos de uso que lo cubren), quien lo pruebe (cada requerimiento funcional trae criterios de aceptación con ID único y cada no funcional un método de verificación) y el profesor del curso, que lo evaluará como base del proyecto individual.

## 1.2 Alcance del sistema

**Nombre tentativo:** Plataforma de servicios para el hogar con técnicos verificados.

El sistema es una aplicación web que conecta a personas que necesitan un servicio para su hogar con técnicos verificados. Cubre el ciclo completo de un servicio: solicitud, consulta de perfiles, cotización previa con rango de precio de referencia, contratación, seguimiento por estados, autorización de costos adicionales con evidencia fotográfica, pago retenido hasta la confirmación del cliente, recibo digital, calificación e historial. Incluye un modo simplificado para adultos mayores y usuarios con poca experiencia digital.

Los servicios se organizan en un **catálogo de tipos de servicio** administrado por la plataforma (por ejemplo, plomería, electricidad y carpintería); el contenido inicial del catálogo está pendiente (Anexo A).

**Dentro del alcance:** clientes, técnicos y administradores como usuarios; pago con tarjeta o transferencia mediante una pasarela externa; notificaciones por WhatsApp, SMS y en la aplicación.

**Fuera del alcance de esta versión:**
- Pagos en efectivo (RD-04).
- Verificación de antecedentes penales de los técnicos (RD-06).
- Operación de atención telefónica: el sistema solo muestra el número de soporte (RF-15).
- Aplicaciones móviles nativas; se entrega una aplicación web responsiva **(P)**.
- Facturación fiscal (CFDI), porque no se exige alta fiscal al técnico (RD-01).

## 1.3 Definiciones y acrónimos

| Término | Definición |
|---------|------------|
| SRS | Especificación de Requerimientos de Software |
| RF / RNF / RD | Requerimiento funcional / no funcional / de dominio |
| AC | Criterio de aceptación en formato Dado que / cuando / entonces, con ID `RF-XX-AC-N` |
| UC | Caso de uso (ver `casos_de_uso.png`) |
| Cliente | Persona que solicita y paga un servicio para su hogar |
| Técnico | Profesional independiente que presta el servicio |
| Administrador | Personal de la plataforma que valida técnicos, mantiene el catálogo y resuelve inconformidades |
| Catálogo de servicios | Lista de tipos de servicio con su rango de precio de referencia y su costo de visita |
| Rango de referencia | Precio mínimo y máximo estimado para un tipo de servicio, tomado del catálogo |
| Costo de visita | Monto que se cobra al cliente si rechaza un costo adicional (definido en el catálogo) |
| Ventana de horario | Intervalo de tiempo en que el técnico se compromete a llegar |
| Técnico disponible | Técnico validado que ofrece el tipo de servicio solicitado y no tiene otro servicio en la misma ventana de horario |
| Trabajo completado | Servicio que llegó al estado "confirmado" |
| Pago retenido | Dinero que la pasarela de pago conserva hasta que el cliente confirma el trabajo o un administrador resuelve una inconformidad |
| Costo adicional | Monto que el técnico propone durante el servicio, por encima del monto fijado al aceptar |
| Modo simplificado | Versión de la interfaz con menos pantallas, texto grande y lenguaje sencillo |
| INE | Credencial para votar, usada como identificación oficial |
| PCI DSS | Estándar de seguridad para el manejo de datos de tarjetas de pago |
| p95 | Percentil 95: el 95% de las mediciones es igual o menor a ese valor |

---

# 2. Descripción general

## 2.1 Perspectiva del producto

El sistema es una aplicación web independiente, con backend, frontend, base de datos y autenticación. Hoy la contratación de estos servicios se hace por recomendaciones o grupos informales; el sistema la reemplaza por un canal formal, sin integrarse con ningún sistema existente.

Interactúa con dos sistemas externos:
- **Pasarela de pago:** procesa el cobro, retiene el dinero y lo libera o reembolsa (UC08, UC17).
- **Servicio de mensajería (WhatsApp y SMS):** entrega las notificaciones de cambio de estado (UC12).

Sus tres tipos de usuario (cliente, técnico y administrador) y sus casos de uso están en `casos_de_uso.png`.

## 2.2 Funciones del producto

| Función | Requerimientos |
|---------|----------------|
| Registro, inicio de sesión y control de acceso por rol | RF-01 |
| Creación de solicitudes de servicio | RF-02 |
| Validación de identidad de técnicos y consulta de perfiles | RF-03, RF-04 |
| Catálogo de servicios y cotización previa | RF-16, RF-05 |
| Contratación del técnico | RF-06 |
| Seguimiento por estados y notificaciones | RF-07, RF-08 |
| Autorización de costos adicionales | RF-09 |
| Pago retenido, inconformidades y cancelaciones | RF-10, RF-11, RF-12 |
| Recibo, calificación e historial | RF-13, RF-14 |
| Modo simplificado | RF-15 |

## 2.3 Características del usuario

Estos perfiles se basan en la entrevista simulada y en una entrevista real (cliente de 26 años); conviene confirmarlos con más entrevistados.

| Usuario | Características relevantes para el diseño |
|---------|-------------------------------------------|
| Cliente | Contrata servicios del hogar pocas veces al año. Usa el celular como dispositivo principal y WhatsApp a diario. Su conexión a internet puede caer al subir información. |
| Cliente en modo simplificado | Adulto mayor o persona con poca experiencia digital. Se pierde con más de dos o tres pantallas y con textos pequeños. Puede depender de un familiar que solicite el servicio por él. |
| Técnico | Profesional independiente, con frecuencia sin factura ni alta fiscal. Poca experiencia digital. Usa el celular mientras trabaja. |
| Administrador | Personal de la plataforma. Valida documentos, mantiene el catálogo y resuelve inconformidades. Usa computadora **(P)**. |

## 2.4 Restricciones

| # | Restricción | Origen |
|---|-------------|--------|
| R-1 | El proyecto se entrega en 12 semanas. | Curso |
| R-2 | Debe incluir backend, frontend, base de datos y autenticación. | Curso |
| R-3 | Los pagos se procesan con una pasarela externa certificada PCI DSS; el sistema no almacena datos de tarjeta. | RNF-04 |
| R-4 | Las notificaciones por WhatsApp dependen de un proveedor externo, con costo y proceso de aprobación propios; por eso existe el respaldo por SMS. | RF-08 |
| R-5 | Los montos se manejan en pesos mexicanos **(P)**. | Entrevista |
| R-6 | El técnico no está obligado a tener factura ni alta fiscal. | RD-01 |
| R-7 | La comisión de la plataforma no está definida; el diseño debe permitir configurarla. | RD-03 |
| R-8 | La conexión de los usuarios puede ser inestable. | RNF-07 |

---

# 3. Requerimientos específicos

## 3.1 Requerimientos funcionales

### Convenciones de esta sección

- **Identificadores:** cada criterio de aceptación tiene un ID único con el formato `RF-XX-AC-N` (por ejemplo, `RF-01-AC-1`). Estos IDs son la clave de trazabilidad: los endpoints del `openapi.yaml` y las pruebas automatizadas deben referenciarlos. En código, los guiones se escriben como guion bajo (`RF_01_AC_1`); el ID canónico es el de este documento.
- **Formato:** cada criterio sigue el esquema **Dado que** (precondición), **cuando** (acción), **entonces** (resultado verificable).
- **Valores marcados con (P):** son propuestas del equipo por validar (Anexo A).
- **Errores de acceso:** "sin sesión" se rechaza con 401 y "sin permiso para esa acción" se rechaza con 403.

### Estados del servicio

| Estado | Significado | Quién lo asigna |
|--------|-------------|-----------------|
| solicitado | La solicitud existe y aún no tiene un técnico con pago retenido. También es el estado al que regresa si el técnico rechaza o cancela. | Sistema |
| aceptado | El técnico aceptó y el pago quedó retenido. | Sistema |
| en camino | El técnico va hacia el domicilio. | Técnico |
| en proceso | El técnico trabaja en el servicio. | Técnico |
| terminado | El técnico terminó el trabajo. | Técnico |
| en revisión | El cliente reportó una inconformidad. | Sistema |
| confirmado | El cliente confirmó, o un administrador resolvió a favor del técnico; el pago se liberó. | Cliente / Administrador |
| cerrado | El servicio terminó sin trabajo completo: el cliente rechazó un costo adicional, o un administrador resolvió un reembolso total. | Sistema |
| cancelado | El cliente no eligió un técnico alternativo después de una cancelación o inasistencia. | Sistema |

---

### RF-01: Registro, inicio de sesión y roles
El sistema permitirá registrarse e iniciar sesión a clientes, técnicos y administradores, y limitará las funciones según el rol.
- **Casos de uso:** UC01

**Criterios de aceptación:**

- **RF-01-AC-1**
  **Dado que** un visitante sin cuenta,
  **cuando** se registra como cliente con nombre completo, teléfono, correo electrónico y una contraseña de al menos 8 caracteres **(P)**,
  **entonces** se crea una cuenta con rol cliente y puede iniciar sesión con ese correo y contraseña.

- **RF-01-AC-2**
  **Dado que** un visitante sin cuenta,
  **cuando** se registra como técnico con los mismos datos, más su especialidad (uno o más tipos de servicio del catálogo) y sus años de experiencia,
  **entonces** se crea una cuenta con rol técnico, con esa especialidad y esa experiencia en su perfil, y estado de validación "sin validar" (RF-03).

- **RF-01-AC-3**
  **Dado que** ya existe una cuenta con un correo,
  **cuando** un visitante intenta registrarse con ese mismo correo,
  **entonces** el sistema no crea la cuenta y responde que el correo ya está registrado.

- **RF-01-AC-4**
  **Dado que** un visitante usa el registro público,
  **cuando** envía el rol "administrador",
  **entonces** el sistema rechaza el registro y no crea ninguna cuenta.

- **RF-01-AC-5**
  **Dado que** un usuario registrado,
  **cuando** inicia sesión con correo y contraseña correctos,
  **entonces** el sistema abre la sesión y devuelve su rol.

- **RF-01-AC-6**
  **Dado que** un usuario o un visitante,
  **cuando** intenta iniciar sesión con un correo inexistente o con una contraseña incorrecta,
  **entonces** el sistema responde 401 con el mismo mensaje en ambos casos, sin indicar cuál dato falló.

- **RF-01-AC-7**
  **Dado que** un usuario con sesión y rol cliente,
  **cuando** intenta ejecutar una función exclusiva de técnico o de administrador,
  **entonces** el sistema responde 403 y no modifica ningún dato.

### RF-02: Crear solicitud de servicio
El cliente podrá crear una solicitud indicando tipo de servicio (del catálogo), descripción, hasta 5 fotos **(P)** y si es urgente.
- **Casos de uso:** UC02

**Criterios de aceptación:**

- **RF-02-AC-1**
  **Dado que** un cliente con sesión,
  **cuando** envía un tipo de servicio activo del catálogo y una descripción no vacía,
  **entonces** el sistema crea la solicitud en estado "solicitado", asociada a ese cliente, y aparece en su historial (RF-14).

- **RF-02-AC-2**
  **Dado que** un cliente con sesión,
  **cuando** envía la solicitud sin tipo de servicio o con la descripción vacía,
  **entonces** el sistema no crea la solicitud y devuelve un error de validación que nombra el campo faltante.

- **RF-02-AC-3**
  **Dado que** un tipo de servicio desactivado en el catálogo,
  **cuando** un cliente intenta crear una solicitud con ese tipo,
  **entonces** el sistema no la crea y devuelve un error de validación.

- **RF-02-AC-4**
  **Dado que** una solicitud con 5 fotos adjuntas,
  **cuando** el cliente intenta adjuntar una sexta,
  **entonces** el sistema la rechaza, conserva las 5 anteriores e indica el límite de 5.

- **RF-02-AC-5**
  **Dado que** un cliente crea una solicitud,
  **cuando** la marca como urgente,
  **entonces** la solicitud queda identificada como urgente y el sistema notifica a los técnicos disponibles en máximo 1 minuto (RNF-06).

- **RF-02-AC-6**
  **Dado que** un visitante sin sesión,
  **cuando** intenta crear una solicitud,
  **entonces** el sistema responde 401 y no la crea.

- **RF-02-AC-7**
  **Dado que** un cliente que está por adjuntar fotos a una solicitud,
  **cuando** abre la pantalla de carga de fotos,
  **entonces** el sistema muestra un aviso que pide fotografiar solo la falla y evitar personas, documentos y objetos de valor **(P)**.

### RF-03: Validación de identidad del técnico
El técnico enviará su INE y un comprobante de domicilio; el administrador validará la identidad, y solo los técnicos validados aparecerán en los resultados.
- **Casos de uso:** UC23, UC24

**Criterios de aceptación:**

- **RF-03-AC-1**
  **Dado que** un técnico con estado de validación "sin validar" o "rechazado",
  **cuando** un cliente consulta técnicos (RF-04),
  **entonces** ese técnico no aparece en los resultados.

- **RF-03-AC-2**
  **Dado que** un técnico que cargó solo uno de los dos documentos,
  **cuando** intenta enviarlos a revisión,
  **entonces** el sistema no los envía e indica qué documento falta.

- **RF-03-AC-3**
  **Dado que** un técnico que cargó INE y comprobante de domicilio,
  **cuando** los envía a revisión,
  **entonces** su estado de validación cambia a "en revisión" y aparece en la lista de pendientes del administrador.

- **RF-03-AC-4**
  **Dado que** un técnico "en revisión",
  **cuando** un administrador aprueba sus documentos,
  **entonces** su estado de validación cambia a "validado" y aparece en los resultados de RF-04 cuando esté disponible.

- **RF-03-AC-5**
  **Dado que** un técnico "en revisión",
  **cuando** un administrador rechaza sus documentos sin escribir un motivo,
  **entonces** el sistema no registra el rechazo y exige el motivo.

- **RF-03-AC-6**
  **Dado que** un técnico "en revisión",
  **cuando** un administrador lo rechaza con un motivo,
  **entonces** su estado cambia a "rechazado", el técnico recibe el motivo y puede volver a enviar documentos.

- **RF-03-AC-7**
  **Dado que** un usuario que no es administrador,
  **cuando** intenta aprobar o rechazar documentos,
  **entonces** el sistema responde 403 y el estado del técnico no cambia.

### RF-04: Consulta de técnicos disponibles
El sistema mostrará al cliente los técnicos disponibles para su solicitud con foto, nombre completo, especialidad, años de experiencia, calificación promedio, número de trabajos completados, los comentarios más recientes de clientes anteriores y la etiqueta "Nuevo" si no tienen historial.
- **Casos de uso:** UC05

**Criterios de aceptación:**

- **RF-04-AC-1**
  **Dado que** una solicitud "solicitado" y al menos un técnico disponible,
  **cuando** el cliente consulta técnicos,
  **entonces** cada resultado incluye foto, nombre completo, especialidad, años de experiencia, calificación promedio, número de trabajos completados y los 3 comentarios más recientes de clientes anteriores **(P)**.

- **RF-04-AC-2**
  **Dado que** un técnico disponible con cero trabajos completados,
  **cuando** aparece en los resultados,
  **entonces** muestra la etiqueta "Nuevo" en lugar de la calificación promedio.

- **RF-04-AC-3**
  **Dado que** un técnico validado que no ofrece el tipo de servicio de la solicitud, o que tiene otro servicio aceptado en la misma ventana de horario,
  **cuando** el cliente consulta técnicos,
  **entonces** ese técnico no aparece en los resultados.

- **RF-04-AC-4**
  **Dado que** ningún técnico está disponible para la solicitud,
  **cuando** el cliente consulta técnicos,
  **entonces** el sistema devuelve una lista vacía, muestra un mensaje que lo indica y permite elegir otra ventana de horario.

### RF-05: Cotización previa
Antes de contratar, el sistema mostrará la cotización previa: el rango mínimo–máximo de referencia del tipo de servicio (RF-16), con el máximo no mayor a 130% del mínimo, y la comisión de la plataforma desglosada.
- **Casos de uso:** UC06

**Criterios de aceptación:**

- **RF-05-AC-1**
  **Dado que** un tipo de servicio con rango de referencia y una comisión configurada,
  **cuando** el cliente consulta la cotización previa,
  **entonces** el sistema muestra el precio mínimo, el precio máximo y la comisión como tres conceptos separados, con los valores vigentes del catálogo.

- **RF-05-AC-2**
  **Dado que** cualquier cotización mostrada,
  **cuando** se compara el máximo con el mínimo,
  **entonces** el máximo es menor o igual a 1.3 veces el mínimo.

- **RF-05-AC-3**
  **Dado que** un cliente contrató con una cotización mostrada,
  **cuando** un administrador modifica después el rango del catálogo,
  **entonces** la cotización guardada en ese servicio conserva los valores originales.

### RF-06: Contratación del técnico
El cliente podrá elegir un técnico y confirmar una ventana de horario; el técnico podrá aceptar o rechazar la solicitud y, al aceptar, fijará el monto del servicio dentro del rango de referencia **(P)**.
- **Casos de uso:** UC07, UC10

**Criterios de aceptación:**

- **RF-06-AC-1**
  **Dado que** una solicitud en estado "solicitado" y un técnico disponible,
  **cuando** el cliente lo elige y confirma una ventana de horario,
  **entonces** la solicitud queda asociada a ese técnico, el técnico recibe una notificación y el cliente ve el estado "pendiente de respuesta del técnico".

- **RF-06-AC-2**
  **Dado que** un técnico que dejó de estar disponible en esa ventana de horario,
  **cuando** el cliente intenta elegirlo,
  **entonces** el sistema no asocia la solicitud y muestra la lista de técnicos actualizada.

- **RF-06-AC-3**
  **Dado que** una solicitud asociada a un técnico y un rango de referencia [mín, máx],
  **cuando** el técnico acepta y fija un monto entre mín y máx (incluidos),
  **entonces** el sistema guarda el monto y pide al cliente autorizar el pago (RF-10).

- **RF-06-AC-4**
  **Dado que** una solicitud asociada a un técnico y un rango de referencia [mín, máx],
  **cuando** el técnico intenta aceptar con un monto menor a mín o mayor a máx,
  **entonces** el sistema no acepta la solicitud e indica el rango permitido.

- **RF-06-AC-5**
  **Dado que** una solicitud asociada a un técnico,
  **cuando** el técnico la rechaza,
  **entonces** la solicitud queda sin técnico en estado "solicitado", el cliente recibe una notificación y se le ofrecen técnicos alternativos.

- **RF-06-AC-6**
  **Dado que** una solicitud asociada a otro técnico,
  **cuando** un técnico distinto intenta aceptarla o rechazarla,
  **entonces** el sistema responde 403 y la solicitud no cambia.

- **RF-06-AC-7**
  **Dado que** una solicitud urgente asociada a un técnico que no ha respondido,
  **cuando** pasan 10 minutos **(P)** desde que se le envió,
  **entonces** el sistema deja la solicitud sin técnico en estado "solicitado", notifica al cliente y le muestra técnicos alternativos disponibles.

### RF-07: Seguimiento por estados
El servicio avanzará por los estados: solicitado, aceptado, en camino, en proceso, terminado y confirmado. El técnico actualiza de "aceptado" a "terminado"; el cliente confirma, y el cliente ve el estado en todo momento.
- **Casos de uso:** UC11, UC14

**Criterios de aceptación:**

- **RF-07-AC-1**
  **Dado que** un servicio en estado "aceptado", "en camino" o "en proceso" y el técnico asignado,
  **cuando** el técnico lo cambia al estado inmediato siguiente (en camino, en proceso o terminado),
  **entonces** el servicio queda en ese estado.

- **RF-07-AC-2**
  **Dado que** un servicio en estado "aceptado",
  **cuando** el técnico intenta cambiarlo a "terminado" (saltar un estado) o cambiar un servicio "en proceso" a "en camino" (retroceder),
  **entonces** el sistema rechaza el cambio y el estado no se modifica.

- **RF-07-AC-3**
  **Dado que** un servicio en estado "terminado",
  **cuando** el técnico intenta cambiarlo a "confirmado",
  **entonces** el sistema responde 403 y el estado sigue en "terminado".

- **RF-07-AC-4**
  **Dado que** un servicio asignado a un técnico,
  **cuando** otro técnico intenta cambiar su estado,
  **entonces** el sistema responde 403 y el estado no cambia.

- **RF-07-AC-5**
  **Dado que** cualquier cambio de estado de un servicio,
  **cuando** se realiza,
  **entonces** el sistema guarda el estado anterior, el nuevo, la fecha y hora y el usuario que lo realizó.

- **RF-07-AC-6**
  **Dado que** un cliente con un servicio propio,
  **cuando** consulta ese servicio,
  **entonces** ve el estado actual y la lista de cambios de estado con fecha y hora.

- **RF-07-AC-7**
  **Dado que** un servicio de otro cliente,
  **cuando** un cliente intenta consultarlo,
  **entonces** el sistema responde 403 y no muestra datos del servicio.

### RF-08: Notificaciones
El sistema notificará cada cambio de estado al cliente por WhatsApp y en la aplicación, con respaldo por SMS.
- **Casos de uso:** UC12

**Criterios de aceptación:**

- **RF-08-AC-1**
  **Dado que** un servicio cambia de estado,
  **cuando** el cambio queda registrado,
  **entonces** el sistema crea una notificación en la aplicación y envía un mensaje de WhatsApp al teléfono del cliente (o al del contacto para avisos, RF-15-AC-3).

- **RF-08-AC-2**
  **Dado que** se envió un mensaje de WhatsApp,
  **cuando** pasan 2 minutos **(P)** sin confirmación de entrega,
  **entonces** el sistema envía un SMS con el mismo contenido al mismo teléfono.

- **RF-08-AC-3**
  **Dado que** se envió un mensaje de WhatsApp,
  **cuando** la entrega se confirma antes de 2 minutos,
  **entonces** el sistema no envía SMS.

- **RF-08-AC-4**
  **Dado que** cualquier notificación de cambio de estado,
  **cuando** se envía,
  **entonces** el mensaje incluye el identificador del servicio, el tipo de servicio, el nuevo estado y la fecha y hora del cambio.

### RF-09: Costo adicional
Para un costo adicional, el técnico enviará el monto y al menos una foto; el cliente lo aprobará o rechazará desde el celular. Sin aprobación no se cobra; si el cliente lo rechaza, solo se cobra el costo de visita.
- **Casos de uso:** UC13, UC15

**Criterios de aceptación:**

- **RF-09-AC-1**
  **Dado que** un servicio en estado "en proceso" y el técnico asignado,
  **cuando** envía un costo adicional con monto mayor a cero y al menos una foto,
  **entonces** el sistema lo registra como "pendiente de aprobación" y notifica al cliente con el monto y las fotos.

- **RF-09-AC-2**
  **Dado que** un servicio en estado "en proceso",
  **cuando** el técnico intenta enviar un costo adicional sin foto o con monto menor o igual a cero,
  **entonces** el sistema no lo registra y devuelve un error de validación.

- **RF-09-AC-3**
  **Dado que** un costo adicional "pendiente de aprobación",
  **cuando** el cliente lo aprueba,
  **entonces** el pago retenido aumenta en ese monto con el mismo método de pago **(P)** y el monto se libera al técnico junto con el resto al confirmar el servicio.

- **RF-09-AC-4**
  **Dado que** un costo adicional "pendiente de aprobación",
  **cuando** el cliente lo rechaza,
  **entonces** el servicio pasa a "cerrado", se libera al técnico solo el costo de visita del catálogo y el resto del pago retenido, incluida la comisión, se reembolsa al cliente **(P)**.

- **RF-09-AC-5**
  **Dado que** un costo adicional "pendiente de aprobación",
  **cuando** el cliente aún no responde,
  **entonces** el sistema no cobra el monto adicional y no permite al técnico cambiar el servicio a "terminado" **(P)**.

### RF-10: Pago retenido
El pago (tarjeta o transferencia) quedará retenido al aceptar el técnico y solo se liberará cuando el cliente confirme el trabajo o un administrador resuelva una inconformidad.
- **Casos de uso:** UC08, UC16, UC17

**Criterios de aceptación:**

- **RF-10-AC-1**
  **Dado que** un técnico aceptó una solicitud y fijó un monto M, con una comisión C,
  **cuando** el cliente autoriza el pago con tarjeta o transferencia y la pasarela confirma la retención,
  **entonces** el sistema registra un pago retenido de M + C y el servicio pasa a "aceptado".

- **RF-10-AC-2**
  **Dado que** un técnico aceptó una solicitud y fijó un monto,
  **cuando** la pasarela rechaza o no completa la retención,
  **entonces** el servicio sigue en "solicitado", no se registra ningún cobro y el cliente recibe un aviso del error.

- **RF-10-AC-3**
  **Dado que** un servicio en estado "terminado" sin inconformidad,
  **cuando** el cliente lo confirma,
  **entonces** el servicio pasa a "confirmado", se libera al técnico el monto M más los costos adicionales aprobados, y la comisión C queda para la plataforma.

- **RF-10-AC-4**
  **Dado que** un servicio en estado "terminado",
  **cuando** el cliente no lo confirma ni reporta inconformidad,
  **entonces** el pago sigue retenido y no se registra ninguna liberación.

- **RF-10-AC-5**
  **Dado que** una solicitud a contratar,
  **cuando** se intenta elegir un método de pago distinto de tarjeta o transferencia (por ejemplo, efectivo),
  **entonces** el sistema rechaza el método y no registra ninguna retención.

- **RF-10-AC-6**
  **Dado que** un servicio "terminado" de otro cliente,
  **cuando** un usuario distinto del cliente dueño intenta confirmarlo,
  **entonces** el sistema responde 403 y no libera el pago.

### RF-11: Inconformidad
El cliente podrá reportar inconformidad con fotos antes de confirmar; el pago seguirá retenido y el caso pasará a revisión de un administrador.
- **Casos de uso:** UC20, UC21

**Criterios de aceptación:**

- **RF-11-AC-1**
  **Dado que** un servicio en estado "terminado",
  **cuando** el cliente reporta una inconformidad con descripción y al menos una foto,
  **entonces** el servicio pasa a "en revisión" y aparece en la lista de casos pendientes del administrador.

- **RF-11-AC-2**
  **Dado que** un servicio en estado "terminado",
  **cuando** el cliente intenta reportar una inconformidad sin foto o sin descripción,
  **entonces** el sistema no registra el reporte y devuelve un error de validación.

- **RF-11-AC-3**
  **Dado que** un servicio en estado distinto de "terminado" (por ejemplo, "confirmado"),
  **cuando** el cliente intenta reportar una inconformidad,
  **entonces** el sistema rechaza el reporte y el servicio no cambia.

- **RF-11-AC-4**
  **Dado que** un servicio "en revisión",
  **cuando** el cliente intenta confirmarlo,
  **entonces** el sistema lo rechaza y el pago sigue retenido.

- **RF-11-AC-5**
  **Dado que** un servicio "en revisión",
  **cuando** un administrador resuelve liberar el pago al técnico,
  **entonces** el servicio pasa a "confirmado" y el pago se libera al técnico como en RF-10-AC-3.

- **RF-11-AC-6**
  **Dado que** un servicio "en revisión",
  **cuando** un administrador resuelve un reembolso total,
  **entonces** el servicio pasa a "cerrado" y el pago retenido se reembolsa completo al cliente.

- **RF-11-AC-7**
  **Dado que** un servicio "en revisión",
  **cuando** un administrador resuelve un reembolso parcial **(P)** indicando un monto para el cliente y un monto para el técnico cuya suma es igual al pago retenido,
  **entonces** el servicio pasa a "cerrado" y cada monto se entrega a su destinatario.

- **RF-11-AC-8**
  **Dado que** un administrador registra la resolución de un caso,
  **cuando** la resolución queda guardada,
  **entonces** el cliente y el técnico reciben una notificación con el resultado.

- **RF-11-AC-9**
  **Dado que** un usuario que no es administrador,
  **cuando** intenta resolver un caso "en revisión",
  **entonces** el sistema responde 403 y el caso no cambia.

### RF-12: Retraso, cancelación o inasistencia del técnico
Si el técnico se retrasa, el sistema avisará al cliente; si cancela o no llega, le ofrecerá además técnicos alternativos sin repetir la solicitud; las cancelaciones afectarán la calificación del técnico.
- **Casos de uso:** UC09

**Criterios de aceptación:**

- **RF-12-AC-1**
  **Dado que** un servicio en estado "aceptado" o "en camino" con pago retenido,
  **cuando** el técnico lo cancela,
  **entonces** el servicio regresa a "solicitado" sin técnico, el pago retenido se reembolsa completo **(P)** y el cliente recibe un aviso con la lista de técnicos alternativos disponibles.

- **RF-12-AC-2**
  **Dado que** un servicio "aceptado" cuya ventana de horario ya comenzó,
  **cuando** pasan 30 minutos **(P)** desde el inicio de la ventana sin que el estado sea "en camino" o posterior,
  **entonces** el sistema lo trata como inasistencia con el mismo resultado que RF-12-AC-1.

- **RF-12-AC-3**
  **Dado que** un servicio que regresó a "solicitado" por cancelación o inasistencia,
  **cuando** el cliente elige un técnico alternativo,
  **entonces** la nueva solicitud conserva el tipo de servicio, la descripción y las fotos originales sin que el cliente los capture de nuevo.

- **RF-12-AC-4**
  **Dado que** un servicio que regresó a "solicitado" por cancelación o inasistencia,
  **cuando** el cliente decide no elegir un técnico alternativo,
  **entonces** el servicio pasa a "cancelado" y no se registra ningún cobro.

- **RF-12-AC-5**
  **Dado que** un técnico cancela o no llega,
  **cuando** se registra el evento,
  **entonces** el sistema guarda un registro con el técnico, el servicio, el tipo de evento (cancelación o inasistencia) y la fecha y hora; el efecto en la calificación queda pendiente (A-8).

- **RF-12-AC-6**
  **Dado que** un servicio "aceptado" cuya ventana de horario ya comenzó,
  **cuando** pasan 10 minutos **(P)** desde el inicio de la ventana sin que el estado sea "en camino" o posterior,
  **entonces** el sistema notifica al cliente y al técnico, por los canales de RF-08, que el servicio lleva retraso.

### RF-13: Recibo y calificación
Al confirmar el servicio, el sistema generará un recibo digital y permitirá al cliente calificar y comentar al técnico.
- **Casos de uso:** UC18, UC19

**Criterios de aceptación:**

- **RF-13-AC-1**
  **Dado que** un servicio pasa a "confirmado",
  **cuando** se registra el cambio,
  **entonces** el sistema genera un recibo con folio único, fecha, cliente, técnico, tipo de servicio, monto, costos adicionales, comisión y método de pago.

- **RF-13-AC-2**
  **Dado que** un servicio "confirmado" que el cliente aún no calificó,
  **cuando** el cliente envía una calificación entera de 1 a 5 **(P)**, con o sin comentario de hasta 500 caracteres **(P)**,
  **entonces** el sistema guarda la calificación asociada al servicio y al técnico.

- **RF-13-AC-3**
  **Dado que** un servicio "confirmado",
  **cuando** el cliente envía una calificación fuera del rango de 1 a 5 o un comentario de más de 500 caracteres,
  **entonces** el sistema no la guarda y devuelve un error de validación.

- **RF-13-AC-4**
  **Dado que** un servicio que el cliente ya calificó,
  **cuando** intenta calificarlo otra vez,
  **entonces** el sistema rechaza la segunda calificación y conserva la primera.

- **RF-13-AC-5**
  **Dado que** un servicio que no está "confirmado", o que pertenece a otro cliente,
  **cuando** un cliente intenta calificarlo,
  **entonces** el sistema rechaza la calificación.

- **RF-13-AC-6**
  **Dado que** un técnico con calificaciones guardadas,
  **cuando** se guarda una nueva calificación,
  **entonces** su calificación promedio pasa a ser la media aritmética de todas sus calificaciones, incluida la nueva.

### RF-14: Historial
El cliente podrá consultar su historial de servicios y descargar los recibos.
- **Casos de uso:** UC22

**Criterios de aceptación:**

- **RF-14-AC-1**
  **Dado que** un cliente con sesión y servicios registrados,
  **cuando** consulta su historial,
  **entonces** ve la lista de sus propios servicios, cada uno con tipo, fecha, técnico, estado y monto.

- **RF-14-AC-2**
  **Dado que** un cliente con sesión y sin servicios,
  **cuando** consulta su historial,
  **entonces** el sistema devuelve una lista vacía y muestra un mensaje que lo indica.

- **RF-14-AC-3**
  **Dado que** un servicio "confirmado" del cliente,
  **cuando** el cliente descarga su recibo,
  **entonces** obtiene un archivo PDF **(P)** con los mismos campos definidos en RF-13-AC-1.

- **RF-14-AC-4**
  **Dado que** un servicio que no está "confirmado",
  **cuando** el cliente intenta descargar su recibo,
  **entonces** el sistema informa que el recibo no está disponible y no genera archivo.

- **RF-14-AC-5**
  **Dado que** servicios y recibos de otro cliente,
  **cuando** un cliente intenta consultarlos o descargarlos,
  **entonces** el sistema responde 403 y no expone ningún dato.

### RF-15: Modo simplificado
El sistema tendrá un modo simplificado activable, con solicitud en máximo 3 pantallas, la opción de que un familiar solicite a nombre de otra persona indicando un contacto para los avisos, y un botón para llamar a soporte.
- **Casos de uso:** UC03, UC04

**Criterios de aceptación:**

- **RF-15-AC-1**
  **Dado que** un cliente con sesión,
  **cuando** activa el modo simplificado, cierra sesión y vuelve a iniciarla,
  **entonces** el modo simplificado sigue activo.

- **RF-15-AC-2**
  **Dado que** un cliente en modo simplificado,
  **cuando** crea una solicitud desde la pantalla de inicio hasta el envío,
  **entonces** recorre como máximo 3 pantallas, sin contar el inicio de sesión.

- **RF-15-AC-3**
  **Dado que** un cliente con sesión,
  **cuando** crea una solicitud a nombre de otra persona indicando nombre y teléfono del contacto,
  **entonces** la solicitud guarda ese contacto y las notificaciones de RF-08 se envían a ese teléfono; el contacto no necesita una cuenta.

- **RF-15-AC-4**
  **Dado que** un cliente que crea una solicitud a nombre de otra persona,
  **cuando** omite el nombre o el teléfono del contacto,
  **entonces** el sistema no crea la solicitud y devuelve un error de validación que nombra el campo faltante.

- **RF-15-AC-5**
  **Dado que** un cliente en modo simplificado,
  **cuando** ve cualquier pantalla,
  **entonces** aparece un botón "Llamar a soporte" que marca el número de soporte configurado.

### RF-16: Catálogo de servicios y precios de referencia
El administrador podrá gestionar el catálogo de tipos de servicio, con su rango de precio de referencia (mínimo y máximo) y su costo de visita.
- **Casos de uso:** UC25

**Criterios de aceptación:**

- **RF-16-AC-1**
  **Dado que** un administrador con sesión,
  **cuando** crea un tipo de servicio con nombre, mínimo mayor a cero, máximo menor o igual a 1.3 veces el mínimo y costo de visita mayor o igual a cero,
  **entonces** el tipo queda activo y disponible para crear solicitudes (RF-02).

- **RF-16-AC-2**
  **Dado que** un administrador con sesión,
  **cuando** captura un rango cuyo máximo es mayor a 1.3 veces el mínimo, o un mínimo menor o igual a cero,
  **entonces** el sistema no lo guarda y devuelve un error de validación.

- **RF-16-AC-3**
  **Dado que** un tipo de servicio activo con solicitudes existentes,
  **cuando** el administrador lo desactiva,
  **entonces** ya no se ofrece en solicitudes nuevas y las solicitudes existentes conservan su tipo.

- **RF-16-AC-4**
  **Dado que** un tipo de servicio con servicios ya contratados,
  **cuando** el administrador cambia su rango o su costo de visita,
  **entonces** los servicios ya contratados conservan los valores con los que se contrataron (RF-05-AC-3).

- **RF-16-AC-5**
  **Dado que** un usuario que no es administrador,
  **cuando** intenta crear, editar o desactivar un tipo de servicio,
  **entonces** el sistema responde 403 y el catálogo no cambia.

## 3.2 Requerimientos no funcionales

| ID | Descripción | Categoría | Verificación (método y criterio de aprobación) |
|----|-------------|-----------|------------------------------------------------|
| RNF-01 | En el modo simplificado el texto será de al menos 18 px, los botones de al menos 48×48 px y el lenguaje sin tecnicismos **(P)** | Usabilidad | Inspección con herramientas del navegador de todas las pantallas del modo simplificado; se aprueba si el 100% del texto y los botones cumplen los tamaños |
| RNF-02 | Un técnico sin experiencia digital podrá actualizar el estado de un servicio en máximo 2 pasos **(P)** | Usabilidad | Prueba con al menos 3 personas sin experiencia digital; se aprueba si todas lo logran en 2 toques o menos |
| RNF-03 | La dirección exacta del cliente solo será visible para el técnico elegido, después de que este acepte el servicio | Seguridad | Prueba con la cuenta de otro técnico y con una solicitud pendiente; se aprueba si la dirección no aparece en pantalla ni en las respuestas de la API |
| RNF-04 | El sistema no almacenará datos de tarjeta, procesará pagos con una pasarela certificada PCI DSS, transmitirá todo por HTTPS y guardará las contraseñas con un algoritmo de hash con sal | Seguridad | Revisión de la base de datos y de la configuración; se aprueba si no existen campos de tarjeta, el tráfico HTTP se redirige a HTTPS y ninguna contraseña está en texto plano |
| RNF-05 | Las fotos y documentos solo serán accesibles para el cliente, el técnico asignado y los administradores | Seguridad / privacidad | Prueba de acceso con usuarios no autorizados y URL directas; se aprueba si todos los intentos reciben acceso denegado |
| RNF-06 | Una solicitud urgente notificará a los técnicos disponibles en máximo 1 minuto, y las pantallas de inicio de sesión, listado de técnicos y seguimiento cargarán en máximo 3 segundos (p95) en conexión 4G simulada **(P)** | Rendimiento | Medición del tiempo entre la creación de la solicitud y el envío de la notificación; medición de carga con red 4G simulada en 20 repeticiones por pantalla |
| RNF-07 | Si se pierde la conexión, el sistema conservará el borrador de la solicitud y reintentará automáticamente la subida de fotos | Confiabilidad | Prueba cortando la red durante la captura; se aprueba si al reconectar el borrador y las fotos se recuperan sin volver a capturarlos |
| RNF-08 | Un pago no se cobrará dos veces ante reintentos o fallas del sistema | Confiabilidad | Prueba enviando la misma operación de cobro dos veces; se aprueba si se registra un solo cargo |
| RNF-09 | La aplicación será responsiva con enfoque mobile-first y funcionará en las dos últimas versiones mayores de Chrome y Safari móviles, en pantallas de 360 a 430 px de ancho **(P)** | Compatibilidad | Ejecución del flujo completo (solicitud a recibo) en esos navegadores y anchos; se aprueba si no hay pantallas que requieran desplazamiento horizontal |
| RNF-10 | El sistema soportará 500 usuarios concurrentes con tiempo de respuesta de la API menor a 3 segundos (p95) y menos de 1% de errores **(P)** | Escalabilidad | Prueba de carga con 500 usuarios simulados durante 10 minutos; se aprueba si se cumplen ambos umbrales |

## 3.3 Requerimientos de dominio

- **RD-01:** Solo pueden prestar servicios los técnicos con identidad validada; no se exige factura ni alta fiscal, y los técnicos sin historial se muestran como "Nuevo". *Relacionado con:* RF-03, RF-04.
- **RD-02:** Ningún cobro puede hacerse sin autorización explícita del cliente: el precio inicial al contratar y cualquier costo adicional al aprobarlo. *Relacionado con:* RF-09, RF-10.
- **RD-03:** La comisión de la plataforma debe ser visible y clara antes de contratar; su monto queda por definir y debe poder configurarse. *Relacionado con:* RF-05.
- **RD-04:** La primera versión acepta solo tarjeta y transferencia; el efectivo queda fuera del alcance porque no permite retener el pago. **Validación:** el stakeholder real pidió poder pagar en efectivo; decisión abierta A-14. *Relacionado con:* RF-10.
- **RD-05:** Los datos personales (INE, domicilio, fotos) se tratarán con aviso de privacidad y conforme a la normativa de protección de datos personales aplicable (por confirmar). *Relacionado con:* RF-03, RNF-03, RNF-05.
- **RD-06:** La plataforma no verificará antecedentes penales en esta versión; la confianza se apoya en la identidad validada y en las calificaciones. **Validación:** el stakeholder real pidió antecedentes verificados; decisión abierta A-15. *Relacionado con:* RF-03, RF-13.

---

# Anexo A. Decisiones abiertas

Estos puntos hay que resolverlos con el stakeholder real antes de considerar cerrado el SRS.

| # | Decisión pendiente | Requerimientos afectados |
|---|--------------------|--------------------------|
| A-1 | Monto final: se propone que el técnico lo fije dentro del rango al aceptar y que el pago retenido sea ese monto más la comisión. | RF-06, RF-10 |
| A-2 | Plazo máximo de retención si el cliente no confirma ni reporta (por ejemplo, liberación automática tras cierto tiempo). | RF-10 |
| A-3 | Cancelación por parte del cliente: cuándo puede hacerlo y qué se le reembolsa. No está cubierta por ningún requerimiento. | RF-07, RF-10 |
| A-4 | Resoluciones posibles de una inconformidad y plazo en que el administrador debe resolver. | RF-11 |
| A-5 | Monto o porcentaje de la comisión. | RF-05, RD-03 |
| A-6 | Contenido inicial del catálogo: tipos de servicio, rangos y costo de visita. | RF-16 |
| A-7 | Tiempo máximo para que un técnico responda una solicitud y meta de aceptación en urgencias. El cliente simulado mencionó 30 minutos; el cliente real espera respuesta en menos de 10 minutos y un técnico en menos de una hora. RF-06-AC-7 usa 10 minutos **(P)** para solicitudes urgentes. | RF-06, RNF-06 |
| A-8 | Cómo afectan las cancelaciones a la calificación del técnico. | RF-12 |
| A-9 | Proveedores de la pasarela de pago y del servicio de WhatsApp. | RF-08, RF-10 |
| A-10 | Confirmar con más de un entrevistado las características de usuario (sección 2.3) y los valores marcados como (P). Se hizo una entrevista real; falta contrastar. | Todo el documento |
| A-11 | Reembolso cuando el técnico cancela: se propone reembolso completo inmediato y una nueva retención con el técnico alternativo. | RF-12 (RF-12-AC-1) |
| A-12 | Si el cliente rechaza un costo adicional, se propone reembolsar también la comisión. | RF-09 (RF-09-AC-4) |
| A-13 | Datos obligatorios del registro y regla de contraseña (se propone nombre completo, teléfono, correo y contraseña de 8 caracteres). | RF-01 (RF-01-AC-1) |
| A-14 | Efectivo: el cliente real pidió poder pagar con tarjeta, transferencia o efectivo. Opciones: mantenerlo fuera de la v1 (no se puede retener) o aceptarlo sin retención. | RD-04, RF-10 |
| A-15 | Antecedentes: el cliente real pidió técnicos con antecedentes verificados. Opciones: mantener la exclusión (RD-06) o pedir al técnico un documento de antecedentes como tercer documento. | RD-06, RF-03 |
| A-16 | Garantía por el trabajo realizado y función de emergencia 24/7: el cliente real los mencionó como valor importante y ningún requerimiento los cubre. | Sin RF |
| A-17 | Rastreo de la llegada del técnico: el cliente real quiere rastrearla; el SRS ofrece solo estados, no ubicación en tiempo real. | RF-07 |

---

# Anexo B. Matriz de trazabilidad

| RF | Criterios de aceptación | Casos de uso | RNF y RD relacionados |
|----|-------------------------|--------------|-----------------------|
| RF-01 | RF-01-AC-1 a RF-01-AC-7 (7) | UC01 | RNF-04 |
| RF-02 | RF-02-AC-1 a RF-02-AC-7 (7) | UC02 | RNF-05, RNF-06, RNF-07 |
| RF-03 | RF-03-AC-1 a RF-03-AC-7 (7) | UC23, UC24 | RD-01, RD-05, RNF-05 |
| RF-04 | RF-04-AC-1 a RF-04-AC-4 (4) | UC05 | RD-01 |
| RF-05 | RF-05-AC-1 a RF-05-AC-3 (3) | UC06 | RD-03 |
| RF-06 | RF-06-AC-1 a RF-06-AC-7 (7) | UC07, UC10 | RD-02, RNF-03 |
| RF-07 | RF-07-AC-1 a RF-07-AC-7 (7) | UC11, UC14 | RNF-02 |
| RF-08 | RF-08-AC-1 a RF-08-AC-4 (4) | UC12 | — |
| RF-09 | RF-09-AC-1 a RF-09-AC-5 (5) | UC13, UC15 | RD-02 |
| RF-10 | RF-10-AC-1 a RF-10-AC-6 (6) | UC08, UC16, UC17 | RD-02, RD-04, RNF-04, RNF-08 |
| RF-11 | RF-11-AC-1 a RF-11-AC-9 (9) | UC20, UC21 | RNF-05 |
| RF-12 | RF-12-AC-1 a RF-12-AC-6 (6) | UC09 | — |
| RF-13 | RF-13-AC-1 a RF-13-AC-6 (6) | UC18, UC19 | — |
| RF-14 | RF-14-AC-1 a RF-14-AC-5 (5) | UC22 | RNF-05 |
| RF-15 | RF-15-AC-1 a RF-15-AC-5 (5) | UC03, UC04 | RNF-01, RNF-09 |
| RF-16 | RF-16-AC-1 a RF-16-AC-5 (5) | UC25 | RD-03 |
| **Total** | **93 criterios** | | |
