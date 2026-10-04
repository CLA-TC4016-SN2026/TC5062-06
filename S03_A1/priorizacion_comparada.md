# Priorización comparada del Product Backlog — FairFix

## 1. Objetivo

El objetivo de este análisis es comparar la priorización propuesta por un agente de IA actuando como Product Owner con una priorización propia del equipo para las 18 historias de usuario incluidas en el backlog inicial de FairFix.

La comparación considera cuatro criterios principales: valor de negocio, dependencias entre historias, riesgo técnico y capacidad de entregar un flujo funcional de principio a fin.

## 2. Criterio de priorización del agente como Product Owner

Como Product Owner, el agente prioriza primero las historias necesarias para habilitar el servicio principal de FairFix y después las funciones que reducen riesgo o atienden excepciones.

Los criterios utilizados son:

- Habilitar primero el flujo mínimo completo: acceso, catálogo, técnicos disponibles, solicitud, contratación, pago, ejecución y cierre.
- Respetar las dependencias entre historias.
- Adelantar funciones con riesgo técnico relevante cuando puedan comprometer el proyecto.
- Proteger la confianza entre cliente, técnico y plataforma.
- Dejar al final funciones condicionadas por decisiones todavía no validadas.

### Priorización propuesta por el agente

| Posición | Historia | Justificación |
|---:|---|---|
| 1 | HU-01 — Inicio de sesión sin contraseña | Es el punto de entrada a la plataforma y habilita las acciones posteriores de clientes y técnicos. |
| 2 | HU-04 — Catálogo de trabajos y precios de referencia | Define qué servicios existen y establece la base para solicitudes y cotizaciones. |
| 3 | HU-02a — Enviar documentos y acreditar experiencia | Se necesita una fuente de técnicos candidatos antes de poder mostrar oferta real. |
| 4 | HU-02b — Validar a un técnico | Garantiza que solo técnicos aprobados puedan aparecer ante el cliente. |
| 5 | HU-03 — Disponibilidad y cotizaciones del técnico | Permite conocer cuándo puede trabajar el técnico y cuánto cobra. |
| 6 | HU-05 — Crear una solicitud de servicio | Inicia el flujo de negocio desde el cliente. |
| 7 | HU-06 — Consultar técnicos y cotización previa | Conecta la demanda del cliente con técnicos ya validados y con precio registrado. |
| 8 | HU-07 — Contratar a un técnico | Convierte la solicitud en un compromiso de servicio. |
| 9 | HU-11a — Pagar y retener el pago | Asegura el compromiso económico de ambas partes y es una integración de alto impacto. |
| 10 | HU-08 — Actualizar el estado del servicio | Permite ejecutar y dar seguimiento al trabajo contratado. |
| 11 | HU-09 — Avisos por WhatsApp con respaldo por SMS | Reduce incertidumbre durante la ejecución y además tiene riesgo técnico por integración con servicios externos. |
| 12 | HU-11b — Confirmar el trabajo y liberar el pago | Completa el flujo económico normal del servicio. |
| 13 | HU-13 — Reportar y resolver una inconformidad | Protege la retención del pago cuando existe un desacuerdo y requiere resolución humana. |
| 14 | HU-10 — Autorizar costos adicionales con evidencia | Protege al cliente ante cambios económicos durante la ejecución, aunque no aparece en todos los servicios. |
| 15 | HU-15 — Modo simplificado | Amplía la accesibilidad del flujo principal una vez que este ya existe. |
| 16 | HU-14 — Recibo, historial y calificación | Aporta trazabilidad y reputación después de completar servicios. |
| 17 | HU-16 — Ayuda de una persona y canal de dudas | Es un mecanismo de soporte importante, pero complementario al flujo principal. |
| 18 | HU-12 — Recordatorio y liberación automática del pago | Se deja al final porque el plazo de 72 horas todavía requiere validación con el stakeholder. |

## 3. Criterio de priorización personal

Mi priorización considera como eje principal el flujo que permite concretar un servicio, pero también los mecanismos necesarios para generar confianza entre cliente y técnico.

FairFix no debe sustituir con decisiones automáticas aquellas situaciones que requieren criterio técnico, autorización económica o mediación. El técnico determina cuestiones propias de su trabajo; el cliente autoriza los cambios económicos; y FairFix interviene cuando existe una duda o conflicto que requiere mediación.

También considero que, antes de que un técnico pueda ofrecer sus servicios al cliente, debe haber sido evaluado, validado y aprobado por la plataforma. Por ello, el envío de documentos, la validación y el registro de disponibilidad y cotizaciones se consideran dependencias internas necesarias para que la consulta de técnicos tenga sentido.

La retención del pago es una función central del modelo. Por una parte, compromete al cliente a contar con los recursos para pagar un trabajo contratado. Por otra, protege al cliente al evitar que el dinero se entregue directamente al técnico antes de que exista evidencia de que el servicio fue realizado. La plataforma funciona así como un intermediario de confianza para ambas partes.

Después de la contratación y la retención del pago, considero prioritario desarrollar primero el seguimiento normal del trabajo. Las rutas de excepción, como inconformidades o costos adicionales, son importantes, pero no necesariamente aparecen en todos los servicios.

### Mi priorización propuesta

