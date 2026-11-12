# QA — estrategia inicial

QA se gobierna por el framework detallado en [`Elevari-repotransfer/docs/QUALITY_FRAMEWORK_GO_NO_GO.md`](https://github.com/Elevariapp/Elevari-repotransfer/blob/b-main/docs/QUALITY_FRAMEWORK_GO_NO_GO.md), hasta que se ratifique e importe como política canónica de este repositorio.

## Capas

- **Unitarias:** reglas puras y componentes/funciones aisladas.
- **Integración:** persistencia, API, auth y dependencias con fixtures/datos no productivos.
- **Contrato:** compatibilidad API con Web/móvil.
- **E2E:** journeys aprobados (exploración, detalle, disponibilidad, reserva, pago de prueba, confirmación).
- **Concurrencia/idempotencia:** solicitudes simultáneas y reintentos en entorno no productivo.
- **UX/accessibilidad:** pruebas manuales y herramientas automáticas; automatización no reemplaza lector de pantalla ni revisión humana.
- **Visual/compatibilidad:** navegadores, tamaños y dispositivos objetivo acordados.
- **Operación:** health checks, despliegue, backup/restauración y rollback.

## Reglas

- Primero inventariar tooling existente; no imponer Playwright/Cypress/u otra herramienta antes de confirmar repositorio.
- Usar datos ficticios/no productivos; nunca copiar secretos o datos personales a reportes.
- Registrar commit/build, entorno, pasos, resultados, defectos, severidad y enlaces a evidencia.
- Cero defectos S0/S1 en journeys afectados para GO de producción.
- Cualquier prueba faltante es pendiente, no aprobada.
