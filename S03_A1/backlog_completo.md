# Product Backlog — FairFix

**Proyecto:** FairFix, plataforma de servicios para el hogar con técnicos verificados  
**Versión:** 1.0  
**Fecha:** 3 de octubre de 2026  
**Documento fuente:** `SRS_equipo.md` (v2.0)  
**Convención de estimación:** Escala Fibonacci (1, 2, 3, 5, 8, 13 Story Points).

---

## 1. Definición de Épicas

Para organizar el alcance en incrementos coherentes de producto durante las 12 semanas de desarrollo, el trabajo se agrupa en 4 épicas:

- **ÉPICA 1: Autenticación, Identidad y Confianza (AIC)**  
  Abarca el acceso seguro por teléfono/código sin contraseña para clientes y técnicos, MFA para administradores y el flujo de validación documental y antecedentes para garantizar la confianza de la plataforma.
- **ÉPICA 2: Catálogo, Búsqueda y Contratación con Precios Justos (CPC)**  
  Cubre la administración del catálogo de servicios con rangos de referencia, la oferta y disponibilidad del técnico, la solicitud con cotización previa transparente y el acuerdo inicial inmutable.
- **ÉPICA 3: Ejecución de Servicio, Control de Cambios y Notificaciones (ESC)**  
  Gestiona la máquina de estados del ciclo de vida del servicio, el registro y autorización estricta de costos adicionales con evidencia fotográfica y las alertas multicanal (WhatsApp/SMS).
- **ÉPICA 4: Custodia de Pagos, Cierre, Garantía y Soporte Accesible (PCG)**  
  Comprende la retención (escrow) y liberación de pagos (confirmación o regla de 72 h), resolución de inconformidades, recibos, calificaciones, garantía posterior y modo simplificado para adultos mayores.

---

## 2. Resumen del Product Backlog

| ID | Historia de Usuario | Épica | SP | Prioridad |
|---|---|---|:---:|:---:|
| **US-01** | Inicio de sesión con teléfono y código de acceso sin contraseña | AIC | 5 | Alta |
| **US-02** | Validación y verificación documental de técnicos por administrador | AIC | 8 | Alta |
| **US-03** | Gestión del catálogo de servicios con rangos de referencia y límites | CPC | 5 | Alta |
| **US-04** | Registro de disponibilidad y cotización por trabajo del técnico | CPC | 5 | Alta |
| **US-05** | Creación de solicitud de servicio y cotización previa transparente | CPC | 5 | Alta |
| **US-06** | Selección y contratación de técnico con acuerdo inicial inmutable | CPC | 5 | Alta |
| **US-07** | Solicitud y cotización de trabajos especiales fuera de catálogo | CPC | 8 | Media |
| **US-08** | Retención de pago en pasarela al confirmar contratación | PCG | 8 | Alta |
| **US-09** | Seguimiento y avance de estados del servicio en tiempo real | ESC | 5 | Alta |
| **US-10** | Solicitud y aprobación explícita de costos adicionales con evidencia | ESC | 8 | Alta |
| **US-11** | Notificaciones críticas del servicio por WhatsApp con respaldo SMS | ESC | 5 | Alta |
| **US-12** | Confirmación de trabajo, liberación de fondos y recibo digital | PCG | 5 | Alta |
| **US-13** | Apertura y resolución administrativa de inconformidades | PCG | 8 | Alta |
| **US-14** | Liberación automática de pago a las 72 horas con recordatorio | PCG | 3 | Media |
| **US-15** | Flujo de contratación en modo simplificado para adultos mayores | PCG | 5 | Alta |

**Total de Story Points:** 88 SP

---

## 3. Detalle de Historias de Usuario

### ÉPICA 1: Autenticación, Identidad y Confianza (AIC)

#### US-01: Inicio de sesión con teléfono y código de acceso sin contraseña
- **Historia:**  
  Como **cliente o técnico**,  
  quiero **iniciar sesión usando únicamente mi número de teléfono y un código de un solo uso recibido por WhatsApp o SMS**,  
  para **acceder de forma segura a la plataforma sin tener que recordar ni gestionar contraseñas complejas**.
