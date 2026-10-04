# Ajustes al backlog — FairFix

**Backlog ajustado:** `backlog_completo.md` (antes 16 historias y 100 puntos; ahora 18 historias y 105 puntos)
**Fecha de la revisión:** 3 de octubre de 2026
**Estado:** revisión cerrada; los 6 ajustes ya están aplicados en `backlog_completo.md`

## Propósito

Este documento registra los cambios que el equipo decide hacer al backlog generado a partir de `SRS_equipo.md`: redacción de historias, estimaciones, prioridades y criterios de aceptación. Cada ajuste indica qué había, qué queda y por qué.

Los seis ajustes ya están aplicados en `backlog_completo.md`. La versión original de cada historia modificada se conserva aquí, en el bloque "Antes" de cada ajuste.

## Cómo leerlo

- **Sección 1:** ajustes aplicados, con el antes, el después y el motivo.
- **Sección 2:** efecto acumulado en historias y puntos.

Tipos de ajuste: **Redacción** (formato Como/quiero/para), **Estimación** (story points), **Prioridad**, **Criterios** (criterios de aceptación), **Estructura** (dividir, fusionar, agregar o quitar historias).

Las historias que resultan de una división llevan el ID original con sufijo (`HU-02a`, `HU-02b`) para no renumerar el resto del backlog.

---

## 1. Ajustes aplicados

### AJ-01: Dividir HU-11 (pago retenido)

- **Tipo:** Estructura y estimación
- **Motivo:** 13 puntos es el máximo de la escala y la historia mezclaba la integración con la pasarela (lo de mayor riesgo técnico) con la confirmación del trabajo. Separadas, cada una cabe en un sprint y la segunda puede probarse sin depender de que la integración esté terminada.

**Antes**

> **HU-11: Pago retenido hasta confirmar el trabajo** — Alta · 13 puntos
> Como cliente, quiero pagar con tarjeta, transferencia o en OXXO y que el dinero quede retenido hasta que yo confirme, para no pagar por un trabajo que no se hizo bien.
> Criterios: HU-11-CA-1 a HU-11-CA-4.

**Después**

> **HU-11a: Pagar y retener el pago** — Alta · 8 puntos · Origen: RF-10, RNF-04, RNF-08
> **Como** cliente, **quiero** pagar con tarjeta, transferencia o en OXXO al contratar, **para** dejar el servicio asegurado sin que el técnico reciba el dinero todavía.
> - **HU-11a-CA-1** (antes HU-11-CA-1; RF-10-AC-1, RF-10-AC-5): Dado que el técnico aceptó, cuando autorizo el pago con tarjeta, transferencia u OXXO y la pasarela confirma, entonces queda retenido el monto más la comisión y el servicio pasa a "aceptado"; cualquier otro método se rechaza.
> - **HU-11a-CA-2** (antes HU-11-CA-2; RF-10-AC-2, RF-10-AC-7): Dado que la pasarela rechaza el pago o todavía no lo reporta como recibido, cuando consulto el servicio, entonces sigue en "solicitado", no hay cobro registrado y veo el error o la referencia pendiente.
> - **HU-11a-CA-3** (antes HU-11-CA-4; RNF-08): Dado que una operación de cobro se envía dos veces, cuando se procesa, entonces se registra un solo cargo.

> **HU-11b: Confirmar el trabajo y liberar el pago** — Alta · 5 puntos · Origen: RF-10
> **Como** cliente, **quiero** confirmar que el trabajo quedó bien para que se le pague al técnico, **para** que el dinero solo salga cuando yo esté conforme.
> - **HU-11b-CA-1** (antes HU-11-CA-3; RF-10-AC-3): Dado que el servicio está "terminado" sin inconformidad, cuando lo confirmo, entonces pasa a "confirmado" y se libera al técnico el monto más los costos adicionales aprobados; la comisión queda para la plataforma.
> - **HU-11b-CA-2** (nuevo; RF-10-AC-6): Dado que el servicio "terminado" es de otro cliente, cuando un usuario distinto del dueño intenta confirmarlo, entonces el sistema responde 403 y no libera el pago.
> - **HU-11b-CA-3** (nuevo; RF-10-AC-4): Dado que el servicio está "terminado", cuando no lo confirmo ni reporto inconformidad y no han pasado 72 horas, entonces el pago sigue retenido.

