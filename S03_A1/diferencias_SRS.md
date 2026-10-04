# Análisis de diferencias entre los SRS individuales — FairFix

**Fecha:** 3 de octubre de 2026
**Resultado:** `SRS_equipo.md` (versión 2.0 consolidada)
**Documentos comparados:**

| Clave | Integrante | Archivo | RF | AC | RNF | RD |
|---|---|---|---|---|---|---|
| **R** | Raul Adrian Delgado Rodriguez (A01246414) | `SRS_final_A01246414_Raul.md` | 16 | 93 | 10 | 6 |
| **A** | Alvaro Daniel Zavala Arreola (A01796929) | `SRS_final_A01796929_Alvaro.md` | 32 | 91 | 21 | 11 |
| **L** | Luis Manuel Mendoza Cruz (A01797418) | `SRS_final_A01797418_Luis.md` | 8 | 16 | 4 | 2 |
| **C** | Carlos Monir Radovich Saad (A01797569) | `SRS_final_A01797569_Carlos.md` | 9 | 21 | 4 | 6 |
| | **SRS del equipo** | `SRS_equipo.md` | 27 | 168 | 21 | 16 |

En las tablas, R, A, L y C se refieren a estos documentos. Los IDs con prefijo (por ejemplo, `A RF-13`) son del SRS individual; los IDs sin prefijo son del SRS del equipo.

---

## 1. Método

1. Se listaron los requerimientos de cada SRS y se agruparon por tema, porque cada documento numera distinto (por ejemplo, `RF-03` es validación de técnicos en R, verificación en A, retención de pago en L y cálculo de precio en C).
2. Cada tema se clasificó como **consenso** (los cuatro lo piden y no se contradicen), **gap** (lo piden uno, dos o tres) o **conflicto** (dos o más versiones no pueden cumplirse a la vez).
3. Los conflictos se resolvieron con estos criterios, en este orden:
   1. Lo que pidió el stakeholder en las entrevistas (R documenta una entrevista real en sus decisiones A-14 a A-17; A cita la transcripción).
   2. Lo que protege al usuario más vulnerable (adulto mayor) y la regla "ningún cobro sin autorización".
   3. Lo que cabe en 12 semanas con una aplicación web.
   4. La posición de la mayoría, cuando los criterios anteriores no deciden.
4. Los gaps se incorporaron cuando no contradicen el alcance y se les asignó prioridad (Alta, Media, Baja) para que la restricción de 12 semanas siga siendo manejable. Los que no se incorporaron están en la sección 6.

### Regla para los IDs de criterios de aceptación

- Se tomó como base la numeración de R porque es la que tiene más criterios, casos de uso asociados y matriz de trazabilidad. **Los 93 IDs de R se conservan**: 69 sin cambio de fondo `[=]` y 24 con el mismo ID y contenido ajustado por un conflicto `[~]`.
- Los criterios nuevos `[+]` (75) se agregan al final de cada RF o en RF-17 a RF-27. Ningún ID se reutiliza para otro tema ni se renumera.
- En los criterios `[=]` solo cambió la terminología unificada ("tipo de servicio" pasó a "trabajo") y, en cuatro casos, un detalle que no altera lo que se prueba (RF-03-AC-1, RF-10-AC-2, RF-11-AC-3, RF-13-AC-2).
- La equivalencia entre los IDs de cada SRS individual y los del equipo está en la sección 7.

---

## 2. Consenso

Requerimientos presentes en los cuatro documentos, sin contradicción de fondo.

| # | Requerimiento | R | A | L | C | SRS del equipo |
|---|---|---|---|---|---|---|
| 1 | Solo los técnicos verificados por la plataforma aparecen o se muestran como verificados | RF-03, RD-01 | RF-01, RF-03 | RF-01 | RF-06-AC-2, RD-04 | RF-03, RD-01 |
| 2 | El cliente ve un precio de referencia antes de contratar | RF-05 | RF-06 | RF-02 | RF-03 | RF-05 |
| 3 | El pago queda retenido y no llega al técnico hasta la conformidad del cliente | RF-10 | RF-12 | RF-03 | RF-09-AC-2, RD-05 | RF-10 |
| 4 | Un costo adicional requiere evidencia y autorización explícita del cliente; sin ella no se cobra | RF-09, RD-02 | RF-08, RF-09, RF-11 | RF-04 | RF-08, RD-01 | RF-09, RD-02 |
| 5 | Recibo digital al cerrar el servicio | RF-13 | RF-23 | RF-06 | RF-09-AC-3 | RF-13 |
| 6 | El cliente califica al técnico con comentario y eso alimenta su reputación pública | RF-13 | RF-21 | RF-07 | RF-09-AC-4 | RF-13 |
| 7 | Interfaz sencilla para adultos mayores y personas con poca experiencia digital | RF-15, RNF-01 | RNF-01 a RNF-06 | RF-08, RNF-02 | RNF-01, RNF-02 | RF-15, RNF-01, RNF-11 |
| 8 | Seguimiento del servicio por estados visibles para el cliente | RF-07 | RF-22 | Estados en RF-03 a RF-05 | RF-09-AC-1 | RF-07 |
| 9 | Control de acceso por rol; el técnico no puede aprobar cobros ni liberar pagos | RF-01-AC-7 | RNF-19 | RNF-01 | RF-01-AC-2, RNF-03 | RF-01, RF-09-AC-11 |
| 10 | Un administrador de la plataforma valida técnicos | RF-03 | RF-03 | RF-01-AC-2 | 2.3 | RF-03 |
| 11 | Comunicación cifrada y datos sensibles protegidos | RNF-04 | RNF-18 | RNF-01 | RNF-03 | RNF-04, RNF-05 |
| 12 | Plomería, electricidad y carpintería como categorías | 1.2 | RD-11 | 1.2 | 1.2 | RD-16 |