- **Criterios de Aceptación:**
  - **AC-01.1 (Ref: RF-01-AC-5, RF-01-AC-6):** Dado que un cliente o técnico registrado ingresa su número telefónico, cuando introduce el código correcto dentro de los 10 minutos de vigencia, el sistema inicia la sesión y expone el rol correspondiente sin solicitar contraseña. Si el código es incorrecto o venció, el sistema responde con error 401 permitiendo solicitar uno nuevo.
  - **AC-01.2 (Ref: RF-01-AC-8, RF-01-AC-9):** Dado que se envía un código por WhatsApp, si pasan 2 minutos sin entrega confirmada, el sistema envía el código automáticamente por SMS. Tras 5 intentos fallidos consecutivos, el acceso para dicho teléfono queda bloqueado por 15 minutos y se muestra enlace a "Necesito ayuda".
- **Estimación:** 5 Story Points *(Justificación: Requiere integración con proveedor de mensajería para WhatsApp/SMS, lógica de tokens OTP con expiración y control de bloqueo por intentos fallidos).*
- **Prioridad:** Alta

---

#### US-02: Validación y verificación documental de técnicos por administrador
- **Historia:**  
  Como **administrador de FairFix**,  
  quiero **revisar la INE, selfie, comprobante de domicilio, carta de no antecedentes y acreditación de experiencia de los técnicos postulantes**,  
  para **aprobar únicamente a técnicos confiables y mostrarlos con la insignia de "Verificado" ante los clientes**.
- **Criterios de Aceptación:**
  - **AC-02.1 (Ref: RF-03-AC-1, RF-03-AC-4, RF-03-AC-7):** Dado un técnico en estado "en revisión", cuando el administrador valida y aprueba los 4 documentos y la acreditación de experiencia (certificación o 2 referencias + fotos), el técnico pasa a estado "validado" y se habilita para aparecer en búsquedas públicas. Cualquier intento de cambio por un no-administrador devuelve HTTP 403.
  - **AC-02.2 (Ref: RF-03-AC-5, RF-03-AC-6):** Dado un técnico en revisión, cuando el administrador decide rechazarlo, el sistema exige ingresar un motivo explícito de rechazo antes de registrarlo; una vez guardado, el técnico recibe la notificación con el motivo para poder corregir su expediente.
- **Estimación:** 8 Story Points *(Justificación: Módulo administrativo con visor de archivos sensibles cifrados, flujo de estados de validación, manejo de feedback de rechazo y reglas estrictas de control de acceso RBAC).*
- **Prioridad:** Alta

---

### ÉPICA 2: Catálogo, Búsqueda y Contratación con Precios Justos (CPC)

#### US-03: Gestión del catálogo de servicios con rangos de referencia y límites
- **Historia:**  
  Como **administrador**,  
  quiero **crear y editar categorías, trabajos, rangos de precios de referencia y costos de visita**,  
  para **mantener un catálogo estandarizado que evite abusos y oriente a clientes y técnicos**.
- **Criterios de Aceptación:**
  - **AC-03.1 (Ref: RF-16-AC-1, RF-16-AC-2):** Dado un administrador autenticado, cuando registra un nuevo trabajo con precio mínimo > 0, precio máximo $\le 1.3 \times$ precio mínimo y costo de visita $\ge 0$, el trabajo se guarda como activo. Si el rango supera el 130% o el mínimo es $\le 0$, el sistema rechaza el guardado con código 422.
  - **AC-03.2 (Ref: RF-16-AC-3, RF-16-AC-4):** Dado un trabajo del catálogo que sufre cambios de rango o desactivación, cuando existen solicitudes previas o servicios ya contratados bajo ese trabajo, el sistema garantiza que los acuerdos y servicios previos mantengan los precios y condiciones originales intactos.
- **Estimación:** 5 Story Points *(Justificación: Lógica CRUD en base de datos con reglas de validación aritmética estricta [regla del 130%] y versionado de precios para no corromper datos históricos).*
- **Prioridad:** Alta

---

#### US-04: Registro de disponibilidad y cotización por trabajo del técnico
- **Historia:**  
  Como **técnico validado**,  
  quiero **definir mis horarios de trabajo y mi precio cotizado para cada trabajo del catálogo**,  
  para **recibir solicitudes que se ajusten a mi agenda y mostrar mi cotización por adelantado al cliente**.
