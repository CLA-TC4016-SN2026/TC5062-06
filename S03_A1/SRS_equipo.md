# Especificación de Requerimientos de Software (SRS) — FairFix

**Proyecto:** FairFix, plataforma de servicios para el hogar con técnicos verificados
**Versión:** 2.0 (consolidada del equipo)
**Fecha:** 3 de octubre de 2026
**Estructura:** IEEE 830 simplificada
**Integrantes:**

| Integrante | Matrícula | SRS individual |
|---|---|---|
| Raul Adrian Delgado Rodriguez | A01246414 | `SRS_final_A01246414_Raul.md` |
| Alvaro Daniel Zavala Arreola | A01796929 | `SRS_final_A01796929_Alvaro.md` |
| Luis Manuel Mendoza Cruz | A01797418 | `SRS_final_A01797418_Luis.md` |
| Carlos Monir Radovich Saad | A01797569 | `SRS_final_A01797569_Carlos.md` |

**Documento relacionado:** `diferencias_SRS.md` (consenso, gaps, conflictos, resolución y equivalencia de IDs).

**Convención:** un valor marcado con **(P)** es una propuesta del equipo que no salió de una entrevista y debe validarse con el stakeholder. Las decisiones abiertas están en el Anexo A.

---

# 1. Introducción

## 1.1 Propósito del documento

Este documento especifica los requerimientos funcionales, no funcionales y de dominio de FairFix en su primera versión (12 semanas). Integra los cuatro SRS individuales del equipo en una sola versión de referencia.

Está dirigido a quien diseñe e implemente el sistema (a partir de él se diseñará la API REST), a quien lo pruebe (cada requerimiento funcional trae criterios de aceptación con ID único, de los que se derivarán las pruebas BDD) y al profesor del curso.

## 1.2 Alcance del sistema

FairFix es una aplicación web responsiva que conecta a personas que necesitan un servicio para su hogar con técnicos independientes verificados. Cubre el ciclo completo de un servicio: solicitud, consulta de perfiles, cotización previa con rango de precio de referencia, contratación, seguimiento por estados, autorización de costos adicionales con evidencia fotográfica, pago retenido hasta la confirmación del cliente, recibo digital, calificación, historial y garantía. Incluye un modo simplificado para adultos mayores y usuarios con poca experiencia digital.

Los servicios se organizan en un **catálogo** administrado por la plataforma, con tres categorías iniciales: plomería, electricidad y carpintería (RD-16). Hay dos tipos de trabajo:

1. **Trabajo de catálogo:** trabajo común con rango de precio de referencia; puede ser parametrizable (su precio depende de variables como cantidad o capacidad).
2. **Trabajo especial:** trabajo que no está en el catálogo y requiere una cotización particular del técnico, con visita de diagnóstico si la información remota no alcanza.

**Dentro del alcance:** clientes, técnicos y administradores como usuarios; pago con tarjeta, transferencia o en OXXO mediante una pasarela externa; notificaciones por WhatsApp, SMS y en la aplicación; ayuda con una persona en horario de atención.

**Fuera del alcance de esta versión:**

- Pago en efectivo entregado directamente al técnico (RD-04).
- Consulta directa de antecedentes penales ante autoridades; la plataforma solo revisa la carta que entrega el técnico (RD-06).
- Aplicaciones móviles nativas y publicación en tiendas; se entrega una aplicación web responsiva.
- Facturación fiscal (CFDI) y retención automática de ISR/IVA (RD-15).
- Cálculo automático o algorítmico del rango de precio; en esta versión el rango se captura manualmente (RD-08, RNF-18).
- Verificación automática de documentos y arbitraje automático de disputas (RD-13).
- Cierre del servicio con código OTP entregado al técnico y confirmación por audio o llamada automatizada.
- Venta, compra o transporte de materiales; pólizas de seguro de daños a terceros.
- Calificación del cliente por parte del técnico.
- Rastreo de la ubicación del técnico en tiempo real y atención de emergencias 24/7 (Anexo A).

## 1.3 Definiciones y acrónimos

| Término | Definición |
|---|---|
| SRS | Especificación de Requerimientos de Software |
| RF / RNF / RD | Requerimiento funcional / no funcional / de dominio |
| AC | Criterio de aceptación en formato Dado que / cuando / entonces, con ID `RF-XX-AC-N` |
| UC | Caso de uso (ver `casos_de_uso.png`) |
| Cliente | Persona que solicita y paga un servicio para su hogar |
| Técnico | Profesional independiente que presta el servicio |
| Técnico validado | Técnico cuya identidad, carta de no antecedentes y experiencia aprobó un administrador; se muestra con la marca "Verificado" |
| Administrador | Personal de FairFix que valida técnicos, mantiene el catálogo, media dudas, resuelve inconformidades y da soporte |
| Catálogo | Lista de categorías y trabajos, cada trabajo con su rango de referencia y su costo de visita |
| Trabajo parametrizable | Trabajo de catálogo cuyo precio depende de variables definidas (cantidad, capacidad, distancia, tamaño) |
| Trabajo especial | Trabajo fuera del catálogo que requiere una cotización particular |
| Rango de referencia | Precio mínimo y máximo habitual de un trabajo del catálogo |
| Cotización | Precio que el técnico tiene registrado para un trabajo del catálogo, o que emite para un trabajo especial, y que el cliente ve antes de contratar |
| Acuerdo inicial | Alcance, precio y evidencia guardados cuando el cliente contrata; referencia para distinguir cambios posteriores |
| Costo de visita | Monto que recibe el técnico cuando el trabajo no puede completarse tras el rechazo de un costo adicional, o por una visita de diagnóstico |
| Visita de diagnóstico | Evaluación presencial previa a la cotización de un trabajo especial |
| Ventana de horario | Intervalo de tiempo en que el técnico se compromete a llegar |
| Técnico disponible | Técnico validado y no suspendido que ofrece el trabajo solicitado, registró disponibilidad en la ventana de horario y no tiene otro servicio en ella |
| Pago retenido | Dinero que la pasarela conserva hasta que el cliente confirma, vence el plazo de liberación o un administrador resuelve una inconformidad |
| Costo adicional | Monto que el técnico propone durante el servicio por un imprevisto fuera del acuerdo inicial |
| Inconformidad | Reporte del cliente sobre un trabajo terminado; mantiene el pago retenido hasta que un administrador resuelve |
| Modo simplificado | Versión de la interfaz con menos pantallas, texto grande y lenguaje sencillo |
| Horario de atención | De 8:00 a 20:00, hora local, todos los días |
| Código de acceso | Código numérico de un solo uso enviado al teléfono del usuario para iniciar sesión |
| INE | Credencial para votar, usada como identificación oficial |
| LFPDPPP | Ley Federal de Protección de Datos Personales en Posesión de los Particulares |
| PCI DSS | Estándar de seguridad para el manejo de datos de tarjetas de pago |
| WCAG | Pautas de accesibilidad para el contenido web |
| MFA | Autenticación con más de un factor |
| p95 | Percentil 95: el 95% de las mediciones es igual o menor a ese valor |

---

# 2. Descripción general

## 2.1 Perspectiva del producto

Hoy la contratación de estos servicios se hace por recomendaciones o grupos informales: el cliente no sabe cuánto debería costar el trabajo ni qué tan confiable es quien entra a su casa, y enfrenta cobros extra a mitad del servicio. FairFix reemplaza ese canal por uno formal que deja registro de la cotización, las evidencias y las autorizaciones de ambas partes.

Es una aplicación web independiente, con backend (API REST), frontend, base de datos y autenticación. No se integra con ningún sistema existente. Interactúa con estos sistemas externos:

| Sistema externo | Uso |
|---|---|
| Pasarela de pago | Cobro, retención, liberación al técnico y reembolsos |
| Servicio de mensajería (WhatsApp y SMS) | Códigos de acceso y avisos del servicio |
| Canal telefónico | Ayuda con una persona, solicitudes por teléfono y respaldo si la aplicación falla |

## 2.2 Funciones del producto

| Función | Requerimientos |
|---|---|
| Registro, inicio de sesión y control de acceso por rol | RF-01 |
| Solicitud de trabajos de catálogo y trabajos especiales | RF-02, RF-18 |
| Validación de técnicos, oferta del técnico y consulta de perfiles | RF-03, RF-17, RF-04, RF-23 |
| Catálogo de servicios y cotización previa | RF-16, RF-05 |
| Contratación y cancelación | RF-06, RF-19 |
| Seguimiento por estados y notificaciones | RF-07, RF-08 |
| Autorización de costos adicionales | RF-09 |
| Pago retenido, liberación, inconformidades y garantía | RF-10, RF-20, RF-11, RF-25 |
| Retrasos, cancelaciones e inasistencias del técnico | RF-12 |
| Recibo, calificación, historial y pagos del técnico | RF-13, RF-14, RF-24 |
| Modo simplificado, ayuda y solicitud por teléfono | RF-15, RF-21, RF-22 |
| Promociones e indicadores | RF-26, RF-27 |

## 2.3 Características del usuario

| Usuario | Características relevantes para el diseño |
|---|---|
| Cliente | Contrata servicios del hogar pocas veces al año, a veces con urgencia. No tiene conocimiento técnico. Usa el celular como dispositivo principal y WhatsApp a diario. Su conexión puede caer al subir información. |
| Cliente en modo simplificado | Adulto mayor o persona con poca experiencia digital. Olvida contraseñas, se equivoca con botones pequeños, se pierde con más de dos o tres pantallas y se pone nervioso con límites de tiempo. Puede depender de un familiar que solicite por él. |
| Técnico | Profesional independiente, con frecuencia sin certificación formal, factura ni alta fiscal. Poca experiencia digital. Usa el celular mientras trabaja. Busca cobrar a tiempo y demostrar su experiencia. |
| Administrador | Personal de FairFix, equipo reducido. Valida documentos, mantiene el catálogo, media dudas, resuelve inconformidades y atiende soporte de forma manual. Usa computadora. |

## 2.4 Restricciones