**Consenso de tres de cuatro** (el cuarto no lo menciona, no lo contradice):

| Requerimiento | Lo piden | SRS del equipo |
|---|---|---|
| No almacenar datos de tarjeta; pasarela externa | R, A, L | RNF-04 |
| Inconformidad o disputa resuelta por un administrador | R, A, L (como garantía) | RF-11 |
| Catálogo de trabajos con rango, administrado por la plataforma | R, A, C (servicios base) | RF-16 |
| Tamaños mínimos de texto (18) y botones (48×48) | R, A, L | RNF-01 |
| Perfil público del técnico con calificaciones | R, A, C | RF-04 |
| Historial que conserva las decisiones del servicio | R, A, C | RF-07-AC-5, RNF-14 |
| Nombre del sistema: FairFix | A, L, C | Todo el documento |

---

## 3. Gaps

Requerimientos que solo uno o algunos integrantes identificaron y cómo quedaron.

### 3.1 Gaps incorporados

| # | Requerimiento | Quién lo identificó | Resolución | SRS del equipo |
|---|---|---|---|---|
| G-1 | Criterios negativos de autorización (401/403) y de validación en casi todos los RF | R | Se conservan completos; son la base de las pruebas | RF-01 a RF-16 |
| G-2 | Solicitud urgente y tiempo límite de respuesta del técnico | R | Incorporado | RF-02-AC-5, RF-06-AC-7 |
| G-3 | Aviso de privacidad al tomar fotos | R | Incorporado | RF-02-AC-7 |
| G-4 | Retraso, cancelación e inasistencia del técnico, con técnicos alternativos sin repetir la solicitud | R | Incorporado | RF-12 |
| G-5 | Un familiar solicita a nombre de otra persona, con contacto para avisos | R | Incorporado | RF-15-AC-3, RF-15-AC-4 |
| G-6 | Respaldo por SMS cuando WhatsApp no entrega | R (A lo tenía como supuesto S-06) | Incorporado y extendido a los códigos de acceso | RF-08-AC-2, RF-01-AC-9 |
| G-7 | Borrador sin conexión y cobro sin duplicados | R | Incorporado | RNF-07, RNF-08 |
| G-8 | Carga de 500 usuarios concurrentes | R | Incorporado como (P) | RNF-10 |
| G-9 | Historial de servicios del cliente y descarga de recibos | R (A lo menciona en RF-23-AC-2) | Incorporado | RF-14 |
| G-10 | Acreditación de experiencia sin certificación (fotos y dos referencias) | A | Incorporado | RF-03-AC-8 a AC-10, RD-01 |
| G-11 | El técnico registra disponibilidad y cotización por trabajo | A | Incorporado; resuelve además el conflicto C-6 | RF-17 |
| G-12 | Justificación obligatoria de precios fuera de rango | A | Incorporado | RF-17-AC-3, RF-17-AC-4, RF-05-AC-4 |
| G-13 | Opción "Tengo dudas" con mediación de una persona | A | Incorporado | RF-21-AC-4 a AC-7 |
| G-14 | Recordatorio a las 48 horas y liberación automática a las 72 | A | Incorporado; ver conflicto C-4 | RF-20 |
| G-15 | Botón "Necesito ayuda" en todas las pantallas, con horario de atención | A (R solo en modo simplificado) | Incorporado para ambos modos | RF-21-AC-1 a AC-3 |
| G-16 | Solicitud creada por un asesor telefónico | A | Incorporado con prioridad Baja | RF-22 |
| G-17 | Cancelación por parte del cliente | A (R lo tenía como decisión abierta A-3) | Incorporado | RF-19 |
| G-18 | Suspensión y reactivación de técnicos | A | Incorporado; los servicios aceptados regresan a "solicitado" en lugar de cancelarse | RF-23 |
| G-19 | El técnico consulta sus pagos | A | Incorporado | RF-24 |
| G-20 | Promociones | A | Incorporado con prioridad Baja | RF-26 |
| G-21 | Indicadores de éxito con fórmulas | A | Incorporado con prioridad Baja | RF-27 |
| G-22 | Confirmación después de cada acción; sin temporizadores; sesión que no expira con servicio activo; regresar sin perder datos | A | Incorporado | RNF-12, RNF-13, RNF-07 |
| G-23 | Eliminación de fotos a los 6 meses | A | Incorporado como (P) | RNF-17 |
| G-24 | Cálculo del rango en un componente separado | A | Incorporado | RNF-18 |
| G-25 | Prueba de usabilidad con adultos mayores (80%) | A | Incorporado como (P) | RNF-19 |
| G-26 | Teléfono de respaldo en los avisos | A | Incorporado | RF-08-AC-6, RNF-16 |
| G-27 | Cobro por trabajo, no por hora; rango definido con investigación de mercado; instalación segura tras un rechazo | A | Incorporado | RD-07, RD-08, RD-09 |
| G-28 | Segundo factor para administradores | L | Incorporado | RF-01-AC-10, RNF-04 |
| G-29 | WCAG 2.1 AA y lector de pantalla | L | Incorporado para el modo simplificado | RNF-11 |
| G-30 | El cliente y el técnico no ven el teléfono del otro | L | Incorporado | RNF-20, RF-04-AC-6, RF-21-AC-7 |
| G-31 | El total del recibo coincide exactamente con lo cobrado | L | Incorporado | RF-13-AC-7 |
| G-32 | El promedio del técnico se actualiza en menos de 5 segundos | L | Incorporado | RF-13-AC-6 |
| G-33 | Resumen en una sola pantalla antes de generar el cargo | L | Incorporado (sin la confirmación por audio) | RF-15-AC-6 |
| G-34 | Garantía de 7 días | L (R lo tenía como decisión abierta A-16, pedida por el cliente real) | Incorporado como (P) con prioridad Baja | RF-25, RD-14 |
| G-35 | Tiempo de respuesta de la cotización (1.5 s p95) | L | Incorporado | RNF-06 |
| G-36 | Trabajos especiales con cotización particular | C | Incorporado | RF-18 |
| G-37 | El técnico pide más información o una visita de diagnóstico | C | Incorporado | RF-18-AC-2, RF-18-AC-3, RD-12 |
| G-38 | Servicios parametrizables (precio según variables) | C | Incorporado; la fórmula queda abierta (A-2) | RF-16-AC-6, RF-02-AC-9, RF-05-AC-6 |
| G-39 | Portafolio del técnico: fotos de trabajos, marcas y materiales | C | Incorporado | RF-04-AC-5 |
| G-40 | La cotización aceptada se guarda como acuerdo inicial | C | Incorporado | RF-06-AC-8, RD-11 |
| G-41 | La corrección de un error del técnico no se cobra | C | Incorporado | RF-09-AC-9, RD-10 |
| G-42 | Usable en teléfono y en computadora | C | Incorporado | RNF-09 |

