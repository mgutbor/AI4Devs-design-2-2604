Escala Fibonacci: 1, 2, 3, 5, 8, 13.

| Ticket | Estimación (SP) | Justificación |
|--------|-----------------|---------------|
| TK-01 | **3** | Modelo acotado (2 entidades, roles fijos); migraciones y relaciones directas. |
| TK-02 | **5** | Integración IdP externo con resolución de sesión y mapeo usuario–tenant; incertidumbre moderada de configuración. |
| TK-03 | **5** | RBAC con 3 roles fijos sobre API; lógica clara pero transversal a todos los endpoints futuros. |
| TK-04 | **5** | Patrón multi-tenant obligatorio en toda la capa de datos; requiere disciplina y pruebas de regresión. |
| TK-05 | **3** | CRUD acotado de usuarios con validaciones RBAC; alcance bien delimitado en §3.1. |
| TK-06 | **5** | UI admin con listado, formularios y feedback de errores; sin complejidad de flujos múltiples. |
| TK-07 | **2** | Caso borde acotado: guard de acceso y pantalla informativa. |
| TK-08 | **2** | Registro append-only de eventos admin; alcance mínimo para MVP. |
| TK-09 | **3** | Pruebas de escenarios de US-01; depende de tickets previos integrados. |
| **Total US-01** | **33 SP** | ~2 sprints para un equipo de 2–3 ingenieros, asumiendo velocidad inicial de 15–18 SP/sprint. |