# Especificación de Requerimientos de Software (SRS)
## Sistema FairFix: Plataforma de Servicios del Hogar con Técnicos Verificados
**Estándar de Referencia:** IEEE Std 830-1998 (Estructura Simplificada)  
**Versión:** 1.0.0-Final  
**Fecha:** Septiembre 2026  
**Proyecto:** Plataforma de Servicios del Hogar con Técnicos Verificados  
**Materia:** TC4016 - Proyecto Integrador  
**Estudiante:** Luis Manuel Mendoza Cruz (Matrícula: A01797418)  

---

## 1. Introducción

### 1.1 Propósito del documento
El propósito de esta Especificación de Requerimientos de Software (SRS) es definir formalmente las necesidades funcionales, no funcionales y de dominio para la plataforma FairFix. Este documento actúa como contrato técnico de desarrollo bajo el enfoque Spec Driven Design (SDD): a partir de sus especificaciones se diseñará la API REST (Semana 4) y se derivarán los casos de prueba automatizados con BDD (Semana 9).

### 1.2 Alcance del sistema
FairFix es un ecosistema tecnológico web y móvil que formaliza la contratación de servicios de oficios técnicos (plomería, electricidad, carpintería, cerrajería) en el hogar. La plataforma abarca:
* Registro, validación de antecedentes y activación de técnicos independientes.
* Cotización algorítmica estimada con rangos tarifarios justos de referencia.
* Custodia transaccional del pago (escrow) mediante pasarela segura.
* Gestión transparente de sobrecostos en sitio con validación gráfica.
* Cierre de servicio verificado por código OTP y emisión de comprobantes fiscales.
* Módulo especializado de accesibilidad "Modo Simplificado" para adultos mayores.

**Exclusiones Explícitas del Alcance:**
* Suministro, venta o transporte directo de materiales pesados y refacciones de obra civil.
* Cobertura de pólizas de seguro de daños a terceros en la versión MVP.

### 1.3 Definiciones, acrónimos y abreviaturas
* **API:** *Application Programming Interface*.
* **BDD:** *Behavior-Driven Development*.
* **Escrow:** Retención de fondos en garantía por un tercero neutral hasta el cumplimiento satisfactorio del servicio acordado.
* **OTP:** *One-Time Password* (Contraseña numérica temporal de un solo uso).
* **PAN:** *Primary Account Number* (Número de 16 dígitos de tarjeta bancaria).
* **PII:** *Personally Identifiable Information* (Información de Identificación Personal).
* **SDD:** *Spec-Driven Design*.
* **WCAG:** *Web Content Accessibility Guidelines*.

---

## 2. Descripción General

### 2.1 Perspectiva del producto
FairFix es una plataforma distribuida en la nube compuesta por:
* **Aplicación Cliente:** Interfaz responsiva con alternancia inmediata a Modo Simplificado.
* **Aplicación Técnico:** Interfaz móvil optimizada para captura de evidencia y seguimiento de órdenes.
* **Panel de Control Administrativo:** Módulo web para auditoría documental y resolución de disputas.
* **Backend de Servicios RESTful:** Capa de lógica de negocio orientada a contratos, desacoplada de la base de datos relacional y conectada a pasarelas bancarias.

### 2.2 Funciones del producto
* Gobierno y verificación de identidad de técnicos.
* Cálculo dinámico de cotizaciones estimadas.
* Gestión de retención y dispersión de fondos.
* Autorización supervisada de sobrecostos con evidencia fotográfica.
* Liberación de pagos y generación de recibos digitales.
* Sistema de reputación y retroalimentación bidireccional.

### 2.3 Características de los usuarios
* **Cliente Residencial:** Busca certeza de precio, puntualidad y seguridad en su hogar.
* **Cliente Adulto Mayor:** Requiere interfaz de baja complejidad, fuentes grandes, alto contraste y confirmaciones claras.
* **Técnico Independiente:** Ofrece mano de obra calificada; requiere certeza de cobro y reputación verificada.
* **Administrador / Validador:** Responsable del cumplimiento normativo, revisión de antecedentes y resolución de garantías.

### 2.4 Restricciones generales
* Ningún dato bancario sensible (PAN/CVV) puede persistir en las bases de datos de FairFix (cumplimiento PCI-DSS).
* La comunicación directa entre clientes y técnicos debe anonimizarse para proteger la PII telefónica.
* La plataforma debe operar bajo estándares de accesibilidad universal WCAG 2.1 Nivel AA.

---

## 3. Requerimientos Específicos y Criterios de Aceptación (Given-When-Then)