| # | Restricción | Relacionado con |
|---|---|---|
| R-1 | El proyecto se entrega en 12 semanas. Los requerimientos tienen prioridad (sección 3.1) para decidir qué entra primero. | Curso |
| R-2 | Debe incluir backend, frontend, base de datos y autenticación. | Curso |
| R-3 | Los pagos se procesan con una pasarela externa certificada PCI DSS; el sistema no almacena datos de tarjeta. | RNF-04 |
| R-4 | WhatsApp depende de un proveedor externo con costo y aprobación propios; por eso existe el respaldo por SMS. | RF-08 |
| R-5 | Los montos se manejan en pesos mexicanos. | — |
| R-6 | El técnico no está obligado a tener factura, alta fiscal ni certificación formal. | RD-01 |
| R-7 | La comisión de la plataforma no está definida; debe poder configurarse. | RD-03 |
| R-8 | La conexión de los usuarios puede ser inestable. | RNF-07 |
| R-9 | La ayuda con personas, la validación de técnicos y la mediación son manuales y operan en horario de atención. | RD-13, RNF-16 |
| R-10 | El cliente y el técnico no intercambian teléfonos ni dinero directamente. | RNF-20, RD-04 |
| R-11 | Tratamiento de datos personales conforme a la LFPDPPP. | RD-05 |

---

# 3. Requerimientos específicos

## 3.1 Requerimientos funcionales

### Convenciones de esta sección

- **Identificadores:** cada criterio de aceptación tiene un ID único `RF-XX-AC-N`. Los endpoints del `openapi.yaml` y las pruebas automatizadas deben referenciarlos. En código los guiones se escriben como guion bajo (`RF_01_AC_1`); el ID canónico es el de este documento.
- **Numeración:** RF-01 a RF-16 conservan la numeración del SRS con mayor cobertura (Raul); RF-17 a RF-27 son requerimientos que aportaron los demás SRS. La equivalencia con los IDs de cada SRS individual está en `diferencias_SRS.md`.
- **Marca de cada criterio:** `[=]` se conserva igual que en el SRS base; `[~]` conserva el ID pero cambió su contenido al resolver un conflicto; `[+]` es nuevo. Un ID nunca se reutiliza para otro tema.
- **Formato:** **Dado que** (precondición), **cuando** (acción), **entonces** (resultado verificable).
- **Respuestas de error de la API:** sin sesión, 401; sin permiso para esa acción, 403; datos faltantes o inválidos, 422; el estado actual no permite la acción, 409.
- **Prioridad:** Alta (necesario para el flujo básico), Media (importante, puede entrar después del flujo básico), Baja (se hace si queda tiempo en las 12 semanas).
- **Casos de uso:** los UC01 a UC25 son los del diagrama actual. Los requerimientos marcados "UC por agregar" todavía no están en el diagrama.

### Estados del servicio

| Estado | Código en la API | Significado | Quién lo asigna | Pasa a |
|---|---|---|---|---|
| solicitado | `REQUESTED` | La solicitud existe y no tiene un técnico con pago retenido. También es el estado al que regresa si el técnico rechaza, cancela o es suspendido. | Sistema | aceptado, cancelado |
| aceptado | `ACCEPTED` | El técnico aceptó y el pago quedó retenido. | Sistema | en camino, solicitado, cancelado |
| en camino | `ON_THE_WAY` | El técnico va hacia el domicilio. | Técnico | en proceso, solicitado |
| en proceso | `IN_PROGRESS` | El técnico trabaja en el servicio. | Técnico | terminado, cerrado |
| terminado | `FINISHED` | El técnico terminó; el pago sigue retenido. | Técnico | confirmado, en revisión |
| en revisión | `IN_REVIEW` | El cliente reportó una inconformidad. | Sistema | confirmado, cerrado, en proceso |
| confirmado | `CONFIRMED` | El cliente confirmó, venció el plazo de 72 horas o un administrador resolvió a favor del técnico; el pago se liberó. Estado final. | Cliente / Sistema / Administrador | — |
| cerrado | `CLOSED` | El servicio terminó sin trabajo completo o con reembolso por inconformidad. Estado final. | Sistema | — |
| cancelado | `CANCELLED` | El servicio se canceló antes de iniciar el trabajo. Estado final. | Sistema | — |

Un costo adicional tiene sus propios estados: pendiente de aprobación, aprobado y rechazado. No es un estado del servicio.

---

### RF-01: Registro, inicio de sesión y roles
El sistema permitirá registrarse e iniciar sesión a clientes y técnicos con su número de teléfono y un código de acceso, sin contraseña; los administradores entrarán con correo, contraseña y un segundo factor. Las funciones se limitarán según el rol.
- **Prioridad:** Alta · **Casos de uso:** UC01

- **RF-01-AC-1** `[~]` **Dado que** un visitante cuyo teléfono no está registrado validó su código de acceso, **cuando** se registra como cliente con nombre completo y dirección, **entonces** se crea una cuenta con rol cliente asociada a ese teléfono.
- **RF-01-AC-2** `[~]` **Dado que** un visitante cuyo teléfono no está registrado validó su código de acceso, **cuando** se registra como técnico con nombre completo, especialidad (uno o más trabajos del catálogo) y años de experiencia, **entonces** se crea una cuenta con rol técnico y estado de validación "sin validar" (RF-03).
- **RF-01-AC-3** `[~]` **Dado que** ya existe una cuenta con un teléfono, **cuando** un visitante intenta registrar otra cuenta con ese mismo teléfono, **entonces** el sistema no crea la cuenta e indica que el teléfono ya está registrado.
- **RF-01-AC-4** `[=]` **Dado que** un visitante usa el registro público, **cuando** envía el rol "administrador", **entonces** el sistema rechaza el registro y no crea ninguna cuenta.
- **RF-01-AC-5** `[~]` **Dado que** un cliente o técnico registrado ingresó su teléfono y recibió un código de acceso por WhatsApp, **cuando** ingresa el código correcto antes de 10 minutos **(P)**, **entonces** el sistema abre la sesión y devuelve su rol, sin pedir contraseña en ningún momento.
- **RF-01-AC-6** `[~]` **Dado que** un usuario o visitante, **cuando** ingresa un código incorrecto o vencido, **entonces** el sistema responde 401 con el mismo mensaje en ambos casos y le permite pedir un código nuevo.
- **RF-01-AC-7** `[=]` **Dado que** un usuario con sesión y rol cliente, **cuando** intenta ejecutar una función exclusiva de técnico o de administrador, **entonces** el sistema responde 403 y no modifica ningún dato.
- **RF-01-AC-8** `[+]` **Dado que** un teléfono acumuló 5 códigos incorrectos seguidos **(P)**, **cuando** se intenta ingresar otro, **entonces** el sistema bloquea el inicio de sesión de ese teléfono durante 15 minutos **(P)** y ofrece el botón "Necesito ayuda" (RF-21).
- **RF-01-AC-9** `[+]` **Dado que** se envió un código de acceso por WhatsApp, **cuando** pasan 2 minutos **(P)** sin confirmación de entrega, **entonces** el sistema envía el mismo código por SMS.
- **RF-01-AC-10** `[+]` **Dado que** un administrador ingresó su correo y contraseña correctos, **cuando** no completa el segundo factor, **entonces** el sistema no abre la sesión.
- **RF-01-AC-11** `[+]` **Dado que** un cliente con sesión, **cuando** edita en su perfil el nombre, la dirección o la edad (dato opcional), **entonces** el sistema guarda los cambios.

### RF-02: Crear solicitud de servicio
El cliente podrá crear una solicitud eligiendo la categoría y el trabajo del catálogo, con descripción, hasta 5 fotos **(P)**, dirección e indicación de urgencia.
- **Prioridad:** Alta · **Casos de uso:** UC02

- **RF-02-AC-1** `[=]` **Dado que** un cliente con sesión, **cuando** envía un trabajo activo del catálogo y una descripción no vacía, **entonces** el sistema crea la solicitud en estado "solicitado", asociada a ese cliente, y aparece en su historial (RF-14).
- **RF-02-AC-2** `[=]` **Dado que** un cliente con sesión, **cuando** envía la solicitud sin trabajo o con la descripción vacía, **entonces** el sistema no crea la solicitud y devuelve un error de validación (422) que nombra el campo faltante.
- **RF-02-AC-3** `[=]` **Dado que** un trabajo desactivado en el catálogo, **cuando** un cliente intenta crear una solicitud con ese trabajo, **entonces** el sistema no la crea y devuelve un error de validación.
- **RF-02-AC-4** `[=]` **Dado que** una solicitud con 5 fotos adjuntas, **cuando** el cliente intenta adjuntar una sexta, **entonces** el sistema la rechaza, conserva las 5 anteriores e indica el límite de 5.
- **RF-02-AC-5** `[=]` **Dado que** un cliente crea una solicitud, **cuando** la marca como urgente, **entonces** la solicitud queda identificada como urgente y el sistema notifica a los técnicos disponibles en máximo 1 minuto (RNF-06).
- **RF-02-AC-6** `[=]` **Dado que** un visitante sin sesión, **cuando** intenta crear una solicitud, **entonces** el sistema responde 401 y no la crea.
- **RF-02-AC-7** `[=]` **Dado que** un cliente que está por adjuntar fotos a una solicitud, **cuando** abre la pantalla de carga de fotos, **entonces** el sistema muestra un aviso que pide fotografiar solo la falla y evitar personas, documentos y objetos de valor **(P)**.
- **RF-02-AC-8** `[+]` **Dado que** un cliente con una dirección registrada, **cuando** crea una solicitud, **entonces** puede usar esa dirección o capturar otra, y la solicitud guarda la que eligió.
- **RF-02-AC-9** `[+]` **Dado que** un cliente eligió un trabajo parametrizable, **cuando** intenta continuar sin capturar una variable obligatoria, **entonces** el sistema nombra el dato faltante y no deja la solicitud lista para contratar.
- **RF-02-AC-10** `[+]` **Dado que** un cliente con sesión, **cuando** entra a solicitar un servicio y elige una categoría, **entonces** ve las categorías con su ícono y, dentro de la elegida, sus trabajos activos y la opción "Otro trabajo sujeto a cotizar" (RF-18).

### RF-03: Validación del técnico
El técnico enviará INE, selfie, comprobante de domicilio, carta de no antecedentes penales y la acreditación de su experiencia; un administrador lo validará, y solo los técnicos validados aparecerán en los resultados.
- **Prioridad:** Alta · **Casos de uso:** UC23, UC24

