# Sprint 1 Planning — FairFix

**Fuente:** `backlog_completo.md` (18 historias, 105 puntos)
**Fecha:** 4 de octubre de 2026
**Duración del sprint:** 2 semanas
**Equipo:** 4 integrantes, 8 a 9 horas por semana cada uno

---

## 1. Sprint goal

> **Al cierre del sprint, un cliente y un técnico pueden registrarse e iniciar sesión con su teléfono y un código, un técnico puede enviar sus documentos a revisión y un administrador puede dar de alta el catálogo de trabajos con su rango de precios, demostrado en un ambiente desplegado con los 10 criterios de aceptación de HU-01, HU-04 y HU-02a aprobados.**

Es la base de la que depende todo el flujo: sin cuentas, roles y catálogo no se puede construir la solicitud, la cotización ni el pago.

| Característica | Cómo la cumple el goal |
|---|---|
| Concreto | Nombra tres capacidades que un usuario puede ejecutar: entrar a la plataforma, enviar documentos como técnico y administrar el catálogo. |
| Medible | 10 criterios de aceptación aprobados y 13 story points terminados, demostrados en un ambiente desplegado. |
| Alcanzable | 13 puntos comprometidos contra una capacidad estimada de 14 (sección 2). |

---

## 2. Capacidad estimada del equipo: 14 story points

**Supuesto:** las 8 a 9 horas semanales son por integrante y el equipo son los 4 integrantes del SRS. Si fueran horas del equipo completo, la capacidad bajaría a unos 4 puntos y solo cabría HU-04 o HU-01.

| Concepto | Cálculo | Horas |
|---|---|---|
| Horas brutas | 4 personas × 8.5 h × 2 semanas | 68 |
| Ceremonias (planning, dailies, review, retrospectiva) | ≈ 15% | −10 |
| Arranque del proyecto (repositorio, base de datos, despliegue, esqueleto de backend y frontend) | Solo ocurre en el sprint 1 | −10 |
| **Horas efectivas para historias** | | **48** |


**Capacidad:** 48 horas ÷ 3.5 horas por punto ≈ **14 story points**.

Este valor es un supuesto inicial. Se corrige con la velocidad real al cerrar el sprint.

---

## 3. Historias seleccionadas: 13 puntos comprometidos

| Historia | Épica | Prioridad | Puntos | Criterios | Por qué entra |
|---|---|---|---|---|---|
| HU-01 Inicio de sesión sin contraseña | EP-01 | Alta | 5 | 4 | Es la primera en el orden de dependencias; crea las cuentas y los roles que usan todas las demás historias. |
| HU-04 Catálogo de trabajos y precios de referencia | EP-02 | Alta | 3 | 3 | También es prerrequisito: sin catálogo no hay solicitudes ni cotizaciones. Es pequeña y sirve para montar el patrón de alta y edición con permisos por rol. |
| HU-02a Enviar documentos y acreditar experiencia | EP-01 | Alta | 5 | 3 | Segundo paso del orden de dependencias; incluye la carga de archivos, que se reutiliza después en las fotos de solicitudes y de costos adicionales. |
| **Total comprometido** | | | **13** | **10** | Deja 1 punto de margen sobre la capacidad. |

### Historia adicional si sobra tiempo

| Historia | Puntos | Por qué no se compromete |
|---|---|---|
| HU-02b Validar a un técnico | 3 | Completa el ciclo del técnico, pero con ella el sprint sube a 16 puntos y rebasa la capacidad de 14. |

### Historias que no entran y por qué

| Historia | Motivo |
|---|---|
| HU-03 Disponibilidad y cotizaciones del técnico | Necesita técnicos validados, es decir, HU-02b. |
| HU-05, HU-06, HU-07 Solicitud, consulta y contratación | Necesitan el catálogo y técnicos con oferta registrada. |
| HU-11a Pagar y retener el pago | Es la de mayor riesgo técnico por la pasarela; conviene empezarla con la base ya estable. |
| Resto del backlog | Depende de las anteriores según el orden sugerido en `backlog_completo.md`. |

---

## 4. Cómo se mide el cumplimiento

| Indicador | Meta |
|---|---|
| Criterios de aceptación aprobados | 10 de 10 (HU-01-CA-1 a CA-4, HU-04-CA-1 a CA-3, HU-02a-CA-1 a CA-3) |
| Story points terminados | 13 |
| Demostración en la review | Recorrido completo en el ambiente desplegado: registro e inicio de sesión de un cliente, registro de un técnico con envío de documentos y alta de un trabajo en el catálogo |

Los puntos terminados en este sprint serán la velocidad base para planear el sprint 2.

---

## 5. Riesgos y decisiones previas

| # | Riesgo | Propuesta | Estado |
|---|---|---|---|
| R-1 | El envío del código por WhatsApp y SMS depende de un proveedor que aún no está elegido (decisión abierta A-8 del SRS). | Simular el envío en este sprint: el código se muestra en un registro de pruebas. La integración real queda para HU-09. Si el equipo exige envío real, HU-01 ya no cabe en 5 puntos. | Por decidir |
| R-2 | HU-04 necesita un administrador con sesión, y ninguna historia del backlog cubre su acceso con correo, contraseña y segundo factor (RF-01-AC-10). | Crear la cuenta de administrador directamente en la base de datos para este sprint y agregar esa historia al backlog. | Por decidir |
| R-3 | La capacidad de 14 puntos no tiene respaldo histórico. | Revisar el avance a mitad del sprint; si va atrasado, HU-02a es la historia que se recorta, porque HU-01 y HU-04 son las que desbloquean el sprint 2. | Aceptado |
