# Product Backlog — LTI MVP

Backlog derivado del PRD (`LTI-MGB.md`, §3.1–§3.11) y de las 14 User Stories (`UserStories-MGB.md`). Priorización MoSCoW orientada a validar el MVP con organizaciones piloto en UE: pipeline operativo, diferenciador de IA explicable (HITL) y colaboración recruiter–manager.

---

## Backlog priorizado (alcance MVP)

| ID | Nombre de la funcionalidad | Historia de usuario asociada | Prioridad MoSCoW | Justificación breve de la prioridad |
|----|---------------------------|------------------------------|------------------|--------------------------------------|
| **PB-01** | Administración multi-tenant y control de acceso | US-01 | **Must Have** | Sin tenant aislado y roles RBAC no hay producto SaaS vendible ni confianza del cliente; es la base para operar con varias organizaciones en paralelo. |
| **PB-02** | Gestión de ofertas de empleo (`JobOffer`) | US-03 | **Must Have** | Toda candidatura depende de una vacante activa con criterios explícitos; sin ofertas no existe pipeline ni insumo para el screening con IA. |
| **PB-03** | Portal de candidatos y postulación (`Candidate`, `Application`) | US-04 | **Must Have** | El trial piloto (1 vacante, hasta 50 candidaturas) requiere captar candidatos de forma digital con consentimiento GDPR; sin portal no se alimenta el embudo. |
| **PB-04** | Gestión de candidaturas y detección de duplicados | US-05 | **Must Have** | Garantiza un registro fiable de candidatos y habilita la transición a `SCREENING`; evita datos inconsistentes que invaliden la evaluación con IA. |
| **PB-05** | Screening asistido por IA (`AIRecommendation`) | US-06 | **Must Have** | Es el diferenciador central del UVP y la hipótesis H-B2 (reducir tiempo en preselección); sin screening el MVP no compite frente a un ATS básico. |
| **PB-06** | Revisión humana obligatoria de recomendaciones IA (HITL) | US-07 | **Must Have** | Condición de adopción en mercado UE: la IA propone pero el humano decide; sin HITL no se valida la propuesta de valor ni la gobernanza del producto. |
| **PB-07** | Colaboración recruiter–hiring manager (`Evaluation`) | US-08 | **Must Have** | Resuelve el pipeline fragmentado (problema §1.2) y valida la hipótesis H-B3; es imprescindible para demostrar co-decisión en la misma candidatura. |
| **PB-08** | Programación de entrevistas con integración de calendario | US-09 | **Must Have** | Aborda uno de los problemas estructurales del PRD (coordinación de entrevistas) y reduce time-to-hire; sin scheduling el flujo se corta tras el screening. |
| **PB-09** | Confirmación de franja horaria por el candidato | US-10 | **Must Have** | Completa el caso de uso UC-03; sin confirmación del candidato la coordinación vuelve al email manual y se pierde el beneficio operativo del scheduling. |
| **PB-10** | Notificaciones operativas del pipeline | US-11 | **Must Have** | Los flujos asíncronos (evaluación IA, solicitud de feedback, confirmación de entrevista) dependen de avisos automáticos; sin notificaciones el proceso se estanca. |
| **PB-11** | Política de retención y cumplimiento GDPR por tenant | US-02 | **Should Have** | Refuerza el posicionamiento GDPR-by-design en UE y habilita la métrica de cumplimiento del Lean Canvas; el consentimiento en postulación (US-04) cubre el mínimo legal para operar. |
| **PB-12** | Automatización básica de workflow del pipeline | US-11, US-08 | **Should Have** | Los recordatorios (p. ej. evaluación pendiente a 48 h) reducen fricción operativa sin intervención manual; mejora retención del piloto pero el flujo principal funciona sin ellos. |
| **PB-13** | Búsqueda y filtrado full-text | US-12 | **Should Have** | Aumenta productividad del recruiter con volumen moderado de candidaturas; en pilotos pequeños (≤50 postulaciones) el valor es alto pero no bloquea la validación del core. |
| **PB-14** | Dashboard de analítica básica del pipeline | US-13 | **Could Have** | Demuestra ROI y soporta métricas clave (tiempo en `SCREENING`, ratio de overrides), pero la validación inicial puede hacerse con datos exportados o medición manual en 2–3 tenants piloto. |
| **PB-15** | Copiloto del reclutador (asistencia generativa acotada) | US-14 | **Could Have** | Complementa el screening en fases posteriores y mejora percepción de productividad; no es necesario para validar las hipótesis críticas H-B2 ni H-B3 en el primer release. |