- **RF-03-AC-1** `[=]` **Dado que** un técnico con estado de validación "sin validar", "en revisión" o "rechazado", **cuando** un cliente consulta técnicos (RF-04), **entonces** ese técnico no aparece en los resultados.
- **RF-03-AC-2** `[~]` **Dado que** un técnico al que le falta cargar alguno de los cuatro documentos, **cuando** intenta enviarlos a revisión, **entonces** el sistema no los envía e indica qué documento falta.
- **RF-03-AC-3** `[~]` **Dado que** un técnico que cargó los cuatro documentos y acreditó su experiencia, **cuando** los envía a revisión, **entonces** su estado de validación cambia a "en revisión" y aparece en la lista de pendientes del administrador.
- **RF-03-AC-4** `[=]` **Dado que** un técnico "en revisión", **cuando** un administrador aprueba sus documentos, **entonces** su estado de validación cambia a "validado" y aparece en los resultados de RF-04 cuando esté disponible.
- **RF-03-AC-5** `[=]` **Dado que** un técnico "en revisión", **cuando** un administrador rechaza sus documentos sin escribir un motivo, **entonces** el sistema no registra el rechazo y exige el motivo.
- **RF-03-AC-6** `[=]` **Dado que** un técnico "en revisión", **cuando** un administrador lo rechaza con un motivo, **entonces** su estado cambia a "rechazado", el técnico recibe el motivo y puede volver a enviar documentos.
- **RF-03-AC-7** `[=]` **Dado que** un usuario que no es administrador, **cuando** intenta aprobar o rechazar documentos, **entonces** el sistema responde 403 y el estado del técnico no cambia.
- **RF-03-AC-8** `[+]` **Dado que** un técnico cargó una certificación de su oficio, **cuando** envía su registro, **entonces** el sistema acepta la experiencia como acreditada sin pedir fotos ni referencias.
- **RF-03-AC-9** `[+]` **Dado que** un técnico sin certificación, **cuando** envía su registro con al menos una foto de un trabajo anterior y dos referencias con nombre y teléfono, **entonces** el sistema acepta la experiencia como acreditada.
- **RF-03-AC-10** `[+]` **Dado que** un técnico sin certificación, **cuando** intenta enviar su registro con menos de dos referencias o sin fotos de trabajos, **entonces** el sistema no lo envía e indica qué falta.
- **RF-03-AC-11** `[+]` **Dado que** existe un técnico "en revisión", **cuando** un administrador abre su solicitud, **entonces** ve todos los documentos, fotos y referencias cargados.

### RF-04: Consulta de técnicos disponibles y perfil
El sistema mostrará al cliente los técnicos disponibles para su solicitud y el perfil público de cada uno, con su portafolio de trabajos.
- **Prioridad:** Alta · **Casos de uso:** UC05

- **RF-04-AC-1** `[~]` **Dado que** una solicitud "solicitado" y al menos un técnico disponible, **cuando** el cliente consulta técnicos, **entonces** cada resultado incluye foto, nombre completo, marca "Verificado", especialidad, años de experiencia, calificación promedio, número de trabajos completados, su cotización para ese trabajo y los 3 comentarios más recientes **(P)**.
- **RF-04-AC-2** `[=]` **Dado que** un técnico disponible con cero trabajos completados, **cuando** aparece en los resultados, **entonces** muestra la etiqueta "Nuevo" en lugar de la calificación promedio.
- **RF-04-AC-3** `[=]` **Dado que** un técnico validado que no ofrece el trabajo de la solicitud, o que tiene otro servicio aceptado en la misma ventana de horario, **cuando** el cliente consulta técnicos, **entonces** ese técnico no aparece en los resultados.
- **RF-04-AC-4** `[~]` **Dado que** ningún técnico está disponible para la solicitud, **cuando** el cliente consulta técnicos, **entonces** el sistema devuelve una lista vacía, muestra un mensaje que lo indica y permite elegir otra ventana de horario o pedir ayuda (RF-21).
- **RF-04-AC-5** `[+]` **Dado que** un técnico validado con información registrada, **cuando** un cliente abre su perfil, **entonces** ve sus especialidades, sus cotizaciones, las fotos de trabajos anteriores, las marcas o materiales que registró, su calificación promedio y sus reseñas.
- **RF-04-AC-6** `[+]` **Dado que** un cliente abre el perfil de un técnico, **cuando** se muestra, **entonces** no aparecen sus documentos, su domicilio, sus referencias ni su teléfono (RNF-05, RNF-20).

### RF-05: Cotización previa
Antes de contratar, el sistema mostrará el precio del técnico, el rango de referencia del trabajo (con el máximo no mayor a 130% del mínimo) y la comisión de la plataforma, cada uno por separado.
- **Prioridad:** Alta · **Casos de uso:** UC06

- **RF-05-AC-1** `[~]` **Dado que** un trabajo con rango de referencia, un técnico con cotización para ese trabajo y una comisión configurada, **cuando** el cliente consulta la cotización previa, **entonces** el sistema muestra el precio del técnico, el texto "normalmente cuesta entre $X y $Y" con el rango, la comisión y el total a pagar, como conceptos separados.
- **RF-05-AC-2** `[=]` **Dado que** cualquier cotización mostrada, **cuando** se compara el máximo del rango con el mínimo, **entonces** el máximo es menor o igual a 1.3 veces el mínimo.
- **RF-05-AC-3** `[=]` **Dado que** un cliente contrató con una cotización mostrada, **cuando** un administrador modifica después el rango del catálogo, **entonces** la cotización guardada en ese servicio conserva los valores originales.
- **RF-05-AC-4** `[+]` **Dado que** un técnico tiene una cotización fuera del rango con justificación (RF-17), **cuando** el cliente la consulta, **entonces** ve un aviso de que el precio está fuera de lo normal junto con la justificación.
- **RF-05-AC-5** `[+]` **Dado que** un administrador modificó el rango de un trabajo, **cuando** un cliente consulta una cotización nueva de ese trabajo, **entonces** ve el rango vigente, no el anterior.
- **RF-05-AC-6** `[+]` **Dado que** un cliente capturó todas las variables de un trabajo parametrizable, **cuando** consulta la cotización previa, **entonces** el precio y el rango corresponden a esas variables y se muestra qué incluye el precio.

### RF-06: Contratación del técnico
El cliente podrá elegir un técnico y confirmar una ventana de horario; el técnico podrá aceptar o rechazar la solicitud. El monto del servicio es la cotización que el cliente vio.
- **Prioridad:** Alta · **Casos de uso:** UC07, UC10

- **RF-06-AC-1** `[=]` **Dado que** una solicitud en estado "solicitado" y un técnico disponible, **cuando** el cliente lo elige y confirma una ventana de horario, **entonces** la solicitud queda asociada a ese técnico, el técnico recibe una notificación y el cliente ve el estado "pendiente de respuesta del técnico".
- **RF-06-AC-2** `[=]` **Dado que** un técnico que dejó de estar disponible en esa ventana de horario, **cuando** el cliente intenta elegirlo, **entonces** el sistema no asocia la solicitud y muestra la lista de técnicos actualizada.
- **RF-06-AC-3** `[~]` **Dado que** una solicitud asociada a un técnico con una cotización Q mostrada al cliente, **cuando** el técnico acepta, **entonces** el sistema fija Q como monto del servicio y pide al cliente autorizar el pago (RF-10).
- **RF-06-AC-4** `[~]` **Dado que** una solicitud asociada a un técnico con una cotización Q mostrada al cliente, **cuando** el técnico intenta aceptar con un monto distinto de Q, **entonces** el sistema no acepta la solicitud e indica que el monto es el cotizado.
- **RF-06-AC-5** `[=]` **Dado que** una solicitud asociada a un técnico, **cuando** el técnico la rechaza, **entonces** la solicitud queda sin técnico en estado "solicitado", el cliente recibe una notificación y se le ofrecen técnicos alternativos.
- **RF-06-AC-6** `[=]` **Dado que** una solicitud asociada a otro técnico, **cuando** un técnico distinto intenta aceptarla o rechazarla, **entonces** el sistema responde 403 y la solicitud no cambia.
- **RF-06-AC-7** `[=]` **Dado que** una solicitud urgente asociada a un técnico que no ha respondido, **cuando** pasan 10 minutos **(P)** desde que se le envió, **entonces** el sistema deja la solicitud sin técnico en estado "solicitado", notifica al cliente y le muestra técnicos alternativos disponibles.
- **RF-06-AC-8** `[+]` **Dado que** el cliente autorizó el pago de una cotización, **cuando** el servicio pasa a "aceptado", **entonces** el sistema guarda la fecha, el alcance, el precio y la evidencia asociada como acuerdo inicial, que no se puede modificar después (RD-11).

### RF-07: Seguimiento por estados
El servicio avanzará por los estados de la tabla "Estados del servicio". El técnico actualiza de "aceptado" a "terminado"; el cliente confirma, y tanto el cliente como el técnico ven el estado en todo momento.
- **Prioridad:** Alta · **Casos de uso:** UC11, UC14

- **RF-07-AC-1** `[=]` **Dado que** un servicio en estado "aceptado", "en camino" o "en proceso" y el técnico asignado, **cuando** el técnico lo cambia al estado inmediato siguiente (en camino, en proceso o terminado), **entonces** el servicio queda en ese estado.
- **RF-07-AC-2** `[=]` **Dado que** un servicio en estado "aceptado", **cuando** el técnico intenta cambiarlo a "terminado" (saltar un estado) o cambiar un servicio "en proceso" a "en camino" (retroceder), **entonces** el sistema rechaza el cambio y el estado no se modifica.
- **RF-07-AC-3** `[=]` **Dado que** un servicio en estado "terminado", **cuando** el técnico intenta cambiarlo a "confirmado", **entonces** el sistema responde 403 y el estado sigue en "terminado".
- **RF-07-AC-4** `[=]` **Dado que** un servicio asignado a un técnico, **cuando** otro técnico intenta cambiar su estado, **entonces** el sistema responde 403 y el estado no cambia.
- **RF-07-AC-5** `[=]` **Dado que** cualquier cambio de estado de un servicio, **cuando** se realiza, **entonces** el sistema guarda el estado anterior, el nuevo, la fecha y hora y el usuario que lo realizó.
- **RF-07-AC-6** `[=]` **Dado que** un cliente con un servicio propio, **cuando** consulta ese servicio, **entonces** ve el estado actual y la lista de cambios de estado con fecha y hora.
- **RF-07-AC-7** `[=]` **Dado que** un servicio de otro cliente, **cuando** un cliente intenta consultarlo, **entonces** el sistema responde 403 y no muestra datos del servicio.

