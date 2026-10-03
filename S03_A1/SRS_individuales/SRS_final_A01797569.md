# SRS_final.md — FairFix
## Especificación de Requerimientos de Software

---

# 1. Introducción

## 1.1 Propósito del documento

Este documento define los requerimientos de software de **FairFix**, una plataforma orientada a conectar personas que requieren servicios de mantenimiento o reparación en el hogar con técnicos verificados.

El SRS establece el alcance funcional, las restricciones, reglas de negocio y criterios de aceptación del sistema. Servirá como referencia para el diseño posterior de la API, la implementación del sistema y la elaboración de casos de prueba.

## 1.2 Alcance del sistema

FairFix permitirá que un cliente busque servicios del hogar dentro de tres categorías iniciales:

- Plomería.
- Carpintería.
- Electricidad.

El sistema manejará dos tipos principales de trabajo:

1. **Servicios base o parametrizables**, cuyo precio puede conocerse o estimarse mediante variables previamente definidas.
2. **Trabajos especiales sujetos a cotización**, donde el cliente proporciona una descripción, fotografías, medidas u otra evidencia para que un técnico pueda valorar el servicio.

FairFix también permitirá consultar perfiles y trabajos realizados por los técnicos, solicitar visitas de diagnóstico cuando la información sea insuficiente, emitir y aceptar cotizaciones, gestionar costos adicionales, dar seguimiento al estado del servicio, confirmar su finalización, liberar el pago y registrar una calificación.

### Exclusiones del alcance

En esta versión no se contempla:

- nómina o administración laboral de técnicos;
- contabilidad fiscal;
- compra automática de materiales;
- diagnóstico automático mediante inteligencia artificial;
- garantizar que todos los servicios puedan cotizarse de forma remota;
- arbitraje automatizado de disputas complejas.

## 1.3 Definiciones y acrónimos

**Cliente:** Persona que utiliza FairFix para solicitar un servicio del hogar.

**Técnico:** Persona que ofrece y ejecuta servicios de plomería, carpintería o electricidad mediante la plataforma.

**Servicio base:** Trabajo previamente definido cuyo costo puede establecerse directamente o mediante variables conocidas.

**Servicio parametrizable:** Servicio base cuyo precio depende de datos como cantidad, capacidad, distancia o carga.

**Trabajo especial:** Servicio que no puede resolverse mediante el catálogo base y requiere una cotización particular.

**Cotización:** Propuesta del técnico que establece alcance, condiciones y precio del trabajo.

**Evidencia:** Fotografías, medidas, descripciones u otros datos utilizados para documentar una solicitud, cotización o imprevisto.

**Imprevisto:** Condición detectada durante el servicio que no formaba parte del alcance originalmente acordado.

**Visita de diagnóstico:** Evaluación presencial previa a una cotización definitiva cuando la información remota no es suficiente.

**SRS:** Software Requirements Specification.

---

# 2. Descripción general

## 2.1 Perspectiva del producto

FairFix será una plataforma web que centralizará la interacción entre clientes y técnicos de servicios del hogar.

La plataforma busca formalizar actividades que frecuentemente se acuerdan mediante recomendaciones personales o medios informales, proporcionando un registro del servicio, la cotización acordada, las evidencias relevantes y las autorizaciones realizadas por ambas partes.

El sistema deberá distinguir claramente las funciones correspondientes al cliente y al técnico y conservar la trazabilidad de las decisiones económicas relevantes.

## 2.2 Funciones del producto

Las funciones principales de FairFix serán:

- registro e inicio de sesión;
- búsqueda de servicios;
- selección de categorías;
- consulta de servicios base;
- captura de variables para servicios parametrizables;
- solicitud de trabajos especiales;
- carga de fotografías, medidas y descripciones;
- solicitud de visita de diagnóstico;
- consulta de perfiles y portafolios de técnicos;
- emisión y aceptación de cotizaciones;
- registro y autorización de costos adicionales;
- seguimiento del estado del servicio;
- confirmación de finalización;
- liberación del pago;
- generación de recibo digital;
- calificación del técnico.

## 2.3 Características de los usuarios

### Cliente

Persona que requiere un servicio de reparación o mantenimiento en su hogar.

