# Política de release GO / NO-GO

## Autoridad

El framework detallado de calidad se mantiene en `docs/QUALITY_FRAMEWORK_GO_NO_GO.md` del repositorio de referencia `Elevariapp/Elevari-repotransfer`. Su adopción como política canónica aquí está pendiente de revisión/ratificación por producto, diseño, ingeniería, seguridad y operación. Esta página resume la regla operativa y no sustituye esa revisión.

## GO

Un release es candidato a GO solo si todos los gates aplicables tienen evidencia vigente, no hay defectos S0/S1 abiertos, rollback/recuperación es viable y los responsables autorizados aprueban expresamente.

## NO-GO

Se decide NO-GO si un gate falla o carece de evidencia, hay bloqueantes, no se puede recuperar un incidente crítico, o falta autorización. No se promedian gates. Staging GO no equivale a producción GO.

## Gates a evaluar

1. Producto/UX y QA visual.
2. Accesibilidad e inclusión.
3. Funcionalidad, datos y contrato API.
4. Seguridad, privacidad y permisos.
5. Rendimiento y confiabilidad con umbrales acordados antes de medir.
6. Operación, backup, restauración y rollback.
7. Compatibilidad Web/móvil y distribución.
8. Soporte, analítica y preparación del negocio.

## Registro requerido

Componente/plataforma, entorno, commit/tag/build, fecha, owners, estado por gate, links a CI/reportes, defectos por severidad, riesgos/excepciones con expiración, backup/rollback, monitoreo posterior, resultado y aprobadores.

## Estado inicial

No hay evidencia de este repositorio que autorice producción. El plan y los documentos son preparación; se requiere ejecutar las pruebas y reunir evidencia para cada release.