### RF-08: Notificaciones
El sistema notificará cada cambio de estado y cada evento importante del servicio por WhatsApp y en la aplicación, con respaldo por SMS.
- **Prioridad:** Alta · **Casos de uso:** UC12

- **RF-08-AC-1** `[=]` **Dado que** un servicio cambia de estado, **cuando** el cambio queda registrado, **entonces** el sistema crea una notificación en la aplicación y envía un mensaje de WhatsApp al teléfono del cliente (o al del contacto para avisos, RF-15-AC-3).
- **RF-08-AC-2** `[=]` **Dado que** se envió un mensaje de WhatsApp, **cuando** pasan 2 minutos **(P)** sin confirmación de entrega, **entonces** el sistema envía un SMS con el mismo contenido al mismo teléfono.
- **RF-08-AC-3** `[=]` **Dado que** se envió un mensaje de WhatsApp, **cuando** la entrega se confirma antes de 2 minutos, **entonces** el sistema no envía SMS.
- **RF-08-AC-4** `[=]` **Dado que** cualquier notificación de cambio de estado, **cuando** se envía, **entonces** el mensaje incluye el identificador del servicio, el trabajo, el nuevo estado y la fecha y hora del cambio.
- **RF-08-AC-5** `[+]` **Dado que** ocurre uno de estos eventos: costo adicional por aprobar, recordatorio de confirmación, pago liberado, inconformidad abierta o resolución de inconformidad, **cuando** el evento queda registrado, **entonces** el cliente y el técnico reciben el aviso correspondiente por los mismos canales en menos de 1 minuto (RNF-06).
- **RF-08-AC-6** `[+]` **Dado que** cualquier aviso de un servicio en estado "aceptado" o posterior, **cuando** se envía por WhatsApp o SMS, **entonces** incluye el teléfono de respaldo de FairFix (RNF-16).

### RF-09: Costo adicional
Para un costo adicional, el técnico enviará el monto, una descripción y de una a cinco fotos; el cliente lo aprobará o rechazará desde el celular. Sin aprobación explícita no se cobra.
- **Prioridad:** Alta · **Casos de uso:** UC13, UC15

- **RF-09-AC-1** `[~]` **Dado que** un servicio en estado "en proceso" y el técnico asignado, **cuando** envía un costo adicional con monto mayor a cero, descripción y de 1 a 5 fotos **(P)**, **entonces** el sistema lo registra como "pendiente de aprobación" y notifica al cliente.
- **RF-09-AC-2** `[~]` **Dado que** un servicio en estado "en proceso", **cuando** el técnico intenta enviar un costo adicional sin foto, sin descripción o con monto menor o igual a cero, **entonces** el sistema no lo registra y devuelve un error de validación (422).
- **RF-09-AC-3** `[~]` **Dado que** un costo adicional "pendiente de aprobación", **cuando** el cliente lo aprueba, **entonces** el pago retenido aumenta en ese monto con el mismo método de pago (con transferencia u OXXO se genera una referencia y esa parte se retiene al recibirla), se notifica al técnico y el monto se libera junto con el resto al confirmar el servicio.
- **RF-09-AC-4** `[~]` **Dado que** un costo adicional "pendiente de aprobación", **cuando** el cliente lo rechaza, **entonces** el total del servicio se mantiene sin ese monto, el servicio sigue "en proceso" y se notifica al técnico que debe terminar lo acordado y dejar la instalación funcionando o segura (RD-09).
- **RF-09-AC-5** `[~]` **Dado que** un costo adicional sigue "pendiente de aprobación", **cuando** el técnico marca el servicio como "terminado", **entonces** el sistema lo registra como rechazado, lo excluye del total y lo notifica al cliente y al técnico.
- **RF-09-AC-6** `[+]` **Dado que** el cliente rechazó un costo adicional, **cuando** el técnico declara que el trabajo acordado no puede completarse sin ese adicional, **entonces** el servicio pasa a "cerrado", se libera al técnico solo el costo de visita del catálogo y el resto del pago retenido, incluida la comisión, se reembolsa al cliente **(P)**.
- **RF-09-AC-7** `[+]` **Dado que** el técnico registró un costo adicional, **cuando** el cliente abre la notificación, **entonces** ve las fotos, la descripción, el monto adicional, el total actualizado, las opciones "Aceptar", "Rechazar" y "Tengo dudas" (RF-21) y el aviso "si no respondes, este costo no se cobra".
- **RF-09-AC-8** `[+]` **Dado que** un servicio en un estado distinto de "en proceso", **cuando** el técnico intenta registrar un costo adicional, **entonces** el sistema lo rechaza (409).
- **RF-09-AC-9** `[+]` **Dado que** el técnico registra un trabajo extra e indica que corrige un error propio, **cuando** lo envía, **entonces** el sistema lo guarda sin monto y no lo presenta al cliente como costo cobrable (RD-10).
- **RF-09-AC-10** `[+]` **Dado que** un servicio con costos adicionales rechazados o sin respuesta, **cuando** se calcula el monto a liberar, **entonces** el total solo incluye el monto del acuerdo inicial y los costos adicionales aprobados.
- **RF-09-AC-11** `[+]` **Dado que** un costo adicional "pendiente de aprobación", **cuando** un usuario distinto del cliente dueño del servicio intenta aprobarlo o rechazarlo, **entonces** el sistema responde 403 y el costo sigue pendiente.

### RF-10: Pago retenido
El pago (tarjeta, transferencia u OXXO) quedará retenido al aceptar el técnico y solo se liberará cuando el cliente confirme el trabajo, venza el plazo de RF-20 o un administrador resuelva una inconformidad.
- **Prioridad:** Alta · **Casos de uso:** UC08, UC16, UC17

- **RF-10-AC-1** `[~]` **Dado que** un técnico aceptó una solicitud con monto M y una comisión C, **cuando** el cliente autoriza el pago con tarjeta, transferencia u OXXO y la pasarela confirma la retención, **entonces** el sistema registra un pago retenido de M + C y el servicio pasa a "aceptado".
- **RF-10-AC-2** `[=]` **Dado que** un técnico aceptó una solicitud, **cuando** la pasarela rechaza o no completa la retención, **entonces** el servicio sigue en "solicitado", no se registra ningún cobro y el cliente recibe un aviso del error para que cambie su método de pago.
- **RF-10-AC-3** `[=]` **Dado que** un servicio en estado "terminado" sin inconformidad, **cuando** el cliente lo confirma, **entonces** el servicio pasa a "confirmado", se libera al técnico el monto M más los costos adicionales aprobados, y la comisión C queda para la plataforma.
- **RF-10-AC-4** `[~]` **Dado que** un servicio en estado "terminado", **cuando** el cliente no lo confirma ni reporta inconformidad y no han pasado 72 horas, **entonces** el pago sigue retenido y no se registra ninguna liberación.
- **RF-10-AC-5** `[~]` **Dado que** una solicitud a contratar, **cuando** se intenta elegir un método de pago distinto de tarjeta, transferencia u OXXO (por ejemplo, efectivo al técnico), **entonces** el sistema rechaza el método y no registra ninguna retención.
- **RF-10-AC-6** `[=]` **Dado que** un servicio "terminado" de otro cliente, **cuando** un usuario distinto del cliente dueño intenta confirmarlo, **entonces** el sistema responde 403 y no libera el pago.
- **RF-10-AC-7** `[+]` **Dado que** el cliente eligió transferencia u OXXO, **cuando** la pasarela todavía no reporta el pago como recibido, **entonces** el servicio sigue en "solicitado" y el cliente ve la referencia de pago pendiente.

### RF-11: Inconformidad
El cliente podrá reportar inconformidad con fotos antes de confirmar; el pago seguirá retenido y un administrador resolverá el caso con una de cuatro resoluciones.
- **Prioridad:** Alta · **Casos de uso:** UC20, UC21

- **RF-11-AC-1** `[=]` **Dado que** un servicio en estado "terminado", **cuando** el cliente reporta una inconformidad con descripción y al menos una foto, **entonces** el servicio pasa a "en revisión" y aparece en la lista de casos pendientes del administrador.
- **RF-11-AC-2** `[=]` **Dado que** un servicio en estado "terminado", **cuando** el cliente intenta reportar una inconformidad sin foto o sin descripción, **entonces** el sistema no registra el reporte y devuelve un error de validación.
- **RF-11-AC-3** `[=]` **Dado que** un servicio en estado distinto de "terminado" (por ejemplo, "confirmado"), **cuando** el cliente intenta reportar una inconformidad, **entonces** el sistema rechaza el reporte, el servicio no cambia y se le ofrece el botón "Necesito ayuda".
- **RF-11-AC-4** `[=]` **Dado que** un servicio "en revisión", **cuando** el cliente intenta confirmarlo, **entonces** el sistema lo rechaza y el pago sigue retenido.
- **RF-11-AC-5** `[=]` **Dado que** un servicio "en revisión", **cuando** un administrador resuelve liberar el pago al técnico, **entonces** el servicio pasa a "confirmado" y el pago se libera al técnico como en RF-10-AC-3.
- **RF-11-AC-6** `[=]` **Dado que** un servicio "en revisión", **cuando** un administrador resuelve un reembolso total, **entonces** el servicio pasa a "cerrado" y el pago retenido se reembolsa completo al cliente.
- **RF-11-AC-7** `[~]` **Dado que** un servicio "en revisión", **cuando** un administrador resuelve un reembolso parcial indicando un monto para el cliente mayor que cero y menor que el pago retenido, **entonces** el servicio pasa a "cerrado", ese monto se reembolsa al cliente y la diferencia se libera al técnico.
- **RF-11-AC-8** `[=]` **Dado que** un administrador registra la resolución de un caso, **cuando** la resolución queda guardada, **entonces** el cliente y el técnico reciben una notificación con el resultado.
- **RF-11-AC-9** `[=]` **Dado que** un usuario que no es administrador, **cuando** intenta resolver un caso "en revisión", **entonces** el sistema responde 403 y el caso no cambia.
- **RF-11-AC-10** `[+]` **Dado que** un servicio "en revisión", **cuando** un administrador resuelve una nueva visita del técnico sin costo, **entonces** el servicio regresa a "en proceso" con una nueva ventana de horario, el pago sigue retenido y el técnico vuelve a ver la dirección (RNF-03).
- **RF-11-AC-11** `[+]` **Dado que** un servicio "en revisión", **cuando** un administrador intenta cerrar el caso sin elegir una resolución, **entonces** el sistema exige elegir una de las cuatro: liberar al técnico, nueva visita sin costo, reembolso parcial o reembolso total.