- **Efecto:** +1 historia; puntos sin cambio (13 = 8 + 5). El criterio HU-11-CA-3 se separó en dos (HU-11b-CA-1 y CA-2) y se agregó HU-11b-CA-3 para que la historia tenga un caso de "todavía no se libera".

### AJ-02: Dividir HU-02 (registro y validación del técnico)

- **Tipo:** Estructura y redacción
- **Motivo:** la historia decía "como técnico", pero tres de sus cinco criterios eran acciones del administrador. Una historia debe tener un solo tipo de usuario para que el "quiero" y el "para" tengan sentido.

**Antes**

> **HU-02: Registro y validación del técnico** — Alta · 8 puntos
> Como técnico, quiero enviar mis documentos y acreditar mi experiencia aunque no tenga certificación, para que la plataforma me valide y pueda recibir solicitudes.
> Criterios: HU-02-CA-1 a HU-02-CA-5.

**Después**

> **HU-02a: Enviar documentos y acreditar experiencia** — Alta · 5 puntos · Origen: RF-01, RF-03
> **Como** técnico, **quiero** enviar mis documentos y acreditar mi experiencia aunque no tenga certificación, **para** que la plataforma me valide y pueda recibir solicitudes.
> - **HU-02a-CA-1** (antes HU-02-CA-1; RF-03-AC-2): Dado que me falta alguno de los cuatro documentos (INE, selfie, comprobante de domicilio, carta de no antecedentes), cuando intento enviarlos a revisión, entonces el sistema no los envía y me dice cuál falta.
> - **HU-02a-CA-2** (antes HU-02-CA-2; RF-03-AC-3, RF-03-AC-8, RF-03-AC-9): Dado que cargué los cuatro documentos, cuando acredito mi experiencia con una certificación, o con al menos una foto de un trabajo anterior y dos referencias, entonces mi estado cambia a "en revisión".
> - **HU-02a-CA-3** (antes parte de HU-02-CA-5; RF-03-AC-1): Dado que mi estado es "sin validar", "en revisión" o "rechazado", cuando un cliente consulta técnicos, entonces no aparezco en los resultados.

> **HU-02b: Validar a un técnico** — Alta · 3 puntos · Origen: RF-03
> **Como** administrador, **quiero** revisar los documentos de un técnico y aprobarlo o rechazarlo con un motivo, **para** que solo personas verificadas entren a las casas de los clientes.
> - **HU-02b-CA-1** (antes HU-02-CA-3; RF-03-AC-11, RF-03-AC-4): Dado que un técnico está "en revisión", cuando abro su solicitud y la apruebo, entonces veo todos sus documentos, fotos y referencias, y su estado cambia a "validado".
> - **HU-02b-CA-2** (antes HU-02-CA-4; RF-03-AC-5, RF-03-AC-6): Dado que un técnico está "en revisión", cuando lo rechazo, entonces el sistema me exige un motivo, el técnico lo recibe y puede volver a enviar documentos.
> - **HU-02b-CA-3** (antes parte de HU-02-CA-5; RF-03-AC-7): Dado que un usuario no es administrador, cuando intenta aprobar o rechazar documentos, entonces el sistema responde 403.

- **Efecto:** +1 historia; puntos sin cambio (8 = 5 + 3). HU-02-CA-5 se separó en dos criterios, uno por historia.

### AJ-03: Subir la estimación de HU-09 (avisos por WhatsApp)

- **Tipo:** Estimación
- **Motivo:** el proveedor de WhatsApp aún no está elegido (decisión abierta A-8 del SRS) y requiere un proceso de aprobación externo; además la historia incluye el respaldo por SMS y la confirmación de entrega. La incertidumbre justifica el valor más alto de la escala.

| | Antes | Después |
|---|---|---|
| Estimación de HU-09 | 8 | 13 |

- **Efecto:** +5 puntos. Redacción y criterios sin cambio.

### AJ-04: Bajar la prioridad de HU-13 (inconformidad)

- **Tipo:** Prioridad
- **Motivo:** con 14 historias en Alta la prioridad no ayudaba a ordenar. El flujo básico de un servicio (solicitar, contratar, pagar, ejecutar, confirmar) puede demostrarse sin inconformidades; mientras tanto el pago sigue protegido porque no se libera sin confirmación del cliente (HU-11b). La resolución de inconformidades entra después del flujo básico.