- **Criterios de Aceptación:**
  - **AC-04.1 (Ref: RF-17-AC-1, RF-17-AC-2):** Dado un técnico con estatus "validado", cuando configura sus días y franjas horarias hábiles y asigna un precio dentro del rango oficial de un trabajo, el sistema guarda su oferta sin solicitar justificación y lo lista como disponible en esas ventanas.
  - **AC-04.2 (Ref: RF-17-AC-3, RF-17-AC-4, RF-17-AC-6):** Si el técnico ingresa un precio fuera del rango oficial, el sistema bloquea el guardado a menos que proporcione una justificación de texto obligatoria. Si un técnico sin validar intenta usar este módulo, el sistema devuelve HTTP 403.
- **Estimación:** 5 Story Points *(Justificación: Gestión de ventanas de calendario, validación contra rangos del catálogo y manejo de estados condicionales para tarifas excepcionales).*
- **Prioridad:** Alta

---

#### US-05: Creación de solicitud de servicio y cotización previa transparente
- **Historia:**  
  Como **cliente**,  
  quiero **describir mi necesidad con fotos, indicar urgencia y visualizar el desglose del precio antes de contratar**,  
  para **saber exactamente cuánto costará el trabajo, cuál es el rango de referencia y qué comisión cobra la plataforma**.
- **Criterios de Aceptación:**
  - **AC-05.1 (Ref: RF-02-AC-1, RF-02-AC-4, RF-02-AC-7):** Dado un cliente con sesión, cuando crea una solicitud seleccionando un trabajo y adjunta descripción y hasta 5 fotos, el sistema crea la solicitud en estado `REQUESTED`. La interfaz muestra advertencia de privacidad para fotografiar únicamente la falla antes de subir imágenes.
  - **AC-05.2 (Ref: RF-05-AC-1, RF-05-AC-4):** Antes de confirmar la contratación, el sistema muestra de forma diferenciada: cotización del técnico, rango de referencia ("normalmente cuesta entre $X y $Y"), comisión de plataforma y total a pagar. Si el técnico cotizó fuera de rango, se despliega la justificación ingresada por el técnico.
- **Estimación:** 5 Story Points *(Justificación: Pantallas de carga con componentes multimedia [dropzone/cámara], llamadas al motor de cotizaciones y maquetación clara del desglose financiero).*
- **Prioridad:** Alta

---

#### US-06: Selección y contratación de técnico con acuerdo inicial inmutable
- **Historia:**  
  Como **cliente**,  
  quiero **elegir a un técnico disponible y confirmar una ventana de llegada**,  
  para **establecer un acuerdo inicial formal con precio y alcance congelados que evite cargos sorpresivos**.
- **Criterios de Aceptación:**
  - **AC-06.1 (Ref: RF-06-AC-1, RF-06-AC-3, RF-06-AC-4):** Dado un técnico disponible, cuando el cliente lo elige, el técnico recibe notificación para aceptar. Al aceptar, el sistema fija la cotización mostrada como monto vinculante, prohibiendo cualquier modificación unilateral de precio por parte del técnico.
  - **AC-06.2 (Ref: RF-06-AC-8, RD-11):** Una vez que el técnico acepta y se autoriza la retención de pago, el sistema almacena de forma inmutable la fecha, alcance, precio y fotos como "acuerdo inicial", sirviendo de base contractual para cualquier reclamo futuro.
- **Estimación:** 5 Story Points *(Justificación: Manejo de concurrencia para apartar ventanas horarias, fijación de snapshot inmutable en base de datos y transiciones de estado de servicio).*
- **Prioridad:** Alta

---

#### US-07: Solicitud y cotización de trabajos especiales fuera de catálogo
- **Historia:**  
  Como **cliente con una necesidad no estándar**,  
  quiero **solicitar una cotización particular o una visita de diagnóstico para trabajos especiales**,  
  para **obtener una propuesta técnica y económica formal cuando el trabajo no encaja en el catálogo común**.
- **Criterios de Aceptación:**
  - **AC-07.1 (Ref: RF-18-AC-1, RF-18-AC-3):** Dado que el cliente selecciona "Otro trabajo sujeto a cotizar", cuando envía los detalles, el técnico puede responder con una cotización personalizada o marcar que "requiere visita de diagnóstico" con su costo asociado, requiriendo aprobación expresa del cliente.
  - **AC-07.2 (Ref: RF-18-AC-4, RF-18-AC-5, RF-18-AC-7):** Cuando el técnico emite la cotización particular con alcance y condiciones, el cliente puede aceptarla (avanzando a la retención de pago) o rechazarla sin costo. El técnico no puede iniciar el servicio (`ON_THE_WAY`) sin aceptación previa (HTTP 409).