### RF-12: Retraso, cancelación o inasistencia del técnico
Si el técnico se retrasa, el sistema avisará al cliente; si cancela o no llega, le ofrecerá además técnicos alternativos sin repetir la solicitud; las cancelaciones afectarán la calificación del técnico.
- **Prioridad:** Media · **Casos de uso:** UC09

- **RF-12-AC-1** `[=]` **Dado que** un servicio en estado "aceptado" o "en camino" con pago retenido, **cuando** el técnico lo cancela, **entonces** el servicio regresa a "solicitado" sin técnico, el pago retenido se reembolsa completo **(P)** y el cliente recibe un aviso con la lista de técnicos alternativos disponibles.
- **RF-12-AC-2** `[=]` **Dado que** un servicio "aceptado" cuya ventana de horario ya comenzó, **cuando** pasan 30 minutos **(P)** desde el inicio de la ventana sin que el estado sea "en camino" o posterior, **entonces** el sistema lo trata como inasistencia con el mismo resultado que RF-12-AC-1.
- **RF-12-AC-3** `[=]` **Dado que** un servicio que regresó a "solicitado" por cancelación o inasistencia, **cuando** el cliente elige un técnico alternativo, **entonces** la nueva solicitud conserva el trabajo, la descripción y las fotos originales sin que el cliente los capture de nuevo.
- **RF-12-AC-4** `[=]` **Dado que** un servicio que regresó a "solicitado" por cancelación o inasistencia, **cuando** el cliente decide no elegir un técnico alternativo, **entonces** el servicio pasa a "cancelado" y no se registra ningún cobro.
- **RF-12-AC-5** `[=]` **Dado que** un técnico cancela o no llega, **cuando** se registra el evento, **entonces** el sistema guarda un registro con el técnico, el servicio, el tipo de evento (cancelación o inasistencia) y la fecha y hora; el efecto en la calificación queda pendiente (A-6).
- **RF-12-AC-6** `[=]` **Dado que** un servicio "aceptado" cuya ventana de horario ya comenzó, **cuando** pasan 10 minutos **(P)** desde el inicio de la ventana sin que el estado sea "en camino" o posterior, **entonces** el sistema notifica al cliente y al técnico, por los canales de RF-08, que el servicio lleva retraso.

### RF-13: Recibo y calificación
Al confirmar el servicio, el sistema generará un recibo digital desglosado y permitirá al cliente calificar y comentar al técnico.
- **Prioridad:** Alta · **Casos de uso:** UC18, UC19

- **RF-13-AC-1** `[~]` **Dado que** un servicio pasa a "confirmado", **cuando** se registra el cambio, **entonces** el sistema genera un recibo con folio único, fecha, cliente, técnico, trabajo, monto del acuerdo inicial, costos adicionales aprobados, comisión, descuento por promoción si lo hubo, total pagado y método de pago.
- **RF-13-AC-2** `[=]` **Dado que** un servicio "confirmado" que el cliente aún no calificó, **cuando** el cliente envía una calificación entera de 1 a 5, con o sin comentario de hasta 500 caracteres **(P)**, **entonces** el sistema guarda la calificación asociada al servicio y al técnico.
- **RF-13-AC-3** `[=]` **Dado que** un servicio "confirmado", **cuando** el cliente envía una calificación fuera del rango de 1 a 5 o un comentario de más de 500 caracteres, **entonces** el sistema no la guarda y devuelve un error de validación.
- **RF-13-AC-4** `[=]` **Dado que** un servicio que el cliente ya calificó, **cuando** intenta calificarlo otra vez, **entonces** el sistema rechaza la segunda calificación y conserva la primera.
- **RF-13-AC-5** `[=]` **Dado que** un servicio que no está "confirmado", o que pertenece a otro cliente, **cuando** un cliente intenta calificarlo, **entonces** el sistema rechaza la calificación.
- **RF-13-AC-6** `[~]` **Dado que** un técnico con calificaciones guardadas, **cuando** se guarda una nueva calificación, **entonces** su calificación promedio pasa a ser la media aritmética de todas sus calificaciones, incluida la nueva, y su perfil la muestra en menos de 5 segundos.
- **RF-13-AC-7** `[+]` **Dado que** un servicio con monto del acuerdo inicial M, costos adicionales aprobados por A, comisión C y descuento D, **cuando** se consulta su recibo, **entonces** el total es exactamente M + A + C − D y coincide con lo cobrado al cliente.

### RF-14: Historial
El cliente podrá consultar su historial de servicios y descargar los recibos.
- **Prioridad:** Media · **Casos de uso:** UC22

- **RF-14-AC-1** `[=]` **Dado que** un cliente con sesión y servicios registrados, **cuando** consulta su historial, **entonces** ve la lista de sus propios servicios, cada uno con trabajo, fecha, técnico, estado y monto.
- **RF-14-AC-2** `[=]` **Dado que** un cliente con sesión y sin servicios, **cuando** consulta su historial, **entonces** el sistema devuelve una lista vacía y muestra un mensaje que lo indica.
- **RF-14-AC-3** `[=]` **Dado que** un servicio "confirmado" del cliente, **cuando** el cliente descarga su recibo, **entonces** obtiene un archivo PDF con los mismos campos definidos en RF-13-AC-1.
- **RF-14-AC-4** `[=]` **Dado que** un servicio que no está "confirmado", **cuando** el cliente intenta descargar su recibo, **entonces** el sistema informa que el recibo no está disponible y no genera archivo.
- **RF-14-AC-5** `[=]` **Dado que** servicios y recibos de otro cliente, **cuando** un cliente intenta consultarlos o descargarlos, **entonces** el sistema responde 403 y no expone ningún dato.

### RF-15: Modo simplificado
El sistema tendrá un modo simplificado activable, con solicitud en máximo 3 pantallas, la opción de que un familiar solicite a nombre de otra persona indicando un contacto para los avisos, y un resumen en una sola pantalla antes del cobro.
- **Prioridad:** Alta · **Casos de uso:** UC03, UC04

- **RF-15-AC-1** `[=]` **Dado que** un cliente con sesión, **cuando** activa el modo simplificado, cierra sesión y vuelve a iniciarla, **entonces** el modo simplificado sigue activo.
- **RF-15-AC-2** `[=]` **Dado que** un cliente en modo simplificado, **cuando** crea una solicitud desde la pantalla de inicio hasta el envío, **entonces** recorre como máximo 3 pantallas, sin contar el inicio de sesión.
- **RF-15-AC-3** `[=]` **Dado que** un cliente con sesión, **cuando** crea una solicitud a nombre de otra persona indicando nombre y teléfono del contacto, **entonces** la solicitud guarda ese contacto y las notificaciones de RF-08 se envían a ese teléfono; el contacto no necesita una cuenta.
- **RF-15-AC-4** `[=]` **Dado que** un cliente que crea una solicitud a nombre de otra persona, **cuando** omite el nombre o el teléfono del contacto, **entonces** el sistema no crea la solicitud y devuelve un error de validación que nombra el campo faltante.
- **RF-15-AC-5** `[~]` **Dado que** un cliente en modo simplificado, **cuando** ve cualquier pantalla, **entonces** aparece el botón "Necesito ayuda" (RF-21) con la opción de llamar al número de soporte configurado.
- **RF-15-AC-6** `[+]` **Dado que** un cliente en modo simplificado está por autorizar un pago, **cuando** llega al último paso, **entonces** el sistema muestra en una sola pantalla el trabajo, el técnico, la ventana de horario y el total, y no genera el cobro hasta que el cliente confirma.

### RF-16: Catálogo de servicios y precios de referencia
El administrador podrá gestionar el catálogo de categorías y trabajos, con su rango de precio de referencia (mínimo y máximo), su costo de visita y, si aplica, sus variables.
- **Prioridad:** Alta · **Casos de uso:** UC25

- **RF-16-AC-1** `[=]` **Dado que** un administrador con sesión, **cuando** crea un trabajo con nombre, mínimo mayor a cero, máximo menor o igual a 1.3 veces el mínimo y costo de visita mayor o igual a cero, **entonces** el trabajo queda activo y disponible para crear solicitudes (RF-02).
- **RF-16-AC-2** `[=]` **Dado que** un administrador con sesión, **cuando** captura un rango cuyo máximo es mayor a 1.3 veces el mínimo, o un mínimo menor o igual a cero, **entonces** el sistema no lo guarda y devuelve un error de validación.
- **RF-16-AC-3** `[=]` **Dado que** un trabajo activo con solicitudes existentes, **cuando** el administrador lo desactiva, **entonces** ya no se ofrece en solicitudes nuevas y las solicitudes existentes conservan su trabajo.
- **RF-16-AC-4** `[=]` **Dado que** un trabajo con servicios ya contratados, **cuando** el administrador cambia su rango o su costo de visita, **entonces** los servicios ya contratados conservan los valores con los que se contrataron (RF-05-AC-3).
- **RF-16-AC-5** `[=]` **Dado que** un usuario que no es administrador, **cuando** intenta crear, editar o desactivar un trabajo, **entonces** el sistema responde 403 y el catálogo no cambia.
- **RF-16-AC-6** `[+]` **Dado que** un administrador con sesión, **cuando** define en un trabajo una o más variables obligatorias (por ejemplo, cantidad o capacidad) y lo que incluye el precio, **entonces** el trabajo queda como parametrizable y esas variables se piden al crear la solicitud (RF-02-AC-9).