| | Antes | Después |
|---|---|---|
| Prioridad de HU-13 | Alta | Media |

- **Efecto:** estimación (8), redacción y criterios sin cambio. Las demás historias conservan su prioridad. En el SRS, RF-11 sigue con prioridad Alta; la diferencia es una decisión de planeación del backlog.

### AJ-05: Anotar la dependencia de HU-12 (liberación automática)

- **Tipo:** Prioridad (nota)
- **Motivo:** el plazo de 72 horas es una decisión que el SRS deja por validar con el stakeholder (Anexo A, A-3), porque dos SRS individuales pedían no liberar el pago sin confirmación del cliente.

| | Antes | Después |
|---|---|---|
| Prioridad de HU-12 | Media | Media |
| Nota | — | "Depende de validar con el stakeholder el plazo de 72 horas (decisión abierta A-3 del SRS). No debe entrar a un sprint antes de esa validación." |

- **Efecto:** sin cambio de prioridad, estimación ni criterios; se agrega la nota de dependencia.

### AJ-06: Reescribir HU-08 (estado del servicio)

- **Tipo:** Redacción y criterios
- **Motivo:** la historia es del técnico, pero su tercer criterio estaba escrito desde lo que hace el cliente. Se mantiene una sola historia y se reescriben el beneficio y ese criterio desde el punto de vista del técnico, dejando explícito qué ve el cliente como resultado.

**Antes**

> **HU-08: Actualizar y consultar el estado del servicio** — Alta · 3 puntos
> Como técnico, quiero marcar con uno o dos toques que voy en camino, que estoy trabajando y que terminé, para que el cliente sepa en todo momento cómo va su servicio.
> - HU-08-CA-3 (RF-07-AC-5, RF-07-AC-6): Dado que hubo cambios de estado, cuando el cliente consulta su servicio, entonces ve el estado actual y la lista de cambios con fecha y hora.

**Después**

> **HU-08: Actualizar el estado del servicio** — Alta · 3 puntos · Origen: RF-07, RNF-02
> **Como** técnico, **quiero** marcar con uno o dos toques que voy en camino, que estoy trabajando y que terminé, **para** que el cliente vea el avance de su servicio sin tener que llamarme.
> - HU-08-CA-1 y HU-08-CA-2: sin cambio.
> - **HU-08-CA-3** (RF-07-AC-5, RF-07-AC-6): Dado que soy el técnico asignado, cuando cambio el estado del servicio, entonces el cambio queda registrado con fecha, hora y usuario, y el cliente dueño del servicio ve el nuevo estado y la lista de cambios al consultarlo.

- **Efecto:** cambia el título, el "para" y la redacción de CA-3. Estimación y prioridad sin cambio.

### Resumen de ajustes aplicados

| # | Historia | Tipo | Antes | Después | Decidido por |
|---|---|---|---|---|---|
| AJ-01 | HU-11 | Estructura / Estimación | 1 historia de 13 puntos | HU-11a (8) y HU-11b (5) | Equipo |
| AJ-02 | HU-02 | Estructura / Redacción | 1 historia de 8 puntos con dos usuarios | HU-02a técnico (5) y HU-02b administrador (3) | Equipo |
| AJ-03 | HU-09 | Estimación | 8 puntos | 13 puntos | Equipo |
| AJ-04 | HU-13 | Prioridad | Alta | Media | Equipo |
| AJ-05 | HU-12 | Prioridad (nota) | Media, sin nota | Media, con dependencia de A-3 | Equipo |
| AJ-06 | HU-08 | Redacción / Criterios | Criterio CA-3 desde el cliente | Título, beneficio y CA-3 desde el técnico | Equipo |

---

## 2. Efecto acumulado

| Concepto | Backlog original | Con ajustes |
|---|---|---|
| Historias | 16 | 18 |
| Story points | 100 | 105 |
| Prioridad Alta / Media / Baja | 14 / 2 / 0 | 15 / 3 / 0 |

| Épica | Puntos originales | Puntos con ajustes |
|---|---|---|
| EP-01 Cuentas y técnicos verificados | 18 | 18 |
| EP-02 Solicitud, cotización y contratación | 18 | 18 |
| EP-03 Ejecución del servicio | 19 | 24 |
| EP-04 Pago, cierre y reputación | 32 | 32 |
| EP-05 Accesibilidad y ayuda | 13 | 13 |
| **Total** | **100** | **105** |