### 3.2 Gaps que ningún SRS cubría y siguen abiertos

| Tema | Dónde quedó |
|---|---|
| Rastreo de la llegada del técnico y emergencias 24/7 (R los registró como petición del cliente real, sin requerimiento) | Anexo A, A-12 |
| Monto de la comisión | Anexo A, A-1 |
| Cancelación del cliente con el técnico ya en camino | Anexo A, A-4 |
| Efecto de las cancelaciones en la calificación y umbrales de suspensión | Anexo A, A-6 |
| Casos de uso de RF-17 a RF-27 en el diagrama | Anexo A, A-14 |

---

## 4. Conflictos y su resolución

| # | Tema | Posiciones | Resolución en el SRS del equipo | Motivo |
|---|---|---|---|---|
| C-1 | Inicio de sesión | **R:** correo y contraseña de 8 caracteres. **A:** teléfono y código por WhatsApp, sin contraseña. **L:** MFA para administradores. **C:** no lo define. | Clientes y técnicos entran con teléfono y código (WhatsApp, respaldo SMS). Administradores entran con correo, contraseña y segundo factor. RF-01-AC-1, 2, 3, 5 y 6 cambian; se agregan AC-8 a AC-11. | El perfil de usuario de los cuatro SRS describe adultos mayores que usan WhatsApp y olvidan contraseñas. La contraseña se conserva donde sí aporta seguridad (administradores). |
| C-2 | Documentos del técnico y antecedentes | **R:** INE y comprobante; sin antecedentes (RD-06), aunque el cliente real los pidió (A-15). **A:** INE, selfie, comprobante y carta de no antecedentes. **L:** identificación y constancia de antecedentes. | Cuatro documentos: INE, selfie, comprobante de domicilio y carta de no antecedentes. La plataforma revisa la carta; no consulta autoridades (RD-06). RF-03-AC-2 y AC-3 cambian. | Lo pidió el stakeholder real y tres de cuatro lo incluyen. Revisar un documento más no agrega integraciones. |
| C-3 | Métodos de pago | **R:** tarjeta y transferencia; efectivo fuera (el cliente real lo pidió, A-14). **A:** tarjeta, OXXO y transferencia. **L:** solo tarjeta. | Tarjeta, transferencia y OXXO por la pasarela. El efectivo entregado al técnico sigue fuera. RF-10-AC-1 y AC-5 cambian; se agrega AC-7. | OXXO permite pagar en efectivo sin perder la retención, que es lo que R quería proteger. |
| C-4 | Liberación del pago sin respuesta del cliente | **R:** sigue retenido sin plazo (decisión abierta A-2). **A:** recordatorio a las 48 h y liberación a las 72 h. **C:** prohíbe la liberación automática (RD-05). **L:** no aplica (cierra con OTP). | Liberación automática a las 72 h, con recordatorio a las 48 h y la fecha visible para el cliente (RF-20). RF-10-AC-4 cambia. | Sin plazo, el técnico puede no cobrar nunca un trabajo bien hecho. La preocupación de C se atiende con el aviso obligatorio, la ventana de inconformidad y la garantía de 7 días. **Queda por validar (A-3)** porque contradice la posición de C. |
| C-5 | Cómo se confirma el trabajo | **L:** el cliente dicta un OTP de 4 dígitos y el técnico lo captura; eso cierra y libera. **R, A, C:** el cliente confirma en la aplicación. | El cliente confirma en la aplicación. El OTP de cierre queda fuera. | El OTP exige que quien solicitó esté presente con la aplicación (no funciona cuando un familiar solicita por un adulto mayor) y elimina la ventana para reportar inconformidad antes de liberar. |
| C-6 | Quién fija el precio y cuándo | **R:** el técnico fija el monto dentro del rango al aceptar. **A:** el técnico registra antes su cotización por trabajo; puede salirse del rango con justificación. **L:** el sistema calcula un rango según categoría y gravedad. **C:** precio base o calculado con variables; cotización particular para trabajos especiales. | Trabajo de catálogo: el técnico registra su cotización por adelantado (RF-17) y el cliente la ve junto al rango antes de elegir. Trabajo especial: cotización particular (RF-18). El rango es manual, no algorítmico. RF-05-AC-1, RF-06-AC-3 y AC-4 cambian. | Con la versión de R el cliente elegía técnico sin conocer el precio final. La de A da el precio antes de contratar; la de C cubre lo que no cabe en el catálogo. El cálculo automático no es viable sin datos históricos (RNF-18 lo deja preparado). |
| C-7 | Precio fuera del rango | **R:** se rechaza. **A:** se permite con justificación y aviso al cliente. | Se permite con justificación obligatoria y aviso visible. | Hay trabajos legítimamente más caros; el cliente decide con la información a la vista. |
| C-8 | Ancho del rango | **R:** máximo ≤ 130% del mínimo. **A:** solo mínimo ≤ máximo. **L:** ejemplo de $350 a $500 (143%). | Se conserva la regla de 130%. | Un rango muy amplio no orienta al cliente. El ejemplo de L no se trasladó. |
| C-9 | Rechazo de un costo adicional | **R:** el servicio se cierra y el técnico cobra solo el costo de visita. **A:** el servicio sigue; el técnico termina lo acordado, cobra lo acordado y deja la instalación segura. | Por defecto sigue "en proceso" y el técnico termina lo acordado (RF-09-AC-4). Si declara que lo acordado no puede completarse sin el adicional, se cierra con costo de visita (RF-09-AC-6). | Las dos situaciones existen. Cerrar siempre castiga al técnico que sí puede terminar; continuar siempre ignora que a veces el trabajo es imposible sin el adicional. |
| C-10 | Costo adicional sin respuesta | **R:** el técnico no puede marcar "terminado" mientras esté pendiente. **A:** al marcar "terminado" cuenta como rechazado. | Cuenta como rechazado (RF-09-AC-5). | Evita que el servicio quede bloqueado y cumple el aviso "si no respondes, este costo no se cobra". |
| C-11 | Evidencia del costo adicional | **R:** al menos una foto. **A:** una o dos fotos y descripción. **C:** descripción y evidencia. | Descripción obligatoria y de 1 a 5 fotos (el mismo límite que la solicitud). RF-09-AC-1 y AC-2 cambian. | La descripción la piden A y C. El tope de 2 no tenía justificación; se unificó con el de RF-02. |
| C-12 | Nombres y significado de los estados | **R:** solicitado, aceptado, en camino, en proceso, terminado, en revisión, confirmado, cerrado, cancelado. **A:** usa "Confirmado" para pago recibido y técnico aceptado, "En curso", "En disputa" y "Costo adicional pendiente" como estado. **L:** códigos en inglés (`PENDING_REVIEW`, `ESCROW_LOCKED`, `IN_PROGRESS`, `COMPLETED`). | Una sola tabla con los nombres de R, un código en inglés para la API (aporte de L) y la columna "Pasa a" (aporte de A). "Confirmado" significa que el cliente confirmó el trabajo. El costo adicional pendiente es un estado del costo, no del servicio. | "Confirmado" significaba dos cosas distintas. Los 93 criterios de R ya usan sus nombres, así que conservarlos mantiene los IDs estables. |
| C-13 | El técnico rechaza la solicitud | **R:** regresa a "solicitado" con técnicos alternativos. **A:** pasa a "Cancelado" y el cliente elige otro. | Regresa a "solicitado" (RF-06-AC-5). | El cliente no repite la solicitud. |
| C-14 | Plataforma | **R, C:** aplicación web. **A:** Android e iOS publicadas en tiendas. **L:** web y móvil. | Aplicación web responsiva, mobile-first. Las aplicaciones nativas quedan fuera. | Restricción de 12 semanas. |
| C-15 | Recibo fiscal e impuestos | **L:** comprobante fiscal con IVA y retención automática de ISR/IVA. **R:** sin CFDI. **C:** contabilidad fiscal fuera. | Recibo digital desglosado, no fiscal. Lo fiscal queda fuera y sujeto a revisión legal (RD-15). | Los técnicos con frecuencia no tienen alta fiscal (R, A). |
| C-16 | Disponibilidad | **A:** 99.5% mensual de 8:00 a 20:00. **L:** 99.9% de 7:00 a 22:00. | 99.5% en horario de atención (RNF-15). | El horario coincide con el del soporte humano; 99.9% no es verificable en el proyecto. |
| C-17 | Unidades de tamaño | **R:** 18 px y 48 px. **A:** 18 pt y 48 dp. **L:** 18 puntos y 48 píxeles. | 18 pt (24 px CSS) de texto y 48×48 px CSS de botón, con 8 px de separación (RNF-01). | Mayoría en puntos para el texto; se expresa en unidades web porque el producto es web. |
| C-18 | Alcance de la interfaz sencilla | **R, L:** modo simplificado activable, 3 pantallas. **A:** toda la interfaz es sencilla, 5 pasos. | Modo activable con 3 pantallas para crear la solicitud (RF-15-AC-2) y máximo 5 pantallas de decisión para contratar en cualquier modo (RNF-21). | Miden tramos distintos del flujo; al definir cada tramo dejan de contradecirse. |
| C-19 | Soporte telefónico | **R:** fuera del alcance; solo se muestra el número. **A:** ayuda con personas, mediación y solicitudes por teléfono. | El software incluye el botón de ayuda, el canal de dudas y la captura de solicitudes por un administrador (RF-21, RF-22). La operación del centro de atención es organizacional (R-9). | Se separa lo que es función del sistema de lo que es operación. |
| C-20 | Categorías iniciales | **C:** solo tres. **L:** agrega cerrajería. **A:** agrega cerrajería y gas. | Tres iniciales; el catálogo permite agregar más (RD-16). | Solo las tres están en los cuatro SRS. Gas implica riesgos de seguridad que ningún SRS trata. |
| C-21 | Resoluciones de una inconformidad | **R:** liberar al técnico, reembolso total, reembolso parcial. **A:** nueva visita sin costo, reembolso parcial, reembolso total. | Las cuatro (RF-11-AC-5, 6, 7, 10 y 11). | Son complementarias. |
| C-22 | Cálculo de la reputación | **R:** media aritmética. **L:** "promedio ponderado" y evaluación "bidireccional". | Media aritmética; solo el cliente califica. | L no define los pesos ni la calificación del técnico al cliente. |
| C-23 | Roles | **R, C:** cliente, técnico, administrador. **A:** seis roles (verificación, mediación, soporte, administración). | Tres roles. | Equipo interno reducido; la subdivisión puede hacerse después sin cambiar los criterios. |
| C-24 | Dirección del cliente | **R:** visible para el técnico después de aceptar. **A:** solo de "Confirmado" a "Terminado" y en nuevas visitas. | La ventana de A (RNF-03). | Es la más restrictiva y contiene a la de R. |
| C-25 | Acceso a documentos | **R:** fotos y documentos para cliente, técnico asignado y administradores. **A:** documentos del técnico solo para verificación. | Fotos del servicio: cliente, técnico asignado y administradores. Documentos del técnico: solo administradores (RNF-05). | El cliente no necesita ver la INE del técnico. |
| C-26 | Códigos de error | **R:** 401 y 403. **L:** 400 y 422 para validación, 403 para estado inválido. | 401 sin sesión, 403 sin permiso, 422 validación, 409 estado que no permite la acción. | Un solo código por tipo de error para el `openapi.yaml`. |