### 3.1 Requerimientos Funcionales (RF)

#### RF-01: Verificación Documental de Técnicos
El sistema debe permitir a los técnicos registrarse cargando identificación oficial vigente y constancia de antecedentes no penales, manteniendo el perfil inactivo hasta su aprobación administrativa.
* **RF-01-AC-1: Bloqueo de técnicos no validados**
  * **Dado que** un técnico tiene su cuenta en estado `PENDING_REVIEW`,
  * **cuando** un cliente realiza una búsqueda de servicios en la zona,
  * **entonces** la API excluye al técnico de los resultados y le impide postularse a órdenes de trabajo.
* **RF-01-AC-2: Aprobación y activación de cuenta**
  * **Dado que** el administrador valida la autenticidad de los documentos cargados,
  * **cuando** ejecuta la acción de aprobación en el panel de control,
  * **entonces** el estado del técnico cambia a `VERIFIED`, habilitando la recepción de solicitudes en tiempo real.

#### RF-02: Cotización Estimada con Rango de Referencia
El sistema debe calcular y desplegar un rango de precio estimado (mínimo y máximo) previo a la confirmación de la solicitud según la categoría y gravedad reportada.
* **RF-02-AC-1: Generación de rango tarifario transparente**
  * **Dado que** un cliente selecciona la categoría "Plomería" y la falla "Fuga de agua en lavabo",
  * **cuando** solicita la estimación del costo,
  * **entonces** el sistema presenta un rango de $350 a $500 MXN desglosando la tarifa base y el costo estimado de mano de obra.
* **RF-02-AC-2: Validación de categorización obligatoria**
  * **Dado que** el usuario omite seleccionar la gravedad o tipo de falla,
  * **cuando** intenta avanzar en la solicitud,
  * **entonces** el sistema bloquea el avance con código `HTTP 422 Unprocessable Entity` señalando los parámetros requeridos.

#### RF-03: Retención Transaccional de Fondos en Custodia (Escrow)
El sistema debe retener el monto base cotizado en la tarjeta del cliente al aceptar la orden, sin transferir los fondos al técnico hasta la conformidad final.
* **RF-03-AC-1: Retención exitosa de fondos en pasarela**
  * **Dado que** el cliente tiene fondos suficientes y confirma la solicitud de servicio,
  * **cuando** la orden es confirmada por un técnico disponible,
  * **entonces** la pasarela procesa la retención temporal (`status: HELD`) y el sistema asigna el estado `ESCROW_LOCKED`.
* **RF-03-AC-2: Manejo de tarjeta declinada**
  * **Dado que** el método de pago del cliente no cuenta con saldo disponible,
  * **cuando** se solicita la retención,
  * **entonces** la transacción se detiene, la orden no se publica y se notifica al usuario para actualizar su método de pago.

#### RF-04: Autorización de Sobrecostos con Evidencia Multimedia
Si el técnico detecta imprevistos en sitio, debe registrar una solicitud de costo adicional adjuntando evidencia fotográfica obligatoria antes de realizar el cobro.
* **RF-04-AC-1: Notificación de sobrecosto con evidencia al cliente**
  * **Dado que** una orden está en estado `IN_PROGRESS`,
  * **cuando** el técnico ingresa un costo extra de $150 MXN adjuntando una fotografía legible de la pieza dañada,
  * **entonces** el cliente recibe una alerta en su pantalla con la imagen y la opción binaria de autorizar o declinar el cargo adicional.
* **RF-04-AC-2: Bloqueo de cobro extra sin evidencia gráfica**
  * **Dado que** el técnico intenta registrar un importe adicional omitiendo adjuntar el archivo gráfico,
  * **cuando** presiona "Solicitar Aumento",
  * **entonces** el backend rechaza la petición con código `HTTP 400 Bad Request` indicando la obligatoriedad de la evidencia.

#### RF-05: Cierre de Servicio Mediante Validación OTP
El servicio solo se dará por concluido cuando el técnico introduzca en su dispositivo el código OTP de 4 dígitos generado por la aplicación del cliente.
* **RF-05-AC-1: Conclusión satisfactoria y dispersión de fondos**
  * **Dado que** el trabajo físico ha terminado y el cliente proporciona su código OTP de 4 dígitos,
  * **cuando** el técnico ingresa el código correcto en su aplicación,
  * **entonces** la orden transiciona a `COMPLETED`, el sistema libera los fondos retenidos al saldo del técnico y emite la orden de dispersión.
