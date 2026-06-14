# User Stories — MVP LTI (Applicant Tracking System)

Historias derivadas exclusivamente del PRD (`LTI-MGB.md`), priorizando las capacidades necesarias para validar el MVP: pipeline colaborativo, screening con IA explicable (HITL), cumplimiento GDPR y coordinación de entrevistas.

---

## US-01 — Configuración del tenant y control de acceso

**Título:** Configurar la organización y gestionar usuarios con roles RBAC

**Como** Organization Admin  
**Quiero** configurar mi organización, invitar usuarios internos y asignarles roles fijos (`ORG_ADMIN`, `RECRUITER`, `HIRING_MANAGER`)  
**Para** operar LTI como SaaS multi-tenant con aislamiento de datos y permisos claros desde el primer día

**Criterios de aceptación:**

- **Given** que soy Organization Admin autenticado vía IdP externo  
  **When** creo o importo un usuario y le asigno un rol  
  **Then** el usuario solo puede acceder a recursos de su `organization_id` según los permisos de su rol

- **Given** que existen usuarios en dos organizaciones distintas  
  **When** un Recruiter de la Organización A intenta consultar datos de la Organización B  
  **Then** el sistema deniega el acceso y no expone información de otro tenant

**Casos borde:**

- Un usuario sin rol asignado o con credenciales válidas pero sin pertenencia a una `Organization` activa no puede acceder a funcionalidades de negocio.

---

## US-02 — Política de retención y cumplimiento GDPR por tenant

**Título:** Definir la política de retención de datos del tenant

**Como** Organization Admin  
**Quiero** configurar la política de retención de datos de mi organización  
**Para** cumplir con GDPR-by-design y reducir el riesgo legal en el mercado UE

**Criterios de aceptación:**

- **Given** que soy Organization Admin de una `Organization` activa  
  **When** configuro la política de retención de datos del tenant  
  **Then** la configuración queda asociada a mi `organization_id` y aplicable a los datos del tenant

- **Given** que la política de retención está configurada  
  **When** se ejecutan acciones sensibles sobre datos de candidatos (acceso PII, cambios de estado, revisión de `AIRecommendation`)  
  **Then** el sistema registra la acción en `AuditLog` de forma trazable

**Casos borde:**

- Una organización sin política de retención configurada puede operar, pero el sistema debe indicar al admin que la configuración está pendiente (métrica de cumplimiento del Lean Canvas).

---

## US-03 — Creación y publicación de ofertas de empleo

**Título:** Crear y publicar vacantes con criterios de evaluación explícitos

**Como** Recruiter  
**Quiero** crear ofertas de empleo con requisitos obligatorios, opcionales y criterios de evaluación, asociar hiring managers y publicarlas  
**Para** alinear expectativas con los hiring managers y alimentar el screening asistido por IA

**Criterios de aceptación:**

- **Given** que soy Recruiter autenticado  
  **When** defino título, descripción, requisitos y criterios de evaluación de una `JobOffer` y la publico como activa  
  **Then** la vacante queda disponible para recibir nuevas `Application` vinculadas automáticamente

- **Given** que una `JobOffer` está activa  
  **When** asocio uno o más Hiring Managers a la vacante  
  **Then** esos managers quedan vinculados a la oferta y podrán colaborar en las postulaciones de esa vacante

**Casos borde:**

- Si la organización ha alcanzado el máximo de `JobOffer` activas según su plan de suscripción, el sistema impide publicar una nueva vacante e informa del límite.

---

## US-04 — Postulación del candidato vía portal

**Título:** Aplicar a una vacante activa desde el portal del candidato

**Como** Candidate  
**Quiero** postularme a una oferta activa completando un formulario, subiendo mi CV y otorgando consentimiento GDPR  
**Para** participar en el proceso de selección con una experiencia simple y conforme a normativa

**Criterios de aceptación:**

- **Given** que existe una `JobOffer` activa  
  **When** completo el formulario de candidatura, subo un CV en formato soportado (PDF/DOC) y acepto el consentimiento GDPR  
  **Then** se crea una `Application` en estado `NEW` con trazabilidad de la fuente de captación

- **Given** que he enviado mi candidatura  
  **When** accedo al portal del candidato  
  **Then** puedo consultar el estado de mi propia postulación sin ver información de otros candidatos