### RF-17: Oferta del técnico
El técnico validado podrá registrar su disponibilidad, elegir qué trabajos del catálogo ofrece y registrar su cotización para cada uno, justificando los precios fuera del rango.
- **Prioridad:** Alta · **Casos de uso:** UC por agregar

- **RF-17-AC-1** `[+]` **Dado que** un técnico validado, **cuando** registra los días y horarios en que trabaja, **entonces** solo aparece en RF-04 para ventanas de horario dentro de esa disponibilidad.
- **RF-17-AC-2** `[+]` **Dado que** un técnico validado, **cuando** elige un trabajo del catálogo y captura un precio dentro del rango, **entonces** el sistema guarda la cotización sin pedir justificación y el técnico aparece para ese trabajo en RF-04.
- **RF-17-AC-3** `[+]` **Dado que** un técnico captura un precio fuera del rango, **cuando** intenta guardar la cotización sin justificación, **entonces** el sistema no la guarda y le pide explicar el motivo.
- **RF-17-AC-4** `[+]` **Dado que** un técnico captura un precio fuera del rango con una justificación, **cuando** guarda la cotización, **entonces** el sistema la guarda y la marca como fuera de rango (RF-05-AC-4).
- **RF-17-AC-5** `[+]` **Dado que** un técnico cambia una cotización, **cuando** la guarda, **entonces** el nuevo precio solo aplica a solicitudes creadas después del cambio.
- **RF-17-AC-6** `[+]` **Dado que** un técnico que no está validado, **cuando** intenta registrar disponibilidad o cotizaciones, **entonces** el sistema responde 403.

### RF-18: Trabajos especiales y visita de diagnóstico
Cuando el trabajo no está en el catálogo, el cliente podrá pedir una cotización particular; el técnico podrá cotizar, pedir más información o indicar que requiere una visita de diagnóstico.
- **Prioridad:** Media · **Casos de uso:** UC por agregar

- **RF-18-AC-1** `[+]` **Dado que** un cliente eligió "Otro trabajo sujeto a cotizar", **cuando** envía la descripción del problema y el resultado esperado, con o sin fotos y medidas, **entonces** el sistema crea una solicitud especial en estado "solicitado" y conserva la evidencia adjunta.
- **RF-18-AC-2** `[+]` **Dado que** un técnico revisa una solicitud especial y la información no le alcanza, **cuando** pide datos adicionales al cliente, **entonces** el sistema registra la petición, notifica al cliente y la solicitud sigue sin cotización hasta que haya respuesta o se defina una visita.
- **RF-18-AC-3** `[+]` **Dado que** un técnico revisa una solicitud especial, **cuando** la marca como "requiere visita de diagnóstico", **entonces** el cliente ve que aún no hay cotización definitiva, que se requiere una evaluación presencial y cuál es el costo de la visita **(P)**, y debe aceptarla para que se agende.
- **RF-18-AC-4** `[+]` **Dado que** un técnico tiene información suficiente, **cuando** registra alcance, precio y condiciones y envía la cotización, **entonces** el cliente la ve con las opciones de aceptarla o rechazarla y con la indicación de que no tiene rango de referencia.
- **RF-18-AC-5** `[+]` **Dado que** el cliente ve una cotización de un trabajo especial, **cuando** la acepta, **entonces** el sistema continúa con la autorización del pago (RF-10) y guarda el acuerdo inicial (RF-06-AC-8).
- **RF-18-AC-6** `[+]` **Dado que** el cliente ve una cotización de un trabajo especial, **cuando** la rechaza, **entonces** la solicitud queda sin técnico en estado "solicitado", no se registra ningún cobro y puede pedir cotización a otro técnico.
- **RF-18-AC-7** `[+]` **Dado que** una solicitud especial sin cotización aceptada, **cuando** el técnico intenta cambiar su estado a "en camino" o posterior, **entonces** el sistema lo rechaza (409).

### RF-19: Cancelación por el cliente
El cliente podrá cancelar un servicio antes de que el técnico vaya en camino.
- **Prioridad:** Media · **Casos de uso:** UC por agregar

- **RF-19-AC-1** `[+]` **Dado que** un servicio en estado "solicitado" o "aceptado", **cuando** el cliente lo cancela, **entonces** el servicio pasa a "cancelado", el pago retenido se reembolsa completo **(P)** y se notifica al técnico.
- **RF-19-AC-2** `[+]` **Dado que** un servicio en estado "en camino" o posterior, **cuando** el cliente intenta cancelarlo, **entonces** el sistema no lo permite desde la aplicación (409) y ofrece el botón "Necesito ayuda".
- **RF-19-AC-3** `[+]` **Dado que** un servicio de otro cliente, **cuando** un usuario distinto del cliente dueño intenta cancelarlo por esta vía, **entonces** el sistema responde 403 y el servicio no cambia.

### RF-20: Recordatorio y liberación automática
El sistema recordará al cliente que confirme el trabajo a las 48 horas y liberará el pago al técnico a las 72 horas sin respuesta.
- **Prioridad:** Media · **Casos de uso:** UC por agregar

- **RF-20-AC-1** `[+]` **Dado que** un servicio pasó a "terminado", **cuando** pasan 48 horas sin que el cliente confirme ni reporte inconformidad, **entonces** el sistema envía un recordatorio por los canales de RF-08 que indica la fecha y hora en que se liberará el pago.
- **RF-20-AC-2** `[+]` **Dado que** un servicio pasó a "terminado", **cuando** pasan 72 horas sin que el cliente confirme ni reporte inconformidad, **entonces** el sistema lo pasa a "confirmado", libera el pago al técnico como en RF-10-AC-3 y genera el recibo.
- **RF-20-AC-3** `[+]` **Dado que** el cliente reportó una inconformidad antes de las 72 horas, **cuando** se cumple el plazo, **entonces** el pago no se libera.
- **RF-20-AC-4** `[+]` **Dado que** un servicio en estado "terminado", **cuando** el cliente lo consulta o recibe el aviso de trabajo terminado, **entonces** ve la fecha y hora de liberación con el texto "si no respondes, el pago se libera el [fecha]".

### RF-21: Ayuda y canal de dudas con mediación
Todas las pantallas tendrán un botón "Necesito ayuda" que comunica con una persona, y la opción "Tengo dudas" de un costo adicional abrirá una conversación entre cliente, técnico y un administrador.
- **Prioridad:** Media · **Casos de uso:** UC04 (botón de ayuda); UC por agregar (canal de dudas)

- **RF-21-AC-1** `[+]` **Dado que** un usuario en cualquier pantalla, en modo normal o simplificado, **cuando** la pantalla carga, **entonces** el botón "Necesito ayuda" está visible.
- **RF-21-AC-2** `[+]` **Dado que** un usuario pulsó "Necesito ayuda" dentro del horario de atención, **cuando** elige llamada o WhatsApp, **entonces** el sistema lo comunica con una persona de FairFix por ese canal.
- **RF-21-AC-3** `[+]` **Dado que** un usuario pulsó "Necesito ayuda" fuera del horario de atención, **cuando** se abre la ayuda, **entonces** el sistema muestra el horario y permite dejar un mensaje por WhatsApp.
- **RF-21-AC-4** `[+]` **Dado que** el cliente está viendo un costo adicional, **cuando** pulsa "Tengo dudas", **entonces** se abre una conversación entre el cliente, el técnico y un administrador.
- **RF-21-AC-5** `[+]` **Dado que** hay una conversación de dudas abierta, **cuando** el cliente todavía no aprueba ni rechaza, **entonces** el costo adicional sigue "pendiente de aprobación" y el cliente puede aprobarlo o rechazarlo desde la conversación.
- **RF-21-AC-6** `[+]` **Dado que** el cliente pulsa "Tengo dudas" fuera del horario de atención, **cuando** se abre la conversación, **entonces** el sistema informa que la mediación está disponible de 8:00 a 20:00 y mantiene la conversación con el técnico.
- **RF-21-AC-7** `[+]` **Dado que** una conversación de dudas, **cuando** el cliente o el técnico la consultan, **entonces** no ven el teléfono de la otra parte (RNF-20).

### RF-22: Solicitud asistida por teléfono
Un administrador podrá crear una solicitud de servicio a nombre de un cliente que llamó a FairFix.
- **Prioridad:** Baja · **Casos de uso:** UC por agregar

- **RF-22-AC-1** `[+]` **Dado que** un cliente llamó a FairFix, **cuando** un administrador captura el teléfono del cliente, el trabajo, la descripción, la ventana de horario y el técnico, **entonces** el sistema registra la solicitud a nombre de ese cliente en estado "solicitado".
- **RF-22-AC-2** `[+]` **Dado que** un administrador registró una solicitud por teléfono, **cuando** se guarda, **entonces** el cliente recibe la confirmación por WhatsApp y el pago se autoriza como en RF-10.
- **RF-22-AC-3** `[+]` **Dado que** un usuario que no es administrador, **cuando** intenta crear una solicitud a nombre de otra cuenta por esta vía, **entonces** el sistema responde 403.

### RF-23: Suspensión y reactivación de técnicos
Un administrador podrá suspender y reactivar técnicos, registrando el motivo.
- **Prioridad:** Media · **Casos de uso:** UC por agregar

- **RF-23-AC-1** `[+]` **Dado que** un técnico validado, **cuando** un administrador lo suspende con un motivo, **entonces** el técnico deja de aparecer en RF-04 y recibe el motivo.
- **RF-23-AC-2** `[+]` **Dado que** un administrador intenta suspender a un técnico, **cuando** no escribe un motivo, **entonces** el sistema no registra la suspensión y exige el motivo.
- **RF-23-AC-3** `[+]` **Dado que** un técnico suspendido tiene servicios en estado "aceptado", **cuando** se registra la suspensión, **entonces** esos servicios regresan a "solicitado" con reembolso completo y aviso de técnicos alternativos, como en RF-12-AC-1.
- **RF-23-AC-4** `[+]` **Dado que** un técnico suspendido, **cuando** un administrador lo reactiva, **entonces** vuelve a aparecer en RF-04.

### RF-24: Pagos del técnico
El técnico podrá consultar sus pagos retenidos, liberados y en revisión.
- **Prioridad:** Media · **Casos de uso:** UC por agregar