* **RF-05-AC-2: Bloqueo por código OTP incorrecto**
  * **Dado que** el técnico ingresa un código erróneo,
  * **cuando** el backend valida el intento,
  * **entonces** retorna `HTTP 401 Unauthorized`, mantiene los fondos en custodia y bloquea nuevos intentos tras 3 fallos consecutivos.

#### RF-06: Emisión de Recibo Digital Desglosado
El sistema debe generar un comprobante fiscal y digital detallando mano de obra, sobrecostos autorizados, IVA y comisión de la plataforma.
* **RF-06-AC-1: Generación y descarga de comprobante**
  * **Dado que** una orden ha finalizado con éxito,
  * **cuando** el cliente ingresa al detalle de la orden concluida,
  * **entonces** el sistema dispone de un comprobante digital en formato PDF descargable con folio único e importes desglosados.
* **RF-06-AC-2: Consistencia aritmética del recibo**
  * **Dado que** un servicio tuvo una tarifa base de $400 MXN y un sobrecosto aprobado de $200 MXN,
  * **cuando** se consulta el comprobante emitido,
  * **entonces** el monto total reflejado es exactamente $600 MXN coincidiendo con el cargo efectuado a la tarjeta.

#### RF-07: Evaluación y Reputación Bidireccional
El cliente debe registrar una calificación de 1 a 5 estrellas y un comentario que impactará el promedio público del técnico.
* **RF-07-AC-1: Actualización de reputación post-servicio**
  * **Dado que** un servicio ha concluido (`COMPLETED`),
  * **cuando** el cliente envía una evaluación de 5 estrellas con reseña escrita,
  * **entonces** el sistema recalcula el promedio ponderado del técnico y actualiza su perfil visible en menos de 5 segundos.
* **RF-07-AC-2: Bloqueo de reseñas en servicios no concluidos**
  * **Dado que** un servicio se encuentra cancelado o en curso,
  * **cuando** un usuario envía una petición de calificación vía API,
  * **entonces** el sistema responde con `HTTP 403 Forbidden` preservando la integridad del historial del técnico.

#### RF-08: Interfaz Adaptativa en Modo Simplificado
El sistema debe incorporar un selector para activar una interfaz orientada a adultos mayores con navegación lineal en 3 pasos y tipografía ampliada.
* **RF-08-AC-1: Adaptación visual en Modo Simplificado**
  * **Dado que** el usuario tiene activado el "Modo Simplificado",
  * **cuando** interactúa con las vistas de la plataforma,
  * **entonces** la interfaz despliega botones con dimensiones mínimas de 48x48 píxeles y tipografía no menor a 18 puntos con ratio de contraste accesible.
* **RF-08-AC-2: Confirmación asistida de solicitud**
  * **Dado que** un usuario en Modo Simplificado completa una solicitud,
  * **cuando** presiona "Confirmar Servicio",
  * **entonces** el sistema presenta un resumen en pantalla única con confirmación por audio o llamada automatizada opcional antes de generar el cargo.

---

### 3.2 Requerimientos No Funcionales (RNF)

* **RNF-01 [Seguridad - Autenticación y Cifrado]:**  
  Todas las comunicaciones externas deben realizarse sobre HTTPS (TLS 1.3), y los accesos administrativos deben exigir autenticación multifactor (MFA).

* **RNF-02 [Usabilidad - Accesibilidad WCAG]:**  
  La interfaz en Modo Simplificado debe cumplir con el nivel de conformidad AA según las directrices WCAG 2.1, garantizando navegación completa mediante lectores de pantalla.

* **RNF-03 [Rendimiento - Tiempo de Respuesta]:**  
  El cálculo y entrega del rango de cotización estimada debe completarse en un tiempo no mayor a 1.5 segundos en el percentil 95 (P95) bajo condiciones nominales de red.

* **RNF-04 [Disponibilidad - SLA Operativo]:**  
  La plataforma debe garantizar una disponibilidad mínima del 99.9% durante la ventana operativa de atención domiciliaria (07:00 a 22:00 horas, lunes a domingo).

---

### 3.3 Requerimientos de Dominio (RD)

* **RD-01 [Garantía Comercial Obligatoria]:**  
  Toda reparación efectuada a través de FairFix cuenta con una garantía legal de 7 días naturales; ante un reporte de reincidencia, la comisión de la plataforma se congela y se asigna revisión sin costo de mano de obra.

* **RD-02 [Cumplimiento Fiscal de Plataformas Digitales]:**  
  El sistema debe calcular y retener automáticamente los porcentajes de ISR e IVA correspondientes a los ingresos percibidos por los técnicos independientes de conformidad con el régimen fiscal de plataformas tecnológicas.