No se asume conocimiento técnico. La plataforma debe permitir que describa el problema en términos comprensibles y proporcione evidencia suficiente para que el técnico pueda valorar la solicitud.

Debido a que el público puede incluir adultos mayores y usuarios con poca experiencia digital, los flujos de búsqueda, registro, cotización y pago deberán ser especialmente sencillos.

### Técnico

Profesional independiente que ofrece servicios dentro de una o más categorías disponibles.

Debe poder revisar solicitudes, identificar si la información es suficiente, solicitar una visita cuando sea necesaria, emitir cotizaciones, registrar imprevistos y mantener actualizado su perfil y portafolio.

### Administrador de la plataforma

Usuario responsable de funciones administrativas que no corresponden directamente a clientes o técnicos, incluyendo la gestión del proceso mediante el cual un técnico obtiene el estado de verificado.

## 2.4 Restricciones

- El alcance inicial se limita a plomería, carpintería y electricidad.
- Un costo adicional no puede aprobarse unilateralmente por el técnico.
- Una corrección causada por un error del técnico no puede registrarse como trabajo adicional cobrable.
- Un técnico no puede mostrarse como verificado antes de completar el proceso correspondiente.
- La plataforma no debe obligar a emitir una cotización definitiva si la información disponible es insuficiente.
- El acuerdo inicial debe conservarse para distinguirlo de modificaciones posteriores.
- En esta versión, el pago no deberá liberarse automáticamente sin confirmación del cliente.

---

# 3. Requerimientos específicos

## 3.1 Requerimientos funcionales

### RF-01 — Registro, autenticación y tipo de usuario

El sistema deberá permitir que una persona se registre e inicie sesión como cliente o técnico, manteniendo separadas las funciones correspondientes a cada tipo de usuario.

#### RF-01-AC-1
**Dado que** una persona no tiene una cuenta en FairFix,  
**cuando** complete los campos obligatorios de registro y seleccione el tipo de usuario,  
**entonces** el sistema deberá crear la cuenta con el rol seleccionado y permitir posteriormente el inicio de sesión.

#### RF-01-AC-2
**Dado que** un usuario ha iniciado sesión,  
**cuando** intente acceder a una función reservada para un rol diferente al suyo,  
**entonces** el sistema deberá impedir la operación y no mostrar controles que permitan ejecutarla.

---

### RF-02 — Búsqueda y selección de servicios por categoría

El sistema deberá permitir al cliente buscar y solicitar servicios dentro de las categorías de plomería, carpintería y electricidad.

Cada categoría deberá presentar servicios base y una opción de **“otro trabajo sujeto a cotizar”**.

#### RF-02-AC-1
**Dado que** un cliente se encuentra en la sección de servicios,  
**cuando** seleccione plomería, carpintería o electricidad,  
**entonces** el sistema deberá mostrar los servicios base disponibles para esa categoría y la opción de otro trabajo sujeto a cotizar.

#### RF-02-AC-2
**Dado que** el cliente visualiza los servicios de una categoría,  
**cuando** seleccione un servicio base o la opción de trabajo especial,  
**entonces** el sistema deberá abrir el flujo correspondiente para capturar la información necesaria.

---

### RF-03 — Cálculo o presentación de precio para servicios base

El sistema deberá mostrar un precio base o calcular un precio de referencia para servicios estandarizados utilizando las variables definidas para cada caso.

Ejemplos de variables:
- HP o capacidad;
- cantidad;
- tipo de componente;
- distancia;
- carga;
- tamaño.

El alcance incluido en el precio deberá mostrarse al cliente.

#### RF-03-AC-1
**Dado que** el cliente seleccionó un servicio parametrizable,  
**cuando** capture todas las variables obligatorias definidas para ese servicio,  
**entonces** el sistema deberá mostrar el precio base o precio calculado correspondiente junto con las condiciones incluidas.

#### RF-03-AC-2
**Dado que** un servicio requiere variables para determinar su precio,  
**cuando** el cliente intente continuar sin completar una variable obligatoria,  
**entonces** el sistema deberá identificar el dato faltante y no deberá presentar la solicitud como lista para contratar.

---