- **Estimación:** 8 Story Points *(Justificación: Flujo asíncrono bidireccional de negociación, soporte para visitas de evaluación presencial y ramificación de reglas fuera de catálogo estándar).*
- **Prioridad:** Media

---

### ÉPICA 3: Ejecución de Servicio, Control de Cambios y Notificaciones (ESC)

#### US-08: Retención de pago en pasarela al confirmar contratación
- **Historia:**  
  Como **cliente**,  
  quiero **que el importe total del servicio quede retenido en garantía mediante tarjeta, transferencia u OXXO**,  
  para **asegurar al técnico que los fondos existen sin que se le entreguen hasta que el trabajo esté terminado a mi entera satisfacción**.
- **Criterios de Aceptación:**
  - **AC-08.1 (Ref: RF-10-AC-1, RF-10-AC-5, RNF-04):** Dado un servicio acordado por monto M y comisión C, cuando el cliente autoriza el cobro vía pasarela (tarjeta, transferencia u OXXO), el sistema bloquea los fondos en custodia (`escrow`) y transiciona el servicio a `ACCEPTED`. Se prohíbe taxativamente registrar transacciones en efectivo directo al técnico.
  - **AC-08.2 (Ref: RF-10-AC-2, RNF-08):** Si la pasarela rechaza la transacción o el cliente no completa el depósito, el servicio permanece en `REQUESTED` sin cobrar ningún cargo. El endpoint de pago debe ser estrictamente idempotente ante reintentos de red.
- **Estimación:** 8 Story Points *(Justificación: Integración técnica con API de pasarela de pagos externa, manejo de webhooks asíncronos para OXXO/transferencia y blindaje de idempotencia transaccional).*
- **Prioridad:** Alta

---

#### US-09: Seguimiento y avance de estados del servicio en tiempo real
- **Historia:**  
  Como **usuario (cliente o técnico)**,  
  quiero **actualizar y visualizar los cambios de estado del servicio (`ACCEPTED` $\to$ `ON_THE_WAY` $\to$ `IN_PROGRESS` $\to$ `FINISHED`)**,  
  para **tener certidumbre del avance físico del trabajo y resguardar la privacidad del domicilio**.
- **Criterios de Aceptación:**
  - **AC-09.1 (Ref: RF-07-AC-1, RF-07-AC-2, RF-07-AC-5):** El técnico asignado puede avanzar el estado únicamente en secuencia lineal permitida (`ACCEPTED` a `ON_THE_WAY`, de ahí a `IN_PROGRESS`, y finalmente a `FINISHED`). Cualquier salto o retroceso es rechazado por la API (HTTP 409/422). Todo cambio registra timestamp y usuario.
  - **AC-09.2 (Ref: RNF-03, RF-07-AC-7):** La dirección física exacta del cliente es visible para el técnico únicamente a partir de `ACCEPTED` y hasta que el servicio llega a `FINISHED`. Usuarios ajenos que intenten consultar el detalle del servicio reciben HTTP 403.
- **Estimación:** 5 Story Points *(Justificación: Máquina de estados formal con candados de seguridad en base de datos, auditoría de eventos y ofuscación condicional de datos privados de geolocalización).*
- **Prioridad:** Alta

---

#### US-10: Solicitud y aprobación explícita de costos adicionales con evidencia
- **Historia:**  
  Como **técnico en sitio**,  
  quiero **solicitar autorización con fotos y descripción si descubro un imprevisto real durante la reparación**,  
  para **cubrir refacciones o maniobras indispensables sin que el cliente sufra cobros no autorizados**.
- **Criterios de Aceptación:**
  - **AC-10.1 (Ref: RF-09-AC-1, RF-09-AC-2, RF-09-AC-8):** Estando el servicio en `IN_PROGRESS`, el técnico puede registrar un adicional adjuntando monto > 0, descripción y de 1 a 5 fotos de evidencia. Si falta foto o descripción, el sistema devuelve HTTP 422. En cualquier otro estado del servicio, la petición devuelve HTTP 409.
  - **AC-10.2 (Ref: RF-09-AC-3, RF-09-AC-4, RF-09-AC-5, RD-02):** Si el cliente aprueba el adicional, se amplía el monto en custodia; si lo rechaza o no responde antes de que el técnico marque `FINISHED`, el adicional se descarta formalmente con costo $0 y el técnico está obligado a concluir lo pactado originalmente dejando la instalación segura.
