# Prompts utilizados para la generación de artefactos Agile

Los siguientes prompts fueron utilizados durante el proceso de generación de User Stories, Product Backlog y planificación inicial del desarrollo, tomando como referencia el documento PRD `LTI-MMX.md`.

---

## Prompt 1 – Generación inicial de User Stories

```text
Actúa como Product Owner Senior especializado en productos SaaS B2B.

Voy a proporcionarte un PRD llamado "LTI-MMX.md".

Analiza exclusivamente la información contenida en dicho documento y genera User Stories para el MVP del producto.

Para cada User Story utiliza la siguiente plantilla:

Título:
Como [rol]
Quiero [objetivo]
Para [beneficio]

Criterios de aceptación:
- Utiliza formato Given / When / Then.
- Incluye al menos 2 criterios de aceptación por historia.

Casos borde:
- Identifica al menos 1 caso borde relevante para cada historia.

Instrucciones:
- No inventes funcionalidades que no aparezcan en el PRD.
- Prioriza las funcionalidades necesarias para validar el MVP.
- Mantén un lenguaje orientado a producto y negocio.
- Genera 3 User Stories.
```

---

## Prompt 2 – Generación del Product Backlog

```text
Actúa como Product Manager Senior.

Utilizando exclusivamente la información del PRD "LTI-MMX.md" y las User Stories previamente generadas:

1. Construye un Product Backlog completo.
2. Prioriza los elementos utilizando la metodología MoSCoW:
   - Must Have
   - Should Have
   - Could Have
   - Won't Have

Para cada elemento incluye:

- ID
- Nombre de la funcionalidad
- Historia de usuario asociada
- Prioridad MoSCoW
- Justificación breve de la prioridad

Instrucciones:

- Prioriza pensando en la construcción de un MVP.
- Justifica cada prioridad desde la perspectiva de negocio.
- Evita entrar en detalles técnicos de implementación.
- Presenta el resultado en formato tabla.
- No añadas funcionalidades que no aparezcan en el PRD.
```

---

## Prompt 3 – Desglose técnico y estimación

```text
Actúa simultáneamente como:

- Product Manager Senior
- Product Owner
- Agile Delivery Lead

En el primer prompt esta el documento PRD adjuntado como fichero de nombre "LTI-MMX.md".

Tu misión es preparar la documentación necesaria para iniciar la implementación del producto.

Utiliza exclusivamente la información contenida en el PRD.

Genera el resultado en las siguientes secciones:

# 1. User Stories

Genera entre 3 y 5 User Stories utilizando la plantilla:

Título

Como [rol]
Quiero [objetivo]
Para [beneficio]

Criterios de aceptación:
- Formato Given / When / Then.
- Mínimo 2 escenarios por historia.

Casos borde:
- Mínimo 1 caso borde por historia.

# 2. Product Backlog

Construye un Product Backlog utilizando la metodología MoSCoW.

Para cada elemento incluye:

- ID
- Funcionalidad
- Historia asociada
- Prioridad
- Justificación

Presenta el backlog en formato tabla.

# 3. Selección de la User Story más importante

Selecciona una única User Story para iniciar el desarrollo.

Incluye:

- Nombre de la historia
- Justificación de negocio
- Relación con la propuesta de valor principal del producto

# 4. Desglose en Tickets de trabajo

Descompón la User Story seleccionada en tickets técnicos preparados para una reunión de Sprint Planning.

Para cada ticket incluye:

- ID
- Nombre
- Descripción
- Objetivo
- Dependencias si existen

Mantén un nivel de detalle práctico y realista.

No diseñes arquitecturas completas ni incluyas código.

# 5. Estimación de esfuerzo

Estima cada ticket utilizando Story Points con escala Fibonacci:

1, 2, 3, 5, 8, 13

Incluye:

- Ticket
- Estimación
- Breve justificación

# 6. Conclusiones

Explica:

- Qué prompt produjo los mejores resultados.
- Por qué fue más efectivo.
- Qué ventajas tuvo frente a los anteriores.

Restricciones:

- No inventes funcionalidades fuera del PRD.
- Mantén foco en MVP.
- Prioriza claridad y aplicabilidad para un equipo ágil.
- Evita detalles excesivos de arquitectura e infraestructura.
```

---

## Conclusiones

Durante la elaboración de la documentación se probaron diferentes estrategias de prompting:

1. El **Prompt 1** permitió generar las User Stories iniciales a partir del PRD.
2. El **Prompt 2** permitió construir y priorizar el Product Backlog utilizando la metodología MoSCoW.
3. El **Prompt 3** produjo el resultado más completo, conectando requisitos funcionales, backlog, planificación técnica y estimaciones de esfuerzo.

---