### RF-04 — Solicitud de cotización para trabajos especiales

Cuando un trabajo no corresponda a un servicio base, el sistema deberá permitir al cliente generar una solicitud especial con descripción, resultado esperado, fotografías, medidas y otros datos aplicables.

El técnico deberá poder solicitar información adicional.

#### RF-04-AC-1
**Dado que** un cliente seleccionó “otro trabajo sujeto a cotizar”,  
**cuando** complete la descripción del problema y capture la información disponible,  
**entonces** el sistema deberá crear una solicitud de cotización y conservar la evidencia adjunta.

#### RF-04-AC-2
**Dado que** un técnico está revisando una solicitud especial y considera insuficiente la información,  
**cuando** solicite datos adicionales al cliente,  
**entonces** el sistema deberá registrar la solicitud de información y mantener la cotización sin aprobar hasta recibir una respuesta o definir una visita.

---

### RF-05 — Solicitud de visita de diagnóstico

El técnico deberá poder indicar que una solicitud requiere una visita de diagnóstico cuando la información disponible no sea suficiente para emitir una cotización responsable.

#### RF-05-AC-1
**Dado que** un técnico revisa una solicitud especial,  
**cuando** determine que no puede cotizar de forma responsable con la información disponible,  
**entonces** el sistema deberá permitir marcar la solicitud como “requiere visita de diagnóstico”.

#### RF-05-AC-2
**Dado que** una solicitud fue marcada como “requiere visita de diagnóstico”,  
**cuando** el cliente consulte el estado del servicio,  
**entonces** el sistema deberá mostrar claramente que aún no existe una cotización definitiva y que se requiere una evaluación presencial.

---

### RF-06 — Consulta de perfil y portafolio del técnico

El cliente deberá poder consultar el perfil de un técnico y visualizar su estado de verificación, especialidades, trabajos realizados, fotografías, evaluaciones y las marcas o materiales registrados por el técnico.

#### RF-06-AC-1
**Dado que** un cliente consulta el perfil de un técnico,  
**cuando** el perfil contenga información registrada,  
**entonces** el sistema deberá mostrar sus especialidades, trabajos previos, fotografías y evaluaciones disponibles.

#### RF-06-AC-2
**Dado que** un técnico no ha completado el proceso de verificación,  
**cuando** un cliente consulte su perfil,  
**entonces** el sistema no deberá mostrarlo como técnico verificado.

---

### RF-07 — Emisión y aceptación de cotización

El técnico deberá poder emitir una cotización con alcance, precio y condiciones.

El cliente deberá poder aceptarla o rechazarla. Una cotización aceptada deberá conservar su alcance, precio y evidencia asociada como referencia del acuerdo inicial.

#### RF-07-AC-1
**Dado que** el técnico cuenta con información suficiente para cotizar,  
**cuando** registre el alcance, precio y condiciones y envíe la cotización,  
**entonces** el sistema deberá presentarla al cliente con opciones explícitas para aceptarla o rechazarla.

#### RF-07-AC-2
**Dado que** el cliente acepta una cotización,  
**cuando** confirme su aceptación,  
**entonces** el sistema deberá registrar la fecha, el alcance, el precio y la evidencia asociada como versión aceptada del acuerdo inicial.

---

### RF-08 — Gestión de trabajos adicionales e imprevistos

El técnico deberá poder reportar una condición no contemplada originalmente mediante descripción, evidencia, trabajo adicional propuesto y costo adicional.

El cliente deberá autorizar o rechazar el cambio antes de que sea considerado aprobado.

#### RF-08-AC-1
**Dado que** durante un servicio aparece una condición no incluida en la cotización aceptada,  
**cuando** el técnico registre el imprevisto, adjunte la evidencia disponible y proponga un costo adicional,  
**entonces** el sistema deberá mostrar la solicitud al cliente como pendiente de autorización.

#### RF-08-AC-2
**Dado que** existe un costo adicional pendiente,  
**cuando** el cliente lo acepte o rechace explícitamente,  
**entonces** el sistema deberá registrar su decisión y no deberá considerar autorizado el trabajo adicional mientras la respuesta sea pendiente.

