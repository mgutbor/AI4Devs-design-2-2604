# 1. Conclusiones

## Qué prompt produjo los mejores resultados

El **Prompt 3** (documento integral: User Stories + Backlog MoSCoW + selección de historia + tickets + estimación + conclusiones) produjo el resultado más completo y accionable para el equipo.

## Por qué fue más efectivo

1. **Visión end-to-end en un solo artefacto:** conecta requisitos de negocio (PRD) con planificación de sprint, eliminando la brecha entre «qué construir» y «por dónde empezar».
2. **Roles múltiples en un único contexto:** al actuar simultáneamente como PM, PO y Agile Delivery Lead, el output equilibra valor de negocio, criterios de aceptación verificables y descomposición práctica para desarrollo.
3. **Estructura obligatoria y completa:** las seis secciones forzaron coherencia entre historias, priorización MoSCoW, historia seleccionada, tickets dependientes y estimaciones — sin dejar huecos entre artefactos ágiles.
4. **Restricciones explícitas bien calibradas:** «solo PRD», «foco MVP», «sin arquitectura excesiva» mantuvieron el detalle en el nivel correcto para Sprint Planning, no en diseño técnico prematuro.

---

# 2. Selección de la User Story más importante

## Historia seleccionada: US-01 — Configuración del tenant y control de acceso

## Justificación de negocio

US-01 es el prerrequisito de cualquier valor comercial del producto. LTI se define como SaaS multi-tenant (§1.1, §3.0): cada cliente es una Organization con usuarios internos en roles fijos. Sin esta capacidad:

- No se puede activar el trial de 14 días por tenant.
- No se garantiza aislamiento de datos entre clientes (riesgo legal y pérdida de confianza).
- Ningún flujo posterior (vacantes, candidaturas, IA, colaboración) puede operar con permisos correctos.

Es la historia con menor dependencia upstream y mayor dependencia downstream: todo el backlog MVP (PB-02 a PB-15) asume tenant configurado, usuarios con roles y acceso acotado por organization_id.

## Relación con la propuesta de valor principal

La UVP del producto («El ATS europeo que reduce la preselección manual con IA explicable y colaboración nativa — sin sacrificar control humano ni cumplimiento GDPR») descansa en tres pilares: confianza operativa, cumplimiento y pipeline colaborativo.

US-01 habilita directamente los dos primeros:

- Confianza operativa: RBAC fijo (RECRUITER, HIRING_MANAGER, ORG_ADMIN) alinea permisos con el flujo real recruiter–manager descrito en el PRD.
- Cumplimiento GDPR: el aislamiento por tenant es requisito transversal (§3.0) y base para políticas de retención y AuditLog posteriores.

Sin US-01, el diferenciador de IA explicable (US-06/US-07) y la colaboración nativa (US-08) no pueden desplegarse de forma segura en un entorno multi-cliente.