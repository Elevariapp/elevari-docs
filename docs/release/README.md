# Release — flujo y evidencias

## Estados

`Propuesto → En desarrollo → En revisión → Aprobado para staging → Validado en staging → GO/NO-GO producción → Observación post-release`.

Una transición requiere evidencia del estado anterior y responsables nombrados. Los estados de plataforma Web, API/infra y móvil pueden ser independientes.

## Antes de release

- Cambio y alcance identificados; arquitectura/documentos revisados.
- Build/commit fijado; CI y pruebas requeridas adjuntas.
- QA UX, visual, accesibilidad, seguridad y compatibilidad aplicable.
- Migración/contrato API revisados.
- Secrets, health, logs/alertas, soporte, backup/restauración y rollback listos para producción.
- Registro GO/NO-GO completado por los owners autorizados.

## Después

Smoke tests no destructivos, observación de errores/latencia/salud de dependencias, respuesta a incidencias y registro de resultado. Activar rollback bajo condiciones acordadas.

Ver checklist completo en [GO/NO-GO](GO_NO_GO.md).