---

## 5. Aporte de cada integrante al SRS consolidado

### Raul (A01246414)
- **Estructura base:** numeración RF-01 a RF-16, formato de criterios con ID, convención (P), tabla de estados, anexo de decisiones abiertas y matriz de trazabilidad con casos de uso.
- **93 criterios de aceptación**, de los cuales 69 pasaron sin cambio de fondo. Es la fuente de casi todos los criterios negativos (401, 403, validaciones, transiciones inválidas).
- Catálogo con regla de 130%, solicitudes urgentes, retraso e inasistencia del técnico, solicitud a nombre de un familiar, respaldo por SMS, borrador sin conexión, cobro idempotente y carga concurrente.
- Registro de la validación con el stakeholder real, que decidió los conflictos C-2 y C-3 y motivó la garantía.
- **Cedió en:** inicio de sesión con contraseña, exclusión de antecedentes y de efectivo, monto fijado al aceptar, cierre automático al rechazar un adicional, bloqueo de "terminado" con adicional pendiente.

### Alvaro (A01796929)
- **Mayor número de requerimientos nuevos:** RF-17 (oferta del técnico), RF-19 (cancelación), RF-20 (liberación automática), RF-21 (ayuda y dudas), RF-22 (solicitud por teléfono), RF-23 (suspensión), RF-24 (pagos del técnico), RF-26 (promociones) y RF-27 (indicadores).
- Inicio de sesión sin contraseña, acreditación de experiencia sin certificación, justificación de precios fuera de rango, pago en OXXO y nueva visita como resolución.
- Columna "Pasa a" de la tabla de estados y la regla "sin respuesta: el adicional se rechaza, la confirmación se acepta" (RD-02).
- Requerimientos no funcionales de usabilidad para adultos mayores (RNF-12, RNF-13, RNF-19), retención de fotos, historial inmutable, disponibilidad y horario de atención.
- **Cedió en:** aplicaciones nativas, seis roles, "Confirmado" como nombre de estado, cancelar el servicio cuando el técnico rechaza, tope de dos fotos, gas y cerrajería como categorías iniciales.