**Casos borde:**

- Si el candidato intenta subir un CV en un formato no soportado en MVP, el sistema rechaza la postulación e indica los formatos aceptados (PDF/DOC prioritarios).

---

## US-05 — Gestión de candidaturas y detección de duplicados

**Título:** Revisar postulaciones nuevas y resolver candidatos duplicados

**Como** Recruiter  
**Quiero** revisar las candidaturas en estado `NEW`, validar duplicados sugeridos y avanzar candidatos al screening  
**Para** mantener un registro único y fiable de candidatos en mi organización

**Criterios de aceptación:**

- **Given** que existe una `Application` en estado `NEW` con consentimiento GDPR y CV adjunto  
  **When** reviso la candidatura y la transiciono a `SCREENING`  
  **Then** la postulación cambia de estado y queda lista para evaluación asistida por IA

- **Given** que el sistema detecta un posible duplicado (mismo email u otro criterio de similitud en la `Organization`)  
  **When** reviso la sugerencia de duplicado  
  **Then** debo confirmar manualmente antes de fusionar registros; la fusión no ocurre de forma automática

**Casos borde:**

- Si el candidato retira su candidatura (`WITHDRAWN`) mientras un proceso de screening está en curso, el sistema no genera una nueva `AIRecommendation`.

---

## US-06 — Screening asistido por IA de candidatos

**Título:** Evaluar automáticamente candidaturas en screening con IA explicable

**Como** Recruiter  
**Quiero** que al pasar una candidatura a `SCREENING` el sistema evalúe el perfil/CV frente a los criterios de la vacante  
**Para** reducir el tiempo de preselección manual y obtener recomendaciones explicables como base de decisión

**Criterios de aceptación:**

- **Given** una `Application` en estado `SCREENING` con CV válido, consentimiento GDPR y cuota de inferencia IA disponible  
  **When** se dispara la evaluación  
  **Then** el sistema encola el proceso de forma asíncrona, actualiza `screening_job_status` a `QUEUED` y genera una `AIRecommendation` en estado `PENDING_REVIEW` con puntuación, razones y evidencia citada del CV

- **Given** que la evaluación IA ha finalizado correctamente  
  **When** consulto la candidatura  
  **Then** recibo una notificación (email e in-app) indicando que la evaluación está lista para revisión humana

**Casos borde:**

- Si la cuota de inferencia IA del tenant está agotada, el sistema no procesa la evaluación, informa al Recruiter y notifica al Organization Admin para que pueda gestionar upgrade o esperar al siguiente ciclo mensual.

---

## US-07 — Revisión humana obligatoria de recomendaciones IA (HITL)

**Título:** Resolver recomendaciones de IA antes de avanzar en el pipeline

**Como** Recruiter  
**Quiero** revisar cada `AIRecommendation` y resolverla como `ACCEPTED`, `REJECTED` u `OVERRIDDEN`  
**Para** mantener el control humano sobre las decisiones de contratación y cumplir el modelo HITL del producto

**Criterios de aceptación:**

- **Given** una `AIRecommendation` en estado `PENDING_REVIEW`  
  **When** la reviso y la marco como `ACCEPTED`, `REJECTED` u `OVERRIDDEN` con motivo en caso de override  
  **Then** la resolución queda registrada en `AuditLog` junto con versión de modelo y timestamp de la inferencia

- **Given** que la última `AIRecommendation` revisada está en `REJECTED`  
  **When** intento transicionar la `Application` a `INTERVIEW`  
  **Then** el sistema bloquea la transición (error 409) y solo permite avanzar a entrevista si la recomendación está en `ACCEPTED` u `OVERRIDDEN`

**Casos borde:**

- Si un Hiring Manager intenta resolver una `AIRecommendation`, el sistema rechaza la acción con 403 Forbidden (solo el Recruiter puede resolverla en MVP).

---

## US-08 — Colaboración recruiter–hiring manager en evaluaciones

**Título:** Solicitar y consolidar evaluaciones estructuradas del hiring manager

**Como** Hiring Manager  
**Quiero** completar una evaluación estructurada (puntuación, comentario y recomendación) sobre una candidatura compartida  
**Para** aportar mi criterio de contratación sin depender de email u hojas de cálculo

**Criterios de aceptación:**