#### RF-08-AC-3
**Dado que** la corrección corresponde a un error cometido por el técnico durante el servicio,  
**cuando** el técnico registre dicha corrección,  
**entonces** el sistema deberá impedir que sea registrada como un trabajo adicional cobrable al cliente.

---

### RF-09 — Seguimiento, cierre, pago y reputación

El sistema deberá permitir dar seguimiento al servicio mediante estados definidos.

Al finalizar, deberá permitir al cliente confirmar la terminación, liberar el pago conforme a las reglas del servicio, obtener un recibo digital y calificar al técnico.

#### RF-09-AC-1
**Dado que** existe un servicio contratado,  
**cuando** cambie su estado durante la ejecución,  
**entonces** el sistema deberá mostrar al cliente y al técnico el estado actual del servicio.

#### RF-09-AC-2
**Dado que** el técnico indicó que el trabajo está finalizado,  
**cuando** el cliente confirme que el servicio fue realizado,  
**entonces** el sistema deberá permitir la liberación del pago retenido.

#### RF-09-AC-3
**Dado que** el pago correspondiente al servicio fue liberado,  
**cuando** el cliente consulte el servicio cerrado,  
**entonces** el sistema deberá permitir visualizar o descargar el recibo digital asociado.

#### RF-09-AC-4
**Dado que** un servicio fue cerrado,  
**cuando** el cliente registre una calificación y comentario,  
**entonces** el sistema deberá asociarlos al perfil del técnico y mostrarlos como parte de su reputación.

---

## 3.2 Requerimientos no funcionales

### RNF-01 — Usabilidad
**Categoría:** Usabilidad

Los procesos principales deberán presentarse mediante pasos claros, etiquetas comprensibles y confirmaciones visibles para acciones importantes.

El sistema deberá evitar formularios innecesariamente extensos, especialmente en búsqueda de servicios, registro, cotización y pago.

### RNF-02 — Accesibilidad y adaptación de interfaz
**Categoría:** Accesibilidad / Usabilidad

La interfaz deberá ser legible y utilizable tanto en teléfono como en computadora, manteniendo controles visibles, textos comprensibles y una jerarquía visual clara en los flujos principales.

### RNF-03 — Seguridad y control de acceso
**Categoría:** Seguridad

El sistema deberá restringir las operaciones según el tipo de usuario.

Un técnico no podrá aceptar cambios de precio ni liberar pagos en nombre del cliente.

Las credenciales y datos sensibles no deberán almacenarse ni transmitirse en texto plano.

### RNF-04 — Integridad y trazabilidad
**Categoría:** Integridad / Auditabilidad

El sistema deberá conservar el historial de cotizaciones, evidencias, cambios de alcance, costos adicionales, autorizaciones, finalización y liberación del pago.

Las modificaciones posteriores no deberán eliminar la información necesaria para reconstruir el acuerdo original y sus cambios.

---

## 3.3 Requerimientos de dominio

### RD-01 — Autorización obligatoria para trabajos adicionales
Un trabajo o costo que no forme parte del acuerdo inicial no podrá considerarse autorizado hasta que el cliente lo acepte explícitamente.

### RD-02 — Corrección de errores del técnico
Una corrección derivada de un error cometido por el técnico durante la ejecución del servicio no deberá registrarse como trabajo adicional cobrable al cliente.

### RD-03 — Evaluación presencial cuando sea necesaria
Cuando la información proporcionada no permita realizar una valoración razonable, el técnico podrá requerir una visita de diagnóstico antes de emitir una cotización definitiva.

### RD-04 — Uso del estado “técnico verificado”
Un técnico no podrá mostrarse como verificado hasta que la plataforma haya completado el proceso de verificación correspondiente.

### RD-05 — Retención y liberación del pago
En esta versión, el pago del servicio deberá permanecer retenido hasta que el cliente confirme que el trabajo fue realizado. No se contempla liberación automática por falta de respuesta del cliente.

### RD-06 — Vigencia del acuerdo inicial
La cotización aceptada, junto con su alcance y evidencia asociada, constituirá la referencia del acuerdo inicial.

Cualquier modificación económica o de alcance deberá conservarse como un cambio posterior y requerir la autorización correspondiente.