### Luis (A01797418)
- Enfoque de contrato para la API: códigos de estado en inglés y códigos HTTP para validación, que se unificaron en las convenciones de la sección 3.1.
- Segundo factor para administradores, WCAG 2.1 AA, protección del teléfono entre cliente y técnico.
- Consistencia aritmética del recibo, actualización de la reputación en 5 segundos, resumen en una pantalla antes del cargo, tiempo de respuesta de la cotización.
- Garantía de 7 días (RF-25, RD-14).
- **Cedió en:** cierre con OTP, cotización algorítmica, comprobante fiscal y retención de impuestos, 99.9% de disponibilidad, reputación ponderada y bidireccional, confirmación por audio.

### Carlos (A01797569)
- **Modelo de dos tipos de trabajo** (catálogo y especial), que cambió el alcance del sistema: RF-18 completo, servicios parametrizables (RF-16-AC-6, RF-02-AC-9, RF-05-AC-6).
- Visita de diagnóstico y solicitud de información adicional (RD-12).
- Acuerdo inicial inmutable como referencia de los cambios posteriores (RF-06-AC-8, RD-11) y trazabilidad (RNF-14).
- Regla de que un error del técnico no se cobra (RF-09-AC-9, RD-10).
- Portafolio del técnico y lista clara de exclusiones del alcance.
- **Cedió en:** prohibición de la liberación automática (queda por validar, A-3).

