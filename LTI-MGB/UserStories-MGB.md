# User Stories y Product Backlog - LTI

Este documento recoge las User Stories, el Product Backlog priorizado, la selección de la historia de usuario principal para el MVP, el desglose de tickets de trabajo y la estimación de esfuerzo para iniciar la implementación del sistema LTI, tomando como referencia el documento PRD `LTI-MMX.md`.

---

## 1. User Stories

### US-01: Evaluación automática y explicable de postulaciones en fase de Screening

**Como** Reclutador (Recruiter)

**Quiero** que el servicio de IA compare el CV del candidato con los criterios de la vacante y devuelva un análisis con puntuación y evidencias.

**Para** reducir el tiempo de filtrado manual inicial sin perder visibilidad sobre los motivos de la recomendación del modelo.

#### Criterios de aceptación

**Escenario 1: Procesamiento exitoso**

- **Given** que una postulación (`Application`) transiciona al estado `SCREENING` y tiene un CV válido adjunto.
- **When** el sistema invoca al servicio de evaluación de IA de manera asíncrona.
- **Then** se debe persistir una recomendación (`AIRecommendation`) con estado `PENDING_REVIEW`, incluyendo puntuación, factores ponderados y evidencias extraídas del CV.

**Escenario 2: Resolución humana (HITL)**

- **Given** que existe una recomendación de IA en estado `PENDING_REVIEW`.
- **When** el reclutador revisa la recomendación y selecciona una resolución manual.
- **Then** el sistema debe registrar la decisión en el log de auditoría y permitir la transición de estado correspondiente.

#### Caso borde

- Si el CV está corrupto o en un formato no soportado, el sistema debe marcar el proceso como `FAILED`, registrar el error correspondiente y permitir que el reclutador continúe el proceso de forma manual.

---

### US-02: Bloqueo del Pipeline ante descartes automatizados de la IA

**Como** Plataforma LTI

**Quiero** impedir que una postulación avance de fase si la recomendación de la IA es de descarte y no ha sido revisada por un humano.

**Para** asegurar el cumplimiento del principio Human-in-the-Loop (HITL) y evitar decisiones automatizadas sin supervisión.

#### Criterios de aceptación

**Escenario 1: Bloqueo de avance**

- **Given** que una postulación tiene una recomendación de IA con resultado negativo sin revisión humana.
- **When** un usuario intenta mover la postulación al estado `INTERVIEW`.
- **Then** el sistema debe rechazar la operación y mantener el estado actual.

**Escenario 2: Anulación manual (Override)**

- **Given** que la recomendación de IA ha sido considerada incorrecta por el reclutador.
- **When** el reclutador registra un override con justificación.
- **Then** el sistema debe permitir continuar el flujo y registrar la acción para auditoría.

#### Caso borde

- Si el candidato retira su candidatura durante el proceso de revisión, el sistema debe priorizar el estado `WITHDRAWN` y cancelar cualquier acción pendiente relacionada con la recomendación.

---

### US-03: Registro y centralización de evaluaciones del Hiring Manager

**Como** Hiring Manager

**Quiero** registrar evaluaciones estructuradas sobre las postulaciones compartidas por el reclutador.

**Para** centralizar el feedback del proceso de selección y facilitar la toma de decisiones conjunta.

#### Criterios de aceptación

**Escenario 1: Registro de evaluación**

- **Given** que el Hiring Manager tiene acceso a una postulación asignada.
- **When** completa la rúbrica de evaluación y envía el formulario.
- **Then** el sistema debe almacenar la evaluación asociada a la candidatura.

**Escenario 2: Notificación al reclutador**

- **Given** que la evaluación ha sido guardada correctamente.
- **When** finaliza la operación.
- **Then** el sistema debe informar al reclutador de que existe nuevo feedback disponible.

#### Caso borde

- Si un Hiring Manager intenta acceder a una candidatura para la que no tiene permisos, el sistema debe rechazar el acceso.

---

## 2. Product Backlog

La priorización se ha realizado utilizando la metodología **MoSCoW**, enfocada en la construcción de un MVP funcional.