---

## Backlog explícitamente excluido (Won't Have — MVP)

Elementos referenciados en el PRD como **fuera de alcance MVP** (§3, limitaciones por funcionalidad y Roadmap §10). No tienen User Story asociada.

| ID | Nombre de la funcionalidad | Historia de usuario asociada | Prioridad MoSCoW | Justificación breve de la prioridad |
|----|---------------------------|------------------------------|------------------|--------------------------------------|
| **PB-W01** | Multiposting automático a portales externos (LinkedIn, InfoJobs) | — | **Won't Have** | Excluido explícitamente en §3.2; no es necesario para validar el pipeline interno con candidatos que aplican vía portal LTI. |
| **PB-W02** | SSO SAML custom, SCIM y sub-organizaciones | — | **Won't Have** | Fuera de alcance §3.1; el IdP gestionado estándar cubre la autenticación del MVP sin complejidad enterprise. |
| **PB-W03** | CRM de talento y nurturing por campañas | — | **Won't Have** | Excluido en §3.3; el MVP se centra en procesos activos, no en gestión de pools de talento a largo plazo. |
| **PB-W04** | Videollamada integrada y salas de vídeo propias | — | **Won't Have** | Excluido en §3.5 y §3.6; las entrevistas usan enlace externo en el campo de `Interview`, suficiente para el piloto. |
| **PB-W05** | Notificaciones por SMS, WhatsApp y push móvil nativo | — | **Won't Have** | Excluido en §3.7; email transaccional e in-app cubren los eventos críticos del pipeline en MVP. |
| **PB-W06** | Editor visual de workflows y automatización tipo Zapier | — | **Won't Have** | Excluido en §3.9; el catálogo cerrado de reglas (≤5 tipos) satisface la automatización básica sin sobreingeniería. |
| **PB-W07** | BI avanzado, benchmarking entre tenants y predicción de contratación | — | **Won't Have** | Excluido en §3.10; la analítica básica (PB-14) es suficiente para demostrar valor inicial sin inversión en data warehouse. |
| **PB-W08** | Matching predictivo candidato–vacante a escala | — | **Won't Have** | Fuera de alcance MVP (§3, Roadmap §10); el screening por vacante (PB-05) cubre la propuesta de IA del release inicial. |
| **PB-W09** | Asistentes de entrevista por voz y agentes autónomos multi-paso | — | **Won't Have** | Excluido en Roadmap §10 y §3.11; contradice el modelo HITL y eleva riesgo ético y de coste en fase de validación. |
| **PB-W10** | Mobility interna, BPM enterprise y SSO on-premise | — | **Won't Have** | Excluido en Roadmap §10; no aplica al segmento primario (pymes 50–500 empleados) ni al modelo SaaS cloud del MVP. |

---

## Resumen ejecutivo MoSCoW

| Prioridad | Elementos | % del backlog MVP |
|-----------|-----------|-------------------|
| **Must Have** | PB-01 a PB-10 | 10 ítems — núcleo indispensable para lanzar y validar |
| **Should Have** | PB-11 a PB-13 | 3 ítems — refuerzan cumplimiento, eficiencia y usabilidad |
| **Could Have** | PB-14, PB-15 | 2 ítems — valor incremental post-validación del core |
| **Won't Have** | PB-W01 a PB-W10 | 10 ítems — exclusiones documentadas del PRD |

**Secuencia recomendada de entrega:** PB-01 → PB-02 → PB-03 → PB-04 → PB-05 + PB-06 (screening + HITL) → PB-10 (notificaciones transaccionales) → PB-07 → PB-08 + PB-09 → PB-11 → PB-12 → PB-13 → PB-14 → PB-15.

Esta secuencia sigue la cadena operativa del PRD (§4.6): vacante → candidatura → screening → colaboración → entrevista, con capacidades de soporte y diferenciación añadidas en iteraciones posteriores.