---

## 6. Elementos no incorporados

| Elemento | Origen | Motivo |
|---|---|---|
| Cierre del servicio con OTP de 4 dígitos | L RF-05 | Conflicto C-5 |
| Cotización algorítmica por gravedad | L RF-02 | Conflicto C-6; sin datos históricos |
| Comprobante fiscal, IVA y retención de ISR/IVA | L RF-06, RD-02 | Conflicto C-15; pasa a RD-15 como pendiente legal |
| Confirmación por audio o llamada automatizada | L RF-08-AC-2 | Fuera de las 12 semanas |
| Evaluación bidireccional | L RF-07 (título) | No tenía criterios |
| Disponibilidad de 99.9% | L RNF-04 | Conflicto C-16 |
| Aplicaciones Android e iOS en tiendas | A RNF-20 | Conflicto C-14 |
| Roles separados de verificación, mediación y soporte | A RNF-19 | Conflicto C-23 |
| Gas y cerrajería como categorías iniciales | A RD-11, L 1.2 | Conflicto C-20; se pueden agregar al catálogo |
| Contraseña para clientes y técnicos | R RF-01 | Conflicto C-1 |
| Exclusión de la verificación de antecedentes | R RD-06 | Conflicto C-2 |
| Prohibición de liberación automática | C RD-05, R RF-10-AC-4 | Conflicto C-4 |