- **RF-24-AC-1** `[+]` **Dado que** un técnico con servicios cobrados, **cuando** consulta sus pagos, **entonces** ve por servicio el monto, el estado del pago (retenido, liberado o en revisión) y la fecha estimada o real de liberación.
- **RF-24-AC-2** `[+]` **Dado que** pagos de otro técnico, **cuando** un técnico intenta consultarlos, **entonces** el sistema responde 403 y no expone ningún dato.

### RF-25: Garantía posterior al servicio
El cliente podrá reportar, dentro de los 7 días naturales posteriores a la confirmación **(P)**, que la falla reparada volvió a presentarse.
- **Prioridad:** Baja · **Casos de uso:** UC por agregar

- **RF-25-AC-1** `[+]` **Dado que** un servicio "confirmado" hace 7 días naturales o menos, **cuando** el cliente reporta que la falla volvió, con descripción y al menos una foto, **entonces** el sistema crea un caso de garantía, lo agrega a los pendientes del administrador y notifica al técnico.
- **RF-25-AC-2** `[+]` **Dado que** un servicio "confirmado" hace más de 7 días naturales, **cuando** el cliente intenta reportar garantía, **entonces** el sistema rechaza el reporte y ofrece el botón "Necesito ayuda".
- **RF-25-AC-3** `[+]` **Dado que** un caso de garantía pendiente, **cuando** un administrador lo resuelve como "revisión sin costo de mano de obra" o como "no procede" con un motivo, **entonces** el cliente y el técnico reciben la resolución y, si procede, se agenda la visita sin generar un cobro de mano de obra.

### RF-26: Promociones
El administrador podrá crear promociones con descuento y vigencia.
- **Prioridad:** Baja · **Casos de uso:** UC por agregar

- **RF-26-AC-1** `[+]` **Dado que** un administrador creó una promoción con descuento y vigencia, **cuando** un cliente autoriza el pago de un servicio dentro de la vigencia, **entonces** el descuento aparece en el total que ve y paga.
- **RF-26-AC-2** `[+]` **Dado que** una promoción vencida, **cuando** un cliente autoriza el pago de un servicio, **entonces** el descuento no se aplica.
- **RF-26-AC-3** `[+]` **Dado que** un cliente pagó un servicio con promoción, **cuando** se libera el pago, **entonces** el técnico recibe el monto completo de su cotización; el descuento lo absorbe FairFix **(P)**.

### RF-27: Indicadores de éxito
El administrador podrá consultar los indicadores de éxito de la plataforma por periodo.
- **Prioridad:** Baja · **Casos de uso:** UC por agregar

- **RF-27-AC-1** `[+]` **Dado que** un administrador en el módulo de indicadores, **cuando** elige un periodo, **entonces** el sistema muestra los seis indicadores de la tabla siguiente calculados para ese periodo.
- **RF-27-AC-2** `[+]` **Dado que** un conjunto de servicios de prueba con resultados conocidos, **cuando** se calculan los indicadores, **entonces** cada valor coincide con el calculado a mano con las mismas fórmulas.

| Indicador | Fórmula (en el periodo elegido) |
|---|---|
| Tasa de recontratación | Clientes con 2 o más servicios confirmados ÷ clientes con al menos 1 servicio confirmado |
| Costos adicionales rechazados o en revisión | (Costos adicionales rechazados + servicios que pasaron a "en revisión") ÷ servicios confirmados o cerrados |
| Diferencia contra el rango de referencia | Promedio de (total pagado − máximo del rango) ÷ máximo del rango, solo en servicios cuyo total superó el rango |
| Contrataciones sin soporte | Servicios confirmados en los que el cliente no usó "Necesito ayuda" ni RF-22 ÷ servicios confirmados. Se reporta también solo para clientes de 60 años o más que registraron su edad |
| Permanencia de técnicos | Técnicos validados con al menos un servicio en el periodo ÷ técnicos validados al inicio del periodo |
| Tiempo de pago a técnicos | Promedio de horas entre "terminado" y la liberación del pago |

## 3.2 Requerimientos no funcionales

| ID | Descripción | Categoría | Verificación (método y criterio de aprobación) |
|---|---|---|---|
| RNF-01 | En el modo simplificado el texto será de al menos 18 pt (24 px CSS), los botones de al menos 48×48 px CSS con 8 px de separación, habrá una sola acción principal por pantalla y el lenguaje no tendrá tecnicismos (por ejemplo, "pago en custodia" o "incidencia"). | Usabilidad | Inspección con herramientas del navegador de todas las pantallas del modo simplificado y revisión de textos contra una lista de términos prohibidos; se aprueba con 100% de cumplimiento |
| RNF-02 | Un técnico sin experiencia digital podrá actualizar el estado de un servicio en máximo 2 pasos **(P)**. | Usabilidad | Prueba con al menos 3 personas sin experiencia digital; se aprueba si todas lo logran en 2 toques o menos |
| RNF-03 | La dirección exacta del cliente solo será visible para el técnico elegido, desde que el servicio está "aceptado" hasta que pasa a "terminado", y durante una nueva visita ordenada por inconformidad o garantía. | Seguridad / privacidad | Prueba de acceso a la dirección en cada estado, con el técnico asignado y con otro técnico; se aprueba si no aparece en pantalla ni en la API fuera de esa ventana |
| RNF-04 | El sistema no almacenará datos de tarjeta, procesará pagos con una pasarela certificada PCI DSS, transmitirá todo por HTTPS (TLS 1.2 o superior), guardará las contraseñas de administrador con hash con sal y exigirá un segundo factor a los administradores. | Seguridad | Revisión de base de datos y configuración; se aprueba si no hay campos de tarjeta, el tráfico HTTP se redirige a HTTPS, ninguna contraseña está en texto plano y un administrador sin segundo factor no entra |
| RNF-05 | Las fotos de un servicio solo serán accesibles para su cliente, el técnico asignado y los administradores; los documentos y referencias de los técnicos, solo para los administradores. Ambos se almacenarán cifrados. | Seguridad / privacidad | Prueba de acceso con usuarios no autorizados y URL directas; se aprueba si todos los intentos reciben acceso denegado |
| RNF-06 | Las notificaciones de solicitud urgente, los avisos de RF-08 y los códigos de acceso se enviarán en máximo 1 minuto; las pantallas cargarán en máximo 3 segundos (p95) en 4G simulada, y la cotización previa se entregará en máximo 1.5 segundos (p95). | Rendimiento | Medición del tiempo entre el evento y el envío; medición de carga con red 4G simulada en 20 repeticiones por pantalla; medición del endpoint de cotización |
| RNF-07 | Si se pierde la conexión, el sistema conservará el borrador de la solicitud y reintentará la subida de fotos; el usuario podrá regresar a un paso anterior sin perder lo capturado. | Confiabilidad | Prueba cortando la red durante la captura y prueba de retroceder y avanzar; se aprueba si los datos y las fotos se conservan |
| RNF-08 | Un pago no se cobrará dos veces ante reintentos o fallas del sistema. | Confiabilidad | Prueba enviando la misma operación de cobro dos veces; se aprueba si se registra un solo cargo |
| RNF-09 | La aplicación será responsiva con enfoque mobile-first, usable en teléfono y en computadora, y funcionará en las dos últimas versiones mayores de Chrome y Safari móviles, en pantallas desde 360 px de ancho. | Compatibilidad | Ejecución del flujo completo (solicitud a recibo) en esos navegadores y anchos; se aprueba si no hay desplazamiento horizontal |
| RNF-10 | El sistema soportará 500 usuarios concurrentes con tiempo de respuesta de la API menor a 3 segundos (p95) y menos de 1% de errores **(P)**. | Escalabilidad | Prueba de carga con 500 usuarios simulados durante 10 minutos; se aprueba si se cumplen ambos umbrales |
| RNF-11 | El modo simplificado cumplirá WCAG 2.1 nivel AA, incluida la navegación con lector de pantalla y el contraste mínimo. | Accesibilidad | Auditoría automática (por ejemplo, axe o Lighthouse) más recorrido manual con lector de pantalla; se aprueba sin incumplimientos de nivel A o AA |
| RNF-12 | Después de cada acción que cambie datos, el sistema mostrará una confirmación que diga qué ocurrió (por ejemplo, "Listo, Juan llega mañana a las 10"). | Usabilidad | Recorrido de todas las acciones que cambian datos; se aprueba si cada una muestra su confirmación |
| RNF-13 | El sistema no mostrará al cliente temporizadores ni cuentas regresivas, y la sesión no expirará por inactividad mientras tenga un servicio activo. | Usabilidad | Inspección de pantallas; prueba de 60 minutos de inactividad con un servicio activo sin cierre de sesión |
| RNF-14 | Los eventos de cada servicio (cotización, acuerdo inicial, evidencias, costos adicionales, autorizaciones, cambios de estado y liberación del pago) quedarán en un historial que no se puede modificar ni borrar, con fecha, hora y usuario. | Integridad / auditabilidad | Intento de editar o borrar un evento con cada rol; se aprueba si el sistema lo impide y el acuerdo original puede reconstruirse |
| RNF-15 | El flujo de un servicio en curso (RF-07 a RF-11) tendrá una disponibilidad de al menos 99.5% mensual dentro del horario de atención **(P)**. | Disponibilidad | Monitoreo mensual del tiempo en servicio |
| RNF-16 | La ayuda con personas operará de 8:00 a 20:00, hora local, todos los días, con un teléfono de respaldo para servicios en curso cuando la aplicación falle. | Operación | Verificación de que el número aparece en los avisos (RF-08-AC-6) y de que la llamada se atiende en horario |
| RNF-17 | Las fotos de evidencia se eliminarán 6 meses después de que el servicio llegue a un estado final o se resuelva su inconformidad **(P)**. | Privacidad | Prueba con fechas simuladas: la foto existe a los 5 meses y no existe a los 6 |
| RNF-18 | El cálculo del rango de precio estará en un componente separado, para poder sustituirlo más adelante por uno basado en datos de la plataforma. | Mantenibilidad | Revisión de diseño: el cálculo del rango es un componente independiente con una interfaz definida |
| RNF-19 | En una prueba de usabilidad con al menos 5 adultos mayores de 60 años, al menos 80% completará una contratación en modo simplificado sin ayuda **(P)**. | Usabilidad | Prueba de usabilidad moderada antes de la entrega |
| RNF-20 | El cliente y el técnico no verán el teléfono del otro; toda comunicación entre ellos pasará por la plataforma. | Privacidad | Revisión de pantallas y respuestas de la API con ambos roles; se aprueba si el teléfono de la otra parte no aparece |
| RNF-21 | La contratación de un trabajo de catálogo, desde elegir la categoría hasta autorizar el pago, se completará en 5 pantallas de decisión como máximo, sin contar el inicio de sesión. | Usabilidad | Recorrido del flujo de RF-02 a RF-10 contando pantallas |

