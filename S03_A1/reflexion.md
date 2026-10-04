# Reflexión sobre SCRUM y el uso de IA — FairFix

**Fecha:** octubre de 2026
**Documentos en que se basa:** `SRS_equipo.md` (v2.0), `ajustes_backlog.md`, `priorizacion_comparada.md`

Esta reflexión parte de lo que ocurrió en las Partes 0 a 3 de la actividad. Cada afirmación se apoya en un ajuste, una decisión o una comparación concreta registrada en los documentos del equipo.

---

## 1. ¿Qué ventajas y riesgos identificas en usar un agente de IA para generar el Product Backlog?

### Ventajas

**Velocidad y cobertura de un documento grande.** El `SRS_equipo.md` tiene 27 requerimientos funcionales y 168 criterios de aceptación. El agente convirtió ese volumen en un backlog de 16 historias y 100 story points organizadas en 5 épicas, algo que al equipo le habría tomado varias sesiones hacer desde cero. El trabajo del equipo pudo concentrarse en revisar, no en redactar.

**Trazabilidad conservada.** El agente vinculó cada criterio de aceptación de las historias con los IDs del SRS (por ejemplo, `HU-11a-CA-1` remite a `RF-10-AC-1` y `RF-10-AC-5`). Eso facilitará derivar las pruebas BDD y los endpoints en las semanas siguientes.

**Lectura de las decisiones abiertas.** El agente, actuando como Product Owner, colocó HU-12 (liberación automática a las 72 horas) al final del backlog porque el Anexo A del SRS marca ese plazo como pendiente de validar con el stakeholder (A-3). Es decir, no solo leyó los requerimientos, también tomó en cuenta las incertidumbres documentadas.

**Una segunda opinión útil.** En la Parte 3, la priorización del agente coincidió con la del equipo en las primeras 11 posiciones y difirió en cinco de las últimas siete. Esa diferencia obligó al equipo a explicar por qué prefería el flujo normal antes que las rutas de excepción, en lugar de asumirlo sin discutirlo.

### Riesgos

**Historias que parecen correctas pero violan reglas básicas de SCRUM.** De los seis ajustes que el equipo tuvo que hacer, dos fueron por mezclar perspectivas de usuario: HU-02 decía "como técnico" pero tres de sus cinco criterios eran acciones del administrador (AJ-02), y HU-08 era del técnico pero uno de sus criterios estaba escrito desde el cliente (AJ-06). Son errores fáciles de pasar por alto porque la redacción se ve profesional.

**Historias demasiado grandes.** HU-11 (pago retenido) salió con 13 puntos, el máximo de la escala, y mezclaba la integración con la pasarela de pago con la confirmación del trabajo (AJ-01). El agente no aplicó el criterio de dividir una historia que no cabe cómodamente en un sprint.

**Estimaciones que no reflejan la incertidumbre.** HU-09 (avisos por WhatsApp) se estimó en 8 puntos, aunque el propio SRS dice que el proveedor aún no está elegido (A-8). El equipo la subió a 13 (AJ-03). El dato estaba en el documento; lo que faltó fue el juicio para darle peso.

**Prioridades que no sirven para ordenar.** El backlog original tenía 14 de 16 historias en prioridad Alta. Con esa distribución la etiqueta no ayuda a decidir qué entra primero (AJ-04).

**Riesgo de confiar en la coincidencia.** Que el agente y el equipo coincidieran en las primeras 11 posiciones no prueba que el orden sea correcto. Ambos partieron del mismo SRS y de las mismas dependencias, así que la coincidencia puede reflejar el documento más que un análisis independiente del valor de negocio.

---

## 2. ¿Puede un agente reemplazar al Product Owner?

No. Puede apoyarlo mucho, pero no reemplazarlo, por tres razones que se vieron en este proyecto.