---

## 7. Equivalencia de IDs

### 7.1 Raul → equipo
Los IDs son los mismos (`R RF-XX-AC-N` = `RF-XX-AC-N`). Cambiaron de contenido: RF-01-AC-1, 2, 3, 5, 6; RF-03-AC-2, 3; RF-04-AC-1, 4; RF-05-AC-1; RF-06-AC-3, 4; RF-09-AC-1 a 5; RF-10-AC-1, 4, 5; RF-11-AC-7; RF-13-AC-1, 6; RF-15-AC-5. Los RNF-01 a RNF-10 y RD-01 a RD-06 conservan su número.

### 7.2 Alvaro → equipo

| ID de Alvaro | ID del equipo |
|---|---|
| RF-01 (AC-1, AC-2) | RF-03-AC-2, RF-03-AC-3 |
| RF-02 (AC-1 a AC-3) | RF-03-AC-8 a RF-03-AC-10 |
| RF-03 (AC-1 a AC-3) | RF-03-AC-11, RF-03-AC-4, RF-03-AC-5 y AC-6 |
| RF-04 (AC-1 a AC-4) | RF-02-AC-10, RF-04-AC-1 y AC-3, RF-02-AC-1 y RF-06-AC-1, RF-04-AC-4 |
| RF-05 (AC-1 a AC-3) | RF-16-AC-1, RF-16-AC-2, RF-16-AC-3 |
| RF-06 (AC-1, AC-2) | RF-05-AC-1, RF-05-AC-5 |
| RF-07 (AC-1 a AC-3) | RF-17-AC-3, RF-05-AC-4, RF-17-AC-2 |
| RF-08 (AC-1 a AC-3) | RF-09-AC-1, RF-09-AC-2, RF-09-AC-8 |
| RF-09 (AC-1 a AC-3) | RF-09-AC-7, RF-09-AC-3, RF-09-AC-4 |
| RF-10 (AC-1 a AC-3) | RF-21-AC-4, RF-21-AC-5, RF-21-AC-6 |
| RF-11 (AC-1, AC-2) | RF-09-AC-5, RF-09-AC-10 |
| RF-12 (AC-1, AC-2) | RF-10-AC-4, RF-10-AC-3 |
| RF-13 (AC-1 a AC-4) | RF-20-AC-1 a RF-20-AC-4 |
| RF-14 (AC-1 a AC-3) | RF-11-AC-1, RF-11-AC-2, RF-11-AC-3 |
| RF-15 (AC-1 a AC-4) | RF-11-AC-11, RF-11-AC-6 y AC-8, RF-11-AC-10, RF-11-AC-7 |
| RF-16 (AC-1 a AC-5) | RF-01-AC-5, RF-01-AC-5, RF-01-AC-6, RF-01-AC-6, RF-01-AC-8 |
| RF-17 (AC-1 a AC-3) | RF-08-AC-1, RF-08-AC-5, RF-08-AC-5 |
| RF-18 (AC-1 a AC-3) | RF-21-AC-1, RF-21-AC-2, RF-21-AC-3 |
| RF-19 (AC-1, AC-2) | RF-22-AC-1, RF-22-AC-2 |
| RF-20 (AC-1 a AC-3) | RF-10-AC-5, RF-10-AC-1, RF-10-AC-7 |
| RF-21 (AC-1 a AC-3) | RF-13-AC-2 y AC-6, RF-13-AC-4, RF-04-AC-5 |
| RF-22 (AC-1 a AC-3) | RF-07-AC-6, RF-07-AC-6, RF-07-AC-2 |
| RF-23 (AC-1, AC-2) | RF-13-AC-1, RF-14-AC-3 |
| RF-24 (AC-1 a AC-3) | RF-26-AC-1 a RF-26-AC-3 |
| RF-25 (AC-1, AC-2) | RF-27-AC-1, RF-27-AC-2 |
| RF-26 (AC-1 a AC-3) | RF-01-AC-1, RF-02-AC-8, RF-01-AC-11 |
| RF-27 (AC-1 a AC-3) | RF-17-AC-2, RF-17-AC-3 y AC-4, RF-17-AC-5 |
| RF-28 (AC-1 a AC-4) | RF-17-AC-1, RF-06-AC-3, RF-06-AC-5 (resuelto distinto, C-13), RF-07-AC-1 |
| RF-29 (AC-1, AC-2) | RF-19-AC-1, RF-19-AC-2 |
| RF-30 (AC-1 a AC-3) | RF-23-AC-1, RF-23-AC-3 (resuelto distinto), RF-23-AC-4 |
| RF-31 (AC-1, AC-2) | RF-04-AC-5, RF-04-AC-6 |
| RF-32 (AC-1) | RF-24-AC-1 |