| Posición | Historia | Justificación |
|---:|---|---|
| 1 | HU-01 — Inicio de sesión sin contraseña | Es el primer acceso del usuario a FairFix. |
| 2 | HU-04 — Catálogo de trabajos y precios de referencia | El cliente necesita saber qué puede solicitar y tener una referencia de precio. |
| 3 | HU-02a — Enviar documentos y acreditar experiencia | Antes de ofrecer servicios debe existir información suficiente para evaluar al técnico. |
| 4 | HU-02b — Validar a un técnico | Ningún técnico debería aparecer al cliente sin haber sido aprobado. |
| 5 | HU-03 — Disponibilidad y cotizaciones del técnico | Un técnico aprobado debe indicar qué trabajos ofrece, cuándo puede atenderlos y cuánto cobra. |
| 6 | HU-05 — Crear una solicitud de servicio | Permite que el cliente exprese una necesidad real. |
| 7 | HU-06 — Consultar técnicos y cotización previa | El cliente puede comparar opciones ya validadas y conocer el costo antes de decidir. |
| 8 | HU-07 — Contratar a un técnico | Formaliza la selección del proveedor del servicio. |
| 9 | HU-11a — Pagar y retener el pago | Genera compromiso y seguridad para cliente y técnico antes de comenzar el trabajo. |
| 10 | HU-08 — Actualizar el estado del servicio | Inicia el seguimiento normal de la ejecución. |
| 11 | HU-09 — Avisos por WhatsApp con respaldo por SMS | Mantiene informado al cliente durante el flujo normal del servicio. |
| 12 | HU-10 — Autorizar costos adicionales con evidencia | Se atiende después del seguimiento normal porque solo aplica cuando aparece un imprevisto. |
| 13 | HU-11b — Confirmar el trabajo y liberar el pago | Cierra el flujo económico cuando el cliente está conforme. |
| 14 | HU-14 — Recibo, historial y calificación | Documenta el servicio terminado y fortalece la reputación futura del técnico. |
| 15 | HU-15 — Modo simplificado | Es importante para accesibilidad, pero se construye sobre un flujo principal que ya debe funcionar. |
| 16 | HU-16 — Ayuda de una persona y canal de dudas | Agrega apoyo humano para situaciones que el flujo normal no resuelve por sí solo. |
| 17 | HU-13 — Reportar y resolver una inconformidad | Es una protección necesaria, pero corresponde a una ruta excepcional y requiere mediación humana. |
| 18 | HU-12 — Recordatorio y liberación automática del pago | Se deja al final porque depende de validar con el stakeholder la regla de 72 horas. |

## 4. Comparación entre ambas priorizaciones

### Coincidencias principales

Ambas propuestas coinciden en que el producto debe empezar por acceso, catálogo y preparación de la oferta de técnicos. También coinciden en que los técnicos deben estar documentados, validados y con disponibilidad y precio definidos antes de que el cliente pueda compararlos.

Las dos priorizaciones colocan la creación de la solicitud, la consulta de técnicos, la contratación y la retención del pago dentro del núcleo del MVP. Esto se debe a que estas historias permiten que exista una transacción real y no solamente una demostración de pantallas.

También existe coincidencia en dejar HU-12 al final, ya que la liberación automática a las 72 horas depende de una decisión pendiente del stakeholder.

### Diferencias principales

La diferencia más importante aparece después de iniciar la ejecución del servicio.

Mi priorización favorece primero el flujo normal: actualizar estados, notificar al cliente y posteriormente atender costos adicionales, confirmación, soporte e inconformidades. La lógica es completar primero el camino que debería seguir un servicio cuando todo ocurre correctamente.

El agente, en cambio, adelanta la confirmación del trabajo y la resolución de inconformidades porque las considera componentes importantes del riesgo económico asociado al pago retenido. Desde esa perspectiva, no basta con retener dinero; también debe definirse pronto cómo se libera y qué ocurre si existe un desacuerdo.

Otra diferencia es la posición de HU-10. En mi propuesta se coloca antes que la liberación del pago porque un costo adicional puede aparecer durante la ejecución y debe resolverse antes del cierre. El agente le da menor prioridad porque se trata de una situación que no ocurre en todos los trabajos.

Estas diferencias no implican que una historia sea poco importante. Reflejan dos enfoques distintos: priorizar primero el recorrido normal del servicio o adelantar mecanismos de control de riesgo y manejo de excepciones.

## 5. Conclusión

La comparación muestra que priorizar un Product Backlog no consiste únicamente en ordenar historias por la etiqueta Alta, Media o Baja.

En FairFix también deben considerarse las dependencias entre funcionalidades, la confianza entre las partes y la diferencia entre el flujo normal y las rutas excepcionales.

Mi propuesta busca construir primero un camino completo y entendible para el usuario: entrar, consultar, solicitar, comparar, contratar, comprometer el pago, dar seguimiento y cerrar. Las funciones de mediación y excepción se incorporan después, sin eliminarlas, porque siguen siendo necesarias para que la plataforma pueda operar de forma segura.

La propuesta del agente coincide en la mayor parte del núcleo, pero adelanta algunas funciones de control de riesgo. Esta diferencia resulta útil porque muestra que la priorización puede variar según se valore más la continuidad del flujo principal o la reducción temprana de riesgos técnicos y de negocio.