**El Product Owner valida con el stakeholder; el agente solo lee lo que ya está escrito.** El SRS tiene 14 decisiones abiertas en el Anexo A y varios valores marcados como **(P)**, propuestas del equipo que no salieron de una entrevista. La más clara es A-3: dos SRS individuales pedían no liberar el pago sin la confirmación del cliente, y el plazo de 72 horas sigue sin validarse. El agente pudo detectar la dependencia y mandar HU-12 al fondo, pero no puede resolverla. Resolverla requiere hablar con el cliente, y eso es responsabilidad del Product Owner.

**Priorizar implica decidir entre valores legítimos.** En la Parte 3, el agente adelantó la inconformidad (HU-13, posición 13) porque la consideró un control de riesgo del pago retenido; el equipo la llevó a la posición 17 porque prefería completar primero el flujo normal. Incluso la mantuvo en prioridad Alta, cuando el equipo ya la había bajado a Media en AJ-04. Ambas posturas son defendibles. Elegir entre ellas depende de qué quiere demostrar el producto primero y de qué riesgos acepta el negocio, y esa decisión necesita a alguien que responda por ella.

**El Product Owner rinde cuentas.** Si la priorización falla, alguien debe explicar por qué se tomó y ajustarla en la siguiente revisión del sprint. Un agente puede justificar un orden, pero no asume la responsabilidad del resultado ni negocia con el equipo de desarrollo cuando cambian las condiciones. Las instrucciones de la actividad lo reflejan: el agente puede ayudar a formular, pero la decisión es del equipo.

La conclusión es que el agente funciona bien como asistente del Product Owner: propone, detecta dependencias y ofrece una segunda perspectiva. Las decisiones sobre valor, riesgo y alcance siguen siendo humanas.

---

## 3. ¿Cómo afecta la calidad del SRS a la calidad del backlog generado?

De forma directa: el backlog heredó tanto las fortalezas como las debilidades del SRS.

**Lo que el SRS hizo bien se notó en el backlog.**

- Los criterios de aceptación en formato *Dado que / cuando / entonces* con IDs únicos (`RF-XX-AC-N`) se tradujeron casi directamente en criterios de historias verificables y trazables.
- La tabla de estados del servicio (solicitado, aceptado, en camino, etc.) le dio al agente un vocabulario preciso, por lo que los criterios de las historias usan los mismos estados que usará la API.
- El Anexo A de decisiones abiertas permitió marcar dependencias reales, como la nota de HU-12 (AJ-05).

**Lo que el SRS dejó débil también pasó al backlog.**

- **Prioridades poco discriminantes.** El SRS define Alta como "necesario para el flujo básico", y casi todos los requerimientos centrales quedaron en Alta. El agente copió esa escala y por eso 14 de 16 historias salieron en Alta. El equipo tuvo que reintroducir el criterio de planeación (AJ-04).
- **Requerimientos con varios actores.** RF-03 junta lo que hace el técnico y lo que hace el administrador en un solo requerimiento, y el agente reprodujo esa mezcla en HU-02 (AJ-02). Un requerimiento que combina roles tiende a generar una historia que también los combina.
- **Lo que el SRS no cubre, el backlog tampoco lo cubre.** El Anexo A (A-12) reconoce que el cliente real entrevistado pidió rastreo de la llegada del técnico y atención 24/7, y que ningún requerimiento los cubre. En consecuencia, no hay ninguna historia para ellas. El agente no inventa necesidades que no están en el documento, lo cual es correcto, pero significa que un hueco en el SRS se vuelve un hueco en el producto.
- **Volumen frente a alcance.** El SRS tiene 27 requerimientos, pero el backlog tiene 18 historias. Varias funciones de prioridad Media o Baja (por ejemplo, retrasos e inasistencias del técnico, cancelación por el cliente, garantía o promociones) no tienen historia propia todavía. Esto es razonable para un primer backlog, pero conviene que el equipo lo decida de forma explícita y no por omisión.

En resumen, un SRS sólido no garantiza un buen backlog, porque el equipo todavía tuvo que hacer seis ajustes. Sin embargo, reduce mucho el trabajo de corrección y hace que los errores sean más fáciles de detectar. El tiempo que el equipo invirtió en la Parte 0 se recuperó en las Partes 1 a 3.