- **Given** que estoy asignado a la `JobOffer` y el Recruiter ha solicitado mi evaluación  
  **When** accedo a la `Application` y completo una `Evaluation` con puntuación, comentario y recomendación (`proceed` / `hold` / `reject`)  
  **Then** mi evaluación queda persistida y el Recruiter recibe una notificación de «Nueva evaluación recibida»

- **Given** que existen evaluaciones de varios Hiring Managers  
  **When** el Recruiter consolida el feedback  
  **Then** puede transicionar la `Application` a `INTERVIEW` o `REJECTED` como único rol autorizado para cambiar estados

**Casos borde:**

- Si un Hiring Manager no asignado a la `JobOffer` intenta acceder a la candidatura, el sistema deniega el acceso con 403 Forbidden.

---

## US-09 — Programación de entrevistas con integración de calendario

**Título:** Coordinar entrevistas consultando disponibilidad de calendarios corporativos

**Como** Recruiter  
**Quiero** programar una entrevista para una candidatura en estado `INTERVIEW`, definiendo tipo, duración y participantes, y consultando disponibilidad vía calendario corporativo  
**Para** reducir la fricción en la coordinación de agendas y acelerar el time-to-hire

**Criterios de aceptación:**

- **Given** una `Application` en estado `INTERVIEW` y al menos un usuario interno con OAuth de calendario válido  
  **When** inicio la programación de entrevista con tipo, duración y participantes  
  **Then** el sistema crea una `Interview` en estado `DRAFT`, consulta disponibilidad (free/busy) y calcula franjas candidatas en una ventana configurable

- **Given** que se han calculado franjas disponibles  
  **When** el sistema genera un enlace seguro con token temporal para el candidato  
  **Then** el candidato recibe un email con el enlace sin exponer datos de otros candidatos

**Casos borde:**

- Si el token OAuth de calendario de un participante interno ha expirado, el sistema devuelve error `CALENDAR_REAUTH_REQUIRED` y solicita reconexión; la `Interview` permanece en `DRAFT`.

---

## US-10 — Confirmación de franja horaria por el candidato

**Título:** Seleccionar franja de entrevista desde enlace seguro

**Como** Candidate  
**Quiero** acceder a un enlace seguro, ver las franjas propuestas y seleccionar la que me convenga  
**Para** confirmar mi entrevista sin intercambiar múltiples correos con el equipo de selección

**Criterios de aceptación:**

- **Given** que he recibido un enlace seguro con token válido y TTL activo (p. ej. 72 h)  
  **When** accedo al enlace y selecciono una franja horaria  
  **Then** la `Interview` pasa a estado `SCHEDULED`, se crea el evento en los calendarios de los participantes internos y todos los involucrados reciben confirmación por email (y notificación in-app para usuarios internos)

- **Given** que he confirmado una franja  
  **When** el Recruiter consulta la ficha de la `Application`  
  **Then** visualiza la entrevista confirmada con fecha, hora y referencia al evento de calendario

**Casos borde:**

- Si el token del enlace ha expirado o es inválido, el candidato ve una página de error genérica y el Recruiter puede regenerar el enlace con nuevas franjas.

---

## US-11 — Notificaciones operativas del pipeline

**Título:** Recibir avisos automáticos de eventos relevantes del proceso de selección

**Como** Recruiter  
**Quiero** recibir notificaciones por email e in-app cuando ocurran eventos clave del pipeline (nueva candidatura, evaluación IA lista, entrevista confirmada, cambio de estado)  
**Para** no depender de seguimiento manual y evitar que el proceso se estanque

**Criterios de aceptación:**

- **Given** que ocurre un evento de dominio relevante (nueva `Application`, `AIRecommendation` lista, `Interview` confirmada o cambio de estado)  
  **When** el Notification Service procesa el evento  
  **Then** recibo la notificación en el canal configurado (email transaccional e in-app) según plantillas estáticas

- **Given** que tengo notificaciones pendientes en la UI  
  **When** las marco como leídas  
  **Then** el estado de lectura se actualiza correctamente

**Casos borde:**

- Si un Hiring Manager no completa su `Evaluation` en 48 horas, el sistema envía recordatorio automático al Hiring Manager y copia al Recruiter, sin cambiar el estado de la candidatura.

---

## US-12 — Búsqueda y filtrado de candidatos y postulaciones