- **Estimación:** 8 Story Points *(Justificación: Flujo crítico del modelo de negocio; involucra subida de evidencia multimedia, ampliación de autorización financiera en pasarela y tratamiento de reglas por omisión).*
- **Prioridad:** Alta

---

#### US-11: Notificaciones críticas del servicio por WhatsApp con respaldo SMS
- **Historia:**  
  Como **cliente**,  
  quiero **recibir alertas oportunas en WhatsApp y SMS ante cada cambio de estado o aviso importante**,  
  para **mantenerme enterado del estado de mi servicio sin depender de estar mirando continuamente la aplicación web**.
- **Criterios de Aceptación:**
  - **AC-11.1 (Ref: RF-08-AC-1, RF-08-AC-4, RF-08-AC-6):** Ante cualquier transición de estado o solicitud de costo adicional, el sistema dispara en menos de 1 minuto un mensaje de WhatsApp al cliente con ID de servicio, trabajo, nuevo estado, fecha/hora y teléfono de soporte FairFix.
  - **AC-11.2 (Ref: RF-08-AC-2, RF-08-AC-3):** Si el webhook de entrega de WhatsApp no confirma recepción tras 2 minutos, el despachador de eventos reenvía automáticamente el contenido vía SMS al mismo teléfono.
- **Estimación:** 5 Story Points *(Justificación: Configuración de colas asíncronas de mensajería [Worker/Job], monitoreo de recibos de entrega y fallback automático a SMS).*
- **Prioridad:** Alta

---

### ÉPICA 4: Custodia de Pagos, Cierre, Garantía y Soporte Accesible (PCG)

#### US-12: Confirmación de trabajo, liberación de fondos y recibo digital
- **Historia:**  
  Como **cliente satisfecho**,  
  quiero **confirmar la finalización del trabajo desde la aplicación y calificar al técnico**,  
  para **liberar el dinero al técnico, descargar mi recibo formal y ayudar a otros clientes con mi reseña**.
- **Criterios de Aceptación:**
  - **AC-12.1 (Ref: RF-10-AC-3, RF-13-AC-1, RF-13-AC-7):** Cuando el cliente presiona "Confirmar trabajo" sobre un servicio `FINISHED`, el servicio pasa a `CONFIRMED`, la pasarela transfiere los fondos al técnico, FairFix cobra su comisión y se genera de inmediato un recibo descargable en PDF con desglose aritméticamente exacto ($Total = M + A + C - D$).
  - **AC-12.2 (Ref: RF-13-AC-2, RF-13-AC-6):** Tras confirmar, el cliente puede ingresar una calificación de 1 a 5 estrellas y un comentario (máximo 500 caracteres). Al guardarse, la calificación promedio pública del técnico se recalcula y refleja en menos de 5 segundos.
- **Estimación:** 5 Story Points *(Justificación: Liquidación de órdenes en pasarela, generación de documentos PDF formateados y actualización reactiva de promedios en perfiles).*
- **Prioridad:** Alta

---

#### US-13: Apertura y resolución administrativa de inconformidades
- **Historia:**  
  Como **cliente inconforme**,  
  quiero **reportar con evidencia si un trabajo quedó mal realizado antes de confirmar el pago**,  
  para **mantener mi dinero retenido hasta que un administrador medie y resuelva una compensación o visita correctiva**.
- **Criterios de Aceptación:**
  - **AC-13.1 (Ref: RF-11-AC-1, RF-11-AC-4):** Mientras el servicio esté en `FINISHED`, el cliente puede emitir un reporte de inconformidad con motivo y al menos una foto. El servicio cambia automáticamente a `IN_REVIEW`, impidiendo cualquier confirmación o liberación de fondos.
  - **AC-13.2 (Ref: RF-11-AC-5, AC-6, AC-7, AC-10):** El administrador revisa el expediente y aplica una de 4 resoluciones únicas: liberar al técnico, reembolso total al cliente, reembolso parcial pactado, o retorno a `IN_PROGRESS` para una visita de corrección sin costo de mano de obra.