## 3.3 Requerimientos de dominio

- **RD-01:** Solo pueden prestar servicios los técnicos validados, y solo ellos se muestran como "Verificado". No se exige factura, alta fiscal ni certificación formal: la experiencia puede acreditarse con trabajos anteriores y referencias. Los técnicos sin historial se muestran como "Nuevo". *Relacionado con:* RF-03, RF-04.
- **RD-02:** Ningún cobro puede hacerse sin autorización explícita del cliente: el precio inicial al contratar y cada costo adicional al aprobarlo. La falta de respuesta a un costo adicional cuenta como rechazo; la falta de respuesta a la confirmación del trabajo cuenta como aceptación a las 72 horas. El sistema debe decirlo en cada caso. *Relacionado con:* RF-09, RF-10, RF-20.
- **RD-03:** La comisión de la plataforma debe ser visible antes de contratar; su monto queda por definir y debe poder configurarse. *Relacionado con:* RF-05.
- **RD-04:** Se acepta tarjeta, transferencia y pago en OXXO, siempre a través de la pasarela. No hay manejo de dinero directo entre cliente y técnico, porque no permitiría retener el pago. *Relacionado con:* RF-10.
- **RD-05:** Los datos personales (INE, domicilio, fotos) se tratan conforme a la LFPDPPP, con aviso de privacidad publicado. *Relacionado con:* RF-03, RNF-03, RNF-05, RNF-17.
- **RD-06:** El técnico entrega una carta de no antecedentes penales que un administrador revisa. La plataforma no consulta antecedentes ante autoridades. *Relacionado con:* RF-03.
- **RD-07:** Los servicios se cobran por trabajo según el catálogo, no por hora. *Relacionado con:* RF-16.
- **RD-08:** El rango de referencia lo define el equipo de FairFix con investigación del mercado local. Un técnico puede cotizar fuera del rango solo si lo justifica. *Relacionado con:* RF-16, RF-17, RF-05.
- **RD-09:** Si el cliente rechaza un costo adicional, el técnico solo cobra lo acordado y debe dejar la instalación funcionando o, por lo menos, segura. *Relacionado con:* RF-09.
- **RD-10:** Corregir un error del propio técnico no es un trabajo adicional cobrable. *Relacionado con:* RF-09.
- **RD-11:** La cotización aceptada, con su alcance y evidencia, es el acuerdo inicial. Todo cambio económico o de alcance se guarda como cambio posterior y requiere autorización. *Relacionado con:* RF-06, RF-09, RNF-14.
- **RD-12:** Cuando la información no permite una valoración razonable, el técnico puede pedir una visita de diagnóstico antes de cotizar; el sistema no lo obliga a cotizar a distancia. *Relacionado con:* RF-18.
- **RD-13:** La validación de técnicos, la mediación de dudas y la resolución de inconformidades las hace personal de FairFix, no un proceso automático. *Relacionado con:* RF-03, RF-11, RF-21.
- **RD-14:** Todo trabajo confirmado tiene una garantía de 7 días naturales **(P)**: si la falla vuelve, se revisa sin costo de mano de obra. *Relacionado con:* RF-25.
- **RD-15:** La retención del pago y las obligaciones fiscales de la plataforma (retención de ISR e IVA a técnicos, comprobantes fiscales) requieren revisión legal antes de operar comercialmente; quedan fuera de esta versión. *Relacionado con:* RF-10, RF-13.
- **RD-16:** Las categorías iniciales son plomería, electricidad y carpintería. El catálogo permite agregar otras (por ejemplo, cerrajería o gas) sin cambios en el sistema. *Relacionado con:* RF-16.

---

# Anexo A. Decisiones abiertas

| # | Decisión pendiente | Requerimientos afectados |
|---|---|---|
| A-1 | Monto o porcentaje de la comisión y si se cobra al cliente, al técnico o a ambos. | RF-05, RF-10, RD-03 |
| A-2 | Contenido inicial del catálogo: trabajos, rangos, costo de visita y variables de los trabajos parametrizables, con su fórmula de precio. | RF-16 |
| A-3 | Validar con el stakeholder el plazo de 72 horas para la liberación automática. Dos SRS individuales pedían no liberar sin confirmación del cliente. | RF-20 |
| A-4 | Cancelación del cliente cuando el técnico ya va en camino: hoy solo se atiende por ayuda; falta definir si se cobra el costo de visita. | RF-19 |
| A-5 | Plazo en que el administrador debe resolver una inconformidad o un caso de garantía. | RF-11, RF-25 |
| A-6 | Cómo afectan las cancelaciones e inasistencias a la calificación del técnico y qué umbrales disparan una suspensión. | RF-12, RF-23 |
| A-7 | Tiempo máximo de respuesta del técnico en solicitudes no urgentes. Se usa 10 minutos solo para urgentes. | RF-06 |
| A-8 | Proveedores de la pasarela de pago y de WhatsApp/SMS, y confirmación de que la pasarela permite retener pagos de OXXO y transferencia. | RF-08, RF-10 |
| A-9 | Costo de la visita de diagnóstico y si se descuenta del precio cuando el cliente acepta la cotización. | RF-18 |
| A-10 | Garantía: validar los 7 días y definir quién paga los materiales de la revisión. | RF-25, RD-14 |
| A-11 | Revisión legal de la retención de pagos y de las obligaciones fiscales de la plataforma. | RD-15 |
| A-12 | Rastreo de la llegada del técnico y atención de emergencias 24/7: el cliente real entrevistado los mencionó y ningún requerimiento los cubre. | RF-07, RNF-16 |
| A-13 | Confirmar con más entrevistados los valores marcados como (P). | Todo el documento |
| A-14 | Actualizar `casos_de_uso.puml` con los casos de uso de RF-17 a RF-27. | Anexo B |

---

# Anexo B. Matriz de trazabilidad

| RF | Criterios de aceptación | Casos de uso | RNF y RD relacionados |
|---|---|---|---|
| RF-01 | RF-01-AC-1 a RF-01-AC-11 (11) | UC01 | RNF-04, RNF-06 |
| RF-02 | RF-02-AC-1 a RF-02-AC-10 (10) | UC02 | RNF-05, RNF-06, RNF-07, RD-16 |
| RF-03 | RF-03-AC-1 a RF-03-AC-11 (11) | UC23, UC24 | RD-01, RD-05, RD-06, RD-13, RNF-05 |
| RF-04 | RF-04-AC-1 a RF-04-AC-6 (6) | UC05 | RD-01, RNF-05, RNF-20 |
| RF-05 | RF-05-AC-1 a RF-05-AC-6 (6) | UC06 | RD-03, RD-08, RNF-06, RNF-18 |
| RF-06 | RF-06-AC-1 a RF-06-AC-8 (8) | UC07, UC10 | RD-02, RD-11, RNF-03 |
| RF-07 | RF-07-AC-1 a RF-07-AC-7 (7) | UC11, UC14 | RNF-02, RNF-14 |
| RF-08 | RF-08-AC-1 a RF-08-AC-6 (6) | UC12 | RNF-06, RNF-16 |
| RF-09 | RF-09-AC-1 a RF-09-AC-11 (11) | UC13, UC15 | RD-02, RD-09, RD-10, RD-11 |
| RF-10 | RF-10-AC-1 a RF-10-AC-7 (7) | UC08, UC16, UC17 | RD-02, RD-04, RD-15, RNF-04, RNF-08 |
| RF-11 | RF-11-AC-1 a RF-11-AC-11 (11) | UC20, UC21 | RD-13, RNF-03, RNF-05 |
| RF-12 | RF-12-AC-1 a RF-12-AC-6 (6) | UC09 | — |
| RF-13 | RF-13-AC-1 a RF-13-AC-7 (7) | UC18, UC19 | RD-15 |
| RF-14 | RF-14-AC-1 a RF-14-AC-5 (5) | UC22 | RNF-05 |
| RF-15 | RF-15-AC-1 a RF-15-AC-6 (6) | UC03, UC04 | RNF-01, RNF-09, RNF-11, RNF-19 |
| RF-16 | RF-16-AC-1 a RF-16-AC-6 (6) | UC25 | RD-07, RD-08, RD-16, RNF-18 |
| RF-17 | RF-17-AC-1 a RF-17-AC-6 (6) | Por agregar | RD-08 |
| RF-18 | RF-18-AC-1 a RF-18-AC-7 (7) | Por agregar | RD-11, RD-12 |
| RF-19 | RF-19-AC-1 a RF-19-AC-3 (3) | Por agregar | — |
| RF-20 | RF-20-AC-1 a RF-20-AC-4 (4) | Por agregar | RD-02, RNF-13 |
| RF-21 | RF-21-AC-1 a RF-21-AC-7 (7) | UC04, por agregar | RD-13, RNF-16, RNF-20 |
| RF-22 | RF-22-AC-1 a RF-22-AC-3 (3) | Por agregar | RNF-16 |
| RF-23 | RF-23-AC-1 a RF-23-AC-4 (4) | Por agregar | RD-01 |
| RF-24 | RF-24-AC-1 a RF-24-AC-2 (2) | Por agregar | — |
| RF-25 | RF-25-AC-1 a RF-25-AC-3 (3) | Por agregar | RD-14, RNF-03 |
| RF-26 | RF-26-AC-1 a RF-26-AC-3 (3) | Por agregar | — |
| RF-27 | RF-27-AC-1 a RF-27-AC-2 (2) | Por agregar | — |
| **Total** | **168 criterios** | | |
