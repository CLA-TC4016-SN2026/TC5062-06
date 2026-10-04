# Definition of Done (DoD) — FairFix

**Fuente:** `sprint1_planning.md` y `tareas_tecnicas.md`
**Estado:** DoD justificada y ajustada para el proyecto
**Aplica desde:** Sprint 1
**Contexto:** Proyecto de curso, roles asumidos por el estudiante (backend, frontend, QA, DevOps), sin pipeline de CI/CD completo todavía

---

## 1. Propósito y alcance

La DoD define cuándo una **historia de usuario** se considera verdaderamente terminada y puede contar para la velocidad del sprint. Aplica por igual a todas las historias (HU-01, HU-04, HU-02a y las siguientes).

Se distinguen dos niveles:

| Nivel | Qué lo define | Dónde está |
|---|---|---|
| **Tarea técnica** (TT-xx-xx) | El criterio de *Done* propio de cada tarea | `tareas_tecnicas.md` |
| **Historia de usuario** (HU-xx) | Esta DoD, además de sus criterios de aceptación | Este documento |

Una historia no está terminada solo porque todas sus tareas lo estén: además debe cumplir cada punto de esta lista. Si falta uno, la historia no se mueve a **Done** en GitHub Projects y sus story points no cuentan.

---

## 2. Criterios

### 2.1 Funcionalidad
- [ ] Todos los criterios de aceptación de la historia están aprobados (por ejemplo, HU-01-CA-1 a CA-4), en la versión aceptada por el equipo.
- [ ] La funcionalidad se probó de punta a punta: frontend → backend → base de datos → resultado visible en pantalla.
- [ ] Los errores esperados muestran un mensaje claro al usuario (código incorrecto, rango de precio inválido, documento faltante) y nunca un error sin control.

### 2.2 Código
- [ ] El código está en GitHub, desarrollado en una rama propia y fusionado a `main` mediante Pull Request.
- [ ] Los commits y el PR hacen referencia al issue de la historia o de la tarea (por ejemplo, `TT-01-04`).
- [ ] No hay credenciales, claves ni datos personales reales en el repositorio; la configuración sensible va en variables de entorno.
- [ ] No queda código de depuración (`print`, `console.log`) ni código comentado sin uso.

### 2.3 Pruebas
- [ ] Las tareas de QA de la historia están completas (TT-01-08, TT-04-06, TT-02A-07 o equivalentes).
- [ ] Cada endpoint nuevo tiene al menos una prueba automatizada del caso exitoso y una del caso de error.
- [ ] Se prueban los **permisos por rol**: un usuario sin el rol adecuado recibe acceso denegado.
- [ ] Todas las pruebas, nuevas y anteriores, pasan en local antes del merge (se ejecutan manualmente mientras no haya CI).

### 2.4 Seguridad y datos
- [ ] Los documentos del técnico solo son accesibles por su dueño y por el administrador.
- [ ] Los códigos de acceso solo aparecen en el registro de pruebas mientras el envío esté simulado, nunca en respuestas de la API.
- [ ] Los cambios de base de datos se aplican con un script o migración reproducible, no con cambios manuales.

### 2.5 Despliegue
- [ ] La historia funciona en el **ambiente desplegado** usado para la Sprint Review, no solo en local.
- [ ] El despliegue puede hacerse de forma manual mientras no exista CI/CD, siguiendo los pasos documentados en el `README.md`.
- [ ] Los datos de demostración necesarios están cargados (categorías, trabajos de ejemplo, cuenta de administrador).

### 2.6 Documentación
- [ ] Los endpoints nuevos tienen descripción y ejemplos en la documentación de la API.
- [ ] El `README.md` está actualizado si cambió la instalación, la configuración o el despliegue.
- [ ] Toda funcionalidad **simulada** (por ejemplo, el envío de WhatsApp/SMS) está marcada como tal en el código y en el README, con la historia que la reemplazará (HU-09).

### 2.7 Revisión y gestión
- [ ] El PR se revisó contra esta lista antes del merge: por otro integrante si es posible o, si no, como auto-revisión con apoyo del agente de IA.
- [ ] Todas las sub-issues de la historia (TT-xx-xx) están cerradas en GitHub Projects.
- [ ] La historia se mostró funcionando en la Sprint Review.

---

## 3. Ajustes respecto a la propuesta del agente

La propuesta inicial del agente seguía una DoD estándar de equipo profesional. Se ajustó al contexto del proyecto:

| Propuesta estándar | Ajuste | Motivo |
|---|---|---|
| Pipeline de CI que ejecuta pruebas en cada push | Pruebas ejecutadas manualmente en local antes del merge | Aún no hay CI/CD configurado |
| Despliegue automático a staging | Despliegue manual documentado en el README | El Sprint Goal exige un ambiente desplegado, pero no hay pipeline |
| Code review obligatorio por otra persona | Revisión por otro integrante cuando sea posible; si no, auto-revisión con checklist y apoyo del agente | Los roles los asume el estudiante |
| Cobertura mínima de pruebas (por ejemplo, 80%) | Caso exitoso y caso de error por endpoint, más permisos por rol | Meta realista con 8-9 h por semana |
| Integraciones externas reales | Se aceptan simulaciones documentadas | Riesgo R-1: proveedor de WhatsApp/SMS sin elegir |
| Pruebas automatizadas de frontend | Prueba manual de punta a punta | Se prioriza el backend por tiempo |

---

## 4. Decisiones pendientes que afectan la DoD

- **R-1 (envío simulado del código):** si el equipo exige envío real, se elimina la excepción de simulación del punto 2.6.
- **R-2 (cuenta de administrador creada en base de datos):** se acepta como dato de demostración (punto 2.5) hasta que exista la historia de acceso del administrador.
- **Ajustes 9, 10 y 11 de HU-02a:** la historia se evalúa contra los criterios de aceptación que el equipo apruebe, no contra la propuesta sin validar.

---

## 5. Revisión

La DoD se revisa en cada retrospectiva. Cuando se configure GitHub Actions, el punto 2.3 cambia a: *"Todas las pruebas pasan en el pipeline de CI antes del merge"*, y el punto 2.5 incorporará el despliegue automático.