- **Estimación:** 8 Story Points *(Justificación: Lógica compleja de conciliación y reversión financiera en pasarela [reembolsos parciales y totales], gestión de expedientes y estados de contingencia).*
- **Prioridad:** Alta

---

#### US-14: Liberación automática de pago a las 72 horas con recordatorio
- **Historia:**  
  Como **técnico**,  
  quiero **que el pago de un trabajo bien terminado se libere automáticamente si el cliente no responde tras 72 horas**,  
  para **cobrar oportunamente por mi labor sin quedar bloqueado indefinidamente por olvido del usuario**.
- **Criterios de Aceptación:**
  - **AC-14.1 (Ref: RF-20-AC-1, RF-20-AC-4):** Cuando un servicio cumple 48 horas en estado `FINISHED` sin confirmación ni reporte de inconformidad, el sistema envía un recordatorio automático informando la fecha y hora exacta en que se liberará el cobro.
  - **AC-14.2 (Ref: RF-20-AC-2, RF-20-AC-3):** Al transcurrir exactamente 72 horas en `FINISHED`, si no existe un caso de inconformidad abierto, un proceso en segundo plano transiciona el servicio a `CONFIRMED`, dispersa los fondos al técnico y genera el recibo digital.
- **Estimación:** 3 Story Points *(Justificación: Implementación de tareas programadas tipo Cron/Scheduler con comprobación estricta de condiciones de frontera y exclusión de disputas).*
- **Prioridad:** Media

---

#### US-15: Flujo de contratación en modo simplificado para adultos mayores
- **Historia:**  
  Como **adulto mayor o familiar que solicita en su nombre**,  
  quiero **una interfaz con texto grande, contraste alto, solicitud en 3 pasos y opción de soporte telefónico visible**,  
  para **solucionar problemas de plomería o electricidad en mi casa sin frustrarme con la tecnología**.
- **Criterios de Aceptación:**
  - **AC-15.1 (Ref: RF-15-AC-1, RF-15-AC-2, RNF-01):** Al activar el modo simplificado, la tipografía tiene un mínimo de 18 pt (24 px CSS), los botones miden al menos 48×48 px con espaciado de 8 px, y el flujo completo para solicitar servicio toma máximo 3 pantallas sin tecnicismos visuales.
  - **AC-15.2 (Ref: RF-15-AC-3, RF-15-AC-5, RF-21-AC-1):** Permite registrar nombre y teléfono de un contacto familiar para recibir réplica de todas las notificaciones vía WhatsApp/SMS. En todas las pantallas permanece visible el botón de emergencia "Necesito ayuda" para enlace telefónico directo.
- **Estimación:** 5 Story Points *(Justificación: Adaptabilidad responsiva con diseño accesible estricto [WCAG 2.1 AA], maquetación de flujos cortos condensados y gestión de datos de terceros).*
- **Prioridad:** Alta

---

## 4. Instrucciones para GitHub Projects (Parte 2 de la actividad)

Para transferir este Product Backlog al tablero de **GitHub Projects** del equipo (`TC5062-[Num_equipo]`):

1. **Creación del Proyecto:**
   - En el repositorio, entrar a la pestaña **Projects** y crear un proyecto con plantilla de tipo **Board** (Kanban).
   - Columnas recomendadas: `Backlog`, `Ready (Sprint)`, `In Progress`, `In Review / Test`, `Done`.

2. **Creación de Issues:**
   - Crear un **GitHub Issue** por cada historia de usuario (`US-01` a `US-15`).
   - **Title:** `US-XX: [Título de la historia]` (ejemplo: `US-01: Inicio de sesión con teléfono y código de acceso sin contraseña`).
   - **Description:** Pegar la redacción formal de la historia (*Como... quiero... para...*) y la sección completa de *Criterios de Aceptación* con sus IDs del SRS.
   - **Labels:** Crear y asociar etiquetas consistentes:
     - Épica: `epic:AIC`, `epic:CPC`, `epic:ESC`, `epic:PCG`.
     - Prioridad: `priority:Alta`, `priority:Media`, `priority:Baja`.
     - Estimación: `sp:3`, `sp:5`, `sp:8`.
     - Tipo: `user-story`.

3. **Captura del Tablero:**
   - Una vez cargados los 15 issues en la columna `Backlog`, tomar la captura de pantalla completa del tablero y guardarla con el nombre exacto: `backlog_screenshot.png`.