| ID | Funcionalidad | Historia asociada | Prioridad | Justificación |
|----|--------------|------------------|------------|---------------|
| BL-01 | Autenticación, RBAC y aislamiento multi-tenant | Arquitectura base | Must Have | Es la base de seguridad y segregación de datos para un entorno SaaS B2B. |
| BL-02 | Pipeline de postulaciones y gestión de estados | US-01, US-02 | Must Have | Permite gestionar el ciclo de vida de las candidaturas. |
| BL-03 | Screening asistido por IA y almacenamiento de recomendaciones | US-01 | Must Have | Constituye la propuesta de valor principal del producto. |
| BL-04 | Control HITL y auditoría de decisiones | US-02 | Must Have | Garantiza cumplimiento regulatorio y supervisión humana. |
| BL-05 | Evaluaciones colaborativas y rúbricas | US-03 | Should Have | Mejora la colaboración entre reclutadores y managers. |
| BL-06 | Gestión de consentimientos GDPR | Requisito legal | Should Have | Permite cumplir los requisitos de privacidad y protección de datos. |
| BL-07 | Notificaciones operativas | US-01, US-03 | Could Have | Mejora la experiencia de usuario y la eficiencia operativa. |
| BL-08 | Portal básico para candidatos | Experiencia del candidato | Could Have | Aporta transparencia al proceso de selección. |
| BL-09 | Dashboard analítico avanzado | Roadmap futuro | Won't Have | Fuera del alcance del MVP inicial. |
| BL-10 | Integraciones con portales externos | Roadmap futuro | Won't Have | Incrementa significativamente la complejidad del MVP. |

---

## 3. Selección de la User Story más importante

### Historia seleccionada

**US-01: Evaluación automática y explicable de postulaciones en fase de Screening**

### Justificación de negocio

La principal problemática identificada en el PRD es el tiempo invertido en el filtrado manual de candidaturas. Esta historia permite validar la propuesta de valor principal de LTI mediante la automatización asistida por IA.

### Relación con la propuesta de valor

La diferenciación de LTI frente a otros ATS reside en la capacidad de proporcionar recomendaciones explicables basadas en IA, manteniendo siempre la supervisión humana sobre la decisión final.

---

## 4. Tickets de trabajo

### TEC-01: Modelo de datos para AIRecommendation

**Descripción**

Crear la estructura de persistencia necesaria para almacenar las recomendaciones generadas por IA.

**Objetivo**

Permitir el almacenamiento de puntuaciones, evidencias y estados asociados al proceso de screening.

**Dependencias**

- Ninguna.

---

### TEC-02: Endpoint de recepción de resultados del servicio de IA

**Descripción**

Implementar el mecanismo de recepción y almacenamiento de los resultados generados por el servicio de evaluación de IA.

**Objetivo**

Integrar el flujo asíncrono entre la plataforma y el servicio de IA.

**Dependencias**

- TEC-01.

---

### TEC-03: Interfaz de visualización de recomendaciones

**Descripción**

Desarrollar la interfaz que permita visualizar la puntuación, evidencias y estado de la recomendación.

**Objetivo**

Facilitar la revisión humana de los resultados generados por IA.

**Dependencias**

- TEC-01.

---

### TEC-04: Gestión de decisiones HITL y auditoría

**Descripción**

Implementar la lógica necesaria para registrar decisiones humanas sobre las recomendaciones generadas por IA.

**Objetivo**

Garantizar el cumplimiento del modelo Human-in-the-Loop.

**Dependencias**

- TEC-02
- TEC-03

---

## 5. Estimación de esfuerzo

Las estimaciones se han realizado utilizando Story Points y la secuencia de Fibonacci.

| Ticket | Story Points | Justificación |
|----------|-------------|---------------|
| TEC-01 | 2 | Trabajo estructural con baja incertidumbre técnica. |
| TEC-02 | 5 | Requiere integración asíncrona y validación de datos externos. |
| TEC-03 | 3 | Desarrollo de interfaz y visualización de información. |
| TEC-04 | 5 | Incluye reglas de negocio, auditoría y validaciones HITL. |

**Total estimado:** 15 Story Points.

---