**Título:** Localizar candidatos y postulaciones mediante búsqueda full-text y filtros

**Como** Recruiter  
**Quiero** buscar candidatos, postulaciones y vacantes por texto y aplicar filtros de pipeline (estado, vacante, fechas)  
**Para** encontrar rápidamente la información que necesito cuando gestiono volumen moderado de candidaturas

**Criterios de aceptación:**

- **Given** que existen `Candidate`, `Application` y `JobOffer` indexados en mi tenant  
  **When** realizo una búsqueda por nombre, email, título de vacante o texto extraído del CV  
  **Then** obtengo resultados filtrados exclusivamente por mi `organization_id`

- **Given** que aplico filtros por estado de `Application`, `JobOffer` o rango de fechas  
  **When** ejecuto la búsqueda  
  **Then** los resultados reflejan los criterios seleccionados

**Casos borde:**

- Un Hiring Manager solo ve resultados acotados a las vacantes a las que está asignado, no de toda la organización.

---

## US-13 — Dashboard de analítica básica del pipeline

**Título:** Visualizar métricas agregadas del embudo de contratación

**Como** Organization Admin  
**Quiero** consultar un dashboard con métricas agregadas del pipeline (candidatos por estado, tiempo medio en `SCREENING`, conversión a `HIRED`, ratio de overrides de IA)  
**Para** demostrar el ROI del producto y tomar decisiones operativas basadas en datos

**Criterios de aceptación:**

- **Given** que mi organización tiene candidaturas en distintos estados  
  **When** accedo al dashboard de analítica  
  **Then** visualizo conteos por estado, tiempo medio en `SCREENING`, tasa de conversión a `HIRED` y ratio `OVERRIDDEN` vs `ACCEPTED` en recomendaciones IA

- **Given** que necesito reporting externo  
  **When** solicito exportación de datos  
  **Then** puedo descargar un CSV con los agregados disponibles, filtrables por vacante y rango de fechas

**Casos borde:**

- En plan Starter, los datos históricos disponibles en el dashboard están limitados a ≤12 meses según las restricciones del plan.

---

## US-14 — Copiloto del reclutador para redacción y síntesis

**Título:** Obtener asistencia generativa para resumir CVs y redactar feedback

**Como** Recruiter  
**Quiero** solicitar al copiloto acciones concretas (resumir CV, proponer preguntas de entrevista alineadas a la vacante, borrador de feedback)  
**Para** acelerar tareas de redacción sin automatizar decisiones ni comunicaciones al candidato

**Criterios de aceptación:**

- **Given** una `Application` con contexto disponible (`Candidate`, `JobOffer`, `Attachment`, `Evaluation`)  
  **When** solicito explícitamente una acción del copiloto (resumen, preguntas o borrador de feedback)  
  **Then** recibo un borrador editable generado por IA, acotado a datos de mi `organization_id`, con aviso de revisión humana obligatoria

- **Given** que he recibido un borrador del copiloto  
  **When** lo edito y lo utilizo en una `Evaluation` o nota interna  
  **Then** nada se publica automáticamente al candidato ni cambia el estado de la `Application`; la solicitud queda registrada en `AuditLog`

**Casos borde:**

- Si la cuota mensual de tokens/inferencias IA (compartida con screening) está agotada, el sistema informa del límite y no genera el borrador hasta que haya cuota disponible.

---

## Resumen de priorización MVP

| Prioridad | Historias | Justificación (PRD) |
|-----------|-----------|---------------------|
| **P0 — Bloqueante** | US-01, US-03, US-04, US-05, US-06, US-07 | Base multi-tenant + pipeline origen + diferenciador IA (H-B2) |
| **P1 — Core flujo** | US-08, US-09, US-10, US-11 | Colaboración (H-B3) + scheduling (time-to-hire) + notificaciones |
| **P2 — Valor añadido** | US-02, US-12, US-13, US-14 | GDPR, productividad, ROI demostrable, copiloto complementario |

Estas 14 historias cubren las **11 funcionalidades MVP** (§3.1–§3.11) y los **3 casos de uso principales** (UC-01, UC-02, UC-03), sin incluir capacidades explícitamente fuera de alcance (multiposting, SSO SAML custom, CRM de talento, videollamada integrada, etc.).