# Tareas técnicas — Sprint 1 — FairFix
**Estado:** Propuesta revisada para validación del equipo  
**Fuente:** `sprint1_planning.md` y `backlog_completo.md`  
**Sprint:** 1  
**Duración:** 2 semanas  
**Historias comprometidas:** HU-01, HU-04 y HU-02a  
**Capacidad efectiva estimada:** 48 horas  

---

## 1. Criterio de descomposición

Esta propuesta toma las historias seleccionadas para el Sprint 1 y las divide en tareas técnicas concretas.

Cada tarea:
- puede completarse en 4 horas o menos;
- tiene un rol responsable;
- tiene una estimación en horas;
- tiene un criterio de **Done** verificable.

La propuesta también incorpora los ajustes realizados durante la revisión humana del backlog técnico. El objetivo es mantener un MVP sencillo, pero suficientemente completo para demostrar el flujo de registro, catálogo y envío de documentos del técnico.

---

# 2. HU-01 — Inicio de sesión sin contraseña

**Story Points:** 5  
**Estimación técnica:** 18 h

### TT-01-01 — Crear modelo de usuarios, roles y códigos de acceso
- **Rol responsable:** Backend / Database Dev
- **Estimación:** 2 h
- **Done:** Existen estructuras para usuario, teléfono, rol, código temporal, expiración, intentos fallidos y bloqueo, con migración o script ejecutable.

### TT-01-02 — Implementar registro de cliente y técnico
- **Rol responsable:** Backend Dev
- **Estimación:** 2.5 h
- **Done:** Un usuario nuevo puede registrarse como:
  - **Cliente:** nombre completo, dirección y el mismo teléfono usado para el acceso.
  - **Técnico:** nombre completo, especialidad y años de experiencia.
  El rol queda guardado correctamente.

### TT-01-03 — Crear solicitud de código con envío simulado
- **Rol responsable:** Backend Dev
- **Estimación:** 2 h
- **Done:** El sistema recibe un teléfono válido, genera un código temporal y lo registra en un log o entorno de pruebas para simular el envío.

### TT-01-04 — Verificar código y abrir sesión
- **Rol responsable:** Backend Dev
- **Estimación:** 3 h
- **Done:** Un código correcto y vigente abre la sesión. Un código incorrecto o vencido responde 401 con el mismo mensaje y permite solicitar un código nuevo.

### TT-01-05 — Implementar bloqueo por intentos fallidos
- **Rol responsable:** Backend Dev
- **Estimación:** 1.5 h
- **Done:** Después de 5 códigos incorrectos consecutivos, el teléfono queda bloqueado durante 15 minutos.

### TT-01-06 — Simular lógica de WhatsApp y respaldo por SMS
- **Rol responsable:** Backend Dev
- **Estimación:** 1.5 h
- **Done:** En pruebas puede simularse un envío por WhatsApp y, si se marca como no entregado, registrar un segundo evento como respaldo por SMS. No se integra todavía un proveedor externo real.

### TT-01-07 — Crear interfaz de acceso separada para cliente y técnico
- **Rol responsable:** Frontend Dev
- **Estimación:** 3.5 h
- **Done:** La pantalla inicial muestra las opciones **“Soy cliente”** y **“Soy técnico”**. Ambas utilizan el mismo mecanismo de teléfono + código, pero llevan al formulario de registro correspondiente cuando la cuenta no existe.

### TT-01-08 — Probar autenticación, bloqueo y permisos por rol
- **Rol responsable:** QA
- **Estimación:** 2 h
- **Done:** Existen pruebas para código correcto, incorrecto, vencido, bloqueo por 5 intentos, simulación de respaldo por SMS y restricción de acceso según el rol.

---

# 3. HU-04 — Catálogo de trabajos y precios de referencia

**Story Points:** 3  
**Estimación técnica:** 11.5 h

### TT-04-01 — Precargar categorías y trabajos de ejemplo
- **Rol responsable:** Backend / Database Dev
- **Estimación:** 1.5 h
- **Done:** La base de datos inicia con las categorías **Plomería, Electricidad y Carpintería** y al menos un trabajo de ejemplo por categoría.

### TT-04-02 — Crear modelo de datos del catálogo
- **Rol responsable:** Backend / Database Dev
- **Estimación:** 1.5 h
- **Done:** Cada trabajo guarda categoría, nombre, precio mínimo, precio máximo, costo de visita y estado activo/inactivo.

### TT-04-03 — Implementar API para crear y editar trabajos
- **Rol responsable:** Backend Dev
- **Estimación:** 3 h
- **Done:** Un administrador puede crear y editar trabajos. El backend rechaza un mínimo menor o igual a cero y un máximo mayor a 1.3 veces el mínimo.

### TT-04-04 — Implementar desactivación y conservación de valores previos
- **Rol responsable:** Backend Dev
- **Estimación:** 2 h
- **Done:** Un trabajo puede desactivarse sin eliminarse. Los datos ya asociados a operaciones previas conservan sus valores.

### TT-04-05 — Crear interfaz administrativa del catálogo
- **Rol responsable:** Frontend Dev
- **Estimación:** 2.5 h
- **Done:** El administrador puede listar, crear, editar y desactivar trabajos dentro de las tres categorías precargadas y recibe mensajes claros cuando un valor es inválido.

### TT-04-06 — Probar CRUD, reglas de precio y permisos
- **Rol responsable:** QA
- **Estimación:** 1 h
- **Done:** Existen pruebas para alta, edición, desactivación, rango inválido y acceso denegado a usuarios que no sean administradores.

---

# 4. HU-02a — Enviar documentos y acreditar experiencia

**Story Points:** 5  
**Estimación técnica:** 18 h