| RNF / RD de Alvaro | Equipo |
|---|---|
| RNF-01, RNF-04 | RNF-01 |
| RNF-02 | RNF-21 |
| RNF-03 | RNF-07 |
| RNF-05, RNF-06 | RNF-12, RNF-13 |
| RNF-07, RNF-18 | RNF-04, RNF-05 |
| RNF-08, RNF-09 | RNF-03, RNF-05 |
| RNF-10 | RNF-17 |
| RNF-11 | RNF-14 |
| RNF-12 | RNF-15 |
| RNF-13, RNF-14 | RNF-16 |
| RNF-15 | RNF-18 |
| RNF-16, RNF-17 | RNF-06 |
| RNF-19 | RF-01-AC-7 (tres roles, C-23) |
| RNF-20 | No incorporado (C-14) |
| RNF-21 | RNF-19 |
| RD-01 | RD-05 |
| RD-02 | RD-15 |
| RD-03 | RD-04 |
| RD-04 | RD-07 |
| RD-05, RD-06 | RD-08 |
| RD-07 | RD-09 |
| RD-08 | RD-02 |
| RD-09 | RD-13 |
| RD-10 | RD-01 |
| RD-11 | RD-16 (solo tres categorías, C-20) |

### 7.3 Luis → equipo

| ID de Luis | ID del equipo |
|---|---|
| RF-01-AC-1, RF-01-AC-2 | RF-03-AC-1, RF-03-AC-4 |
| RF-02-AC-1, RF-02-AC-2 | RF-05-AC-1, RF-02-AC-2 y RF-02-AC-9 |
| RF-03-AC-1, RF-03-AC-2 | RF-10-AC-1, RF-10-AC-2 |
| RF-04-AC-1, RF-04-AC-2 | RF-09-AC-1 y AC-7, RF-09-AC-2 |
| RF-05-AC-1, RF-05-AC-2 | No incorporados (C-5); la liberación está en RF-10-AC-3 |
| RF-06-AC-1, RF-06-AC-2 | RF-13-AC-1 y RF-14-AC-3, RF-13-AC-7 |
| RF-07-AC-1, RF-07-AC-2 | RF-13-AC-6, RF-13-AC-5 |
| RF-08-AC-1, RF-08-AC-2 | RNF-01 y RNF-11, RF-15-AC-6 |
| RNF-01 | RNF-04, RF-01-AC-10 |
| RNF-02 | RNF-11 |
| RNF-03 | RNF-06 |
| RNF-04 | RNF-15 (con el valor de C-16) |
| RD-01 | RD-14, RF-25 |
| RD-02 | RD-15 (fuera de esta versión) |

### 7.4 Carlos → equipo

| ID de Carlos | ID del equipo |
|---|---|
| RF-01-AC-1, RF-01-AC-2 | RF-01-AC-1 y AC-2, RF-01-AC-7 |
| RF-02-AC-1, RF-02-AC-2 | RF-02-AC-10, RF-02-AC-1 y RF-18-AC-1 |
| RF-03-AC-1, RF-03-AC-2 | RF-05-AC-6, RF-02-AC-9 |
| RF-04-AC-1, RF-04-AC-2 | RF-18-AC-1, RF-18-AC-2 |
| RF-05-AC-1, RF-05-AC-2 | RF-18-AC-3 (ambos) |
| RF-06-AC-1, RF-06-AC-2 | RF-04-AC-5, RF-03-AC-1 |
| RF-07-AC-1, RF-07-AC-2 | RF-18-AC-4, RF-18-AC-5 y RF-06-AC-8 |
| RF-08-AC-1 a AC-3 | RF-09-AC-1, RF-09-AC-3 a AC-5, RF-09-AC-9 |
| RF-09-AC-1 a AC-4 | RF-07-AC-6, RF-10-AC-3, RF-14-AC-3, RF-13-AC-2 |
| RNF-01, RNF-02 | RNF-12 y RNF-21, RNF-09 |
| RNF-03 | RF-09-AC-11, RF-10-AC-6, RNF-04 |
| RNF-04 | RNF-14 |
| RD-01 | RD-02 |
| RD-02 | RD-10 |
| RD-03 | RD-12 |
| RD-04 | RD-01 |
| RD-05 | RD-02 (con liberación a las 72 h, C-4) |
| RD-06 | RD-11 |

---

## 8. Resumen

| Concepto | Cantidad |
|---|---|
| Temas en consenso de los cuatro SRS | 12 |
| Temas en consenso de tres SRS | 7 |
| Gaps incorporados | 42 |
| Conflictos resueltos | 26 |
| Elementos no incorporados | 12 |
| Decisiones que siguen abiertas (Anexo A del SRS del equipo) | 14 |
| Criterios de aceptación del SRS del equipo | 168 (69 sin cambio, 24 ajustados, 75 nuevos) |