### TT-02A-01 — Configurar almacenamiento seguro de archivos
- **Rol responsable:** Backend / DevOps
- **Estimación:** 1.5 h
- **Done:** Existe un almacenamiento de pruebas con nombres únicos y acceso restringido para los documentos del técnico.

### TT-02A-02 — Implementar carga de documentos obligatorios
- **Rol responsable:** Backend Dev
- **Estimación:** 2.5 h
- **Done:** El técnico puede cargar INE, selfie, comprobante de domicilio y carta de no antecedentes, y todos quedan asociados a su cuenta.

### TT-02A-03 — Registrar evidencia de experiencia reforzada
- **Rol responsable:** Backend Dev
- **Estimación:** 2 h
- **Done:** Si el técnico no cuenta con certificación formal, el sistema exige **3 referencias con nombre y teléfono** y evidencia de **3 trabajos realizados** antes de permitir continuar.

### TT-02A-04 — Crear prototipo de prevalidación automática
- **Rol responsable:** Backend / AI Dev
- **Estimación:** 4 h
- **Done:** Existe un prototipo que intenta comprobar legibilidad y presencia de información básica mediante extracción de texto u otra validación automática disponible. La prevalidación solo funciona como filtro; no sustituye la aprobación final del administrador.

### TT-02A-05 — Bloquear el envío cuando la prevalidación falle
- **Rol responsable:** Backend Dev
- **Estimación:** 2 h
- **Done:** Si falta un documento, una referencia, evidencia de trabajo o la prevalidación detecta un problema, el expediente no puede enviarse a revisión y se devuelve un mensaje indicando qué debe corregirse.

### TT-02A-06 — Crear interfaz de documentos, experiencia y prevalidación
- **Rol responsable:** Frontend Dev
- **Estimación:** 3.5 h
- **Done:** El técnico puede cargar los cuatro documentos, certificación o evidencia alternativa, tres referencias y tres trabajos realizados. La interfaz muestra qué elemento falta o qué documento debe corregirse.

### TT-02A-07 — Probar expediente técnico y transición a revisión
- **Rol responsable:** QA
- **Estimación:** 2.5 h
- **Done:** Existen pruebas para expediente incompleto, certificación válida, alternativa con 3 referencias + 3 trabajos, prevalidación fallida, corrección del expediente y cambio final a estado **“en revisión”**.

---

# 5. Resumen de carga técnica

| Historia | Story Points | Horas estimadas | Tareas |
|---|---:|---:|---:|
| HU-01 | 5 | 18 h | 8 |
| HU-04 | 3 | 11.5 h | 6 |
| HU-02a | 5 | 18 h | 7 |
| **Total** | **13** | **47.5 h** | **21** |

La propuesta utiliza **47.5 de las 48 horas efectivas** calculadas para el Sprint 1. El margen es pequeño, por lo que el equipo debe revisar el avance a mitad del sprint y recortar primero tareas de prototipo o refinamiento antes de comprometer horas adicionales.

---

# 6. Ajustes realizados durante la revisión humana

Los siguientes puntos modifican o aterrizan la propuesta inicial del agente:

1. **Autenticación sencilla para el piloto.**  
   Se conserva teléfono + código, pero el envío se simula en lugar de integrar desde ahora un proveedor real de WhatsApp/SMS.

2. **Se mantiene el bloqueo de seguridad.**  
   Después de 5 códigos incorrectos consecutivos, el teléfono se bloquea durante 15 minutos.

3. **Acceso visual separado por rol.**  
   Se muestran las opciones “Soy cliente” y “Soy técnico”, aunque ambos utilizan el mismo mecanismo de autenticación.

4. **Registro inicial del técnico.**  
   Se capturan nombre completo, especialidad y años de experiencia antes del flujo de documentos.

5. **Registro inicial del cliente.**  
   Se capturan nombre completo, dirección y el mismo teléfono utilizado para iniciar sesión.

6. **Categorías fijas para el MVP.**  
   Plomería, Electricidad y Carpintería vienen precargadas; en este sprint no se crean categorías nuevas.

7. **Gestión completa de trabajos.**  
   El administrador puede crear, editar y desactivar trabajos desde Sprint 1.

8. **Datos iniciales para la demostración.**  
   Se precargan trabajos de ejemplo para que el Sprint Review no dependa de llenar todo el catálogo manualmente.

9. **Prevalidación automática como apoyo.**  
   Se prueba un filtro automático básico para detectar documentos ilegibles o incompletos, pero la aprobación final sigue correspondiendo al administrador.

10. **Mayor evidencia cuando no existe certificación.**  
    Se proponen 3 referencias y evidencia de 3 trabajos realizados en lugar del mínimo original de 2 referencias y una foto de trabajo.

11. **Bloqueo previo a revisión.**  
    Un expediente con problemas detectados por las validaciones no puede pasar al estado “en revisión” hasta que el técnico lo corrija.

> **Nota para el equipo:** Los puntos 9, 10 y 11 modifican o amplían el criterio original de HU-02a y deben ser aceptados por el equipo antes de considerarlos parte definitiva del Sprint Backlog.

---

# 7. Consideraciones para GitHub Projects

Cada tarea debe registrarse como sub-issue de su historia correspondiente o como nota técnica vinculada.

Formato sugerido para títulos:

`TT-01-01 | Crear modelo de usuarios, roles y códigos de acceso`

Campos o labels recomendados:
- Historia padre: HU-01 / HU-04 / HU-02a
- Rol: Backend / Frontend / QA / DevOps / AI
- Estimación: horas
- Sprint: Sprint 1
- Estado: Todo / In Progress / Done

Una tarea solo se marca como **Done** cuando cumple el criterio indicado en este documento.
