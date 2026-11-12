# Elevari — documentación canónica

Este repositorio es el punto propuesto para la documentación canónica compartida de Elevari. La canonicalidad aplica cuando el responsable del producto ratifique esta gobernanza. Hasta entonces, los documentos en el repositorio existente `Elevariapp/Elevari-repotransfer` siguen siendo fuentes documentales de referencia; las diferencias deben registrarse y resolverse, no sobrescribirse silenciosamente.

## Repositorios

- Web existente: [`Elevariapp/elevari-web`](https://github.com/Elevariapp/elevari-web)
- Documentación existente: `Elevariapp/elevari-docs` (este repositorio)
- Plataforma/API propuesta: `Elevariapp/elevari-platform` (no creada aún)
- Móvil propuesta: `Elevariapp/elevari-mobile` (no creada aún)
- Operaciones propuesta: `Elevariapp/elevari-ops` (no creada aún)
- Código de plataforma actual de referencia: [`Elevariapp/Elevari-repotransfer`](https://github.com/Elevariapp/Elevari-repotransfer)

Los repositorios propuestos no se crean ni se mueve código sin confirmación explícita de nombres, visibilidad y ownership.

## Primeros documentos

- [Mapa de repositorios y límites](docs/REPOSITORY_MAP.md)
- [Convenciones de nombres y estructura](docs/NAMING_AND_STRUCTURE.md)
- [Backlog de preparación](docs/DEVELOPMENT_BACKLOG.md)
- [Política de release GO/NO-GO](docs/release/GO_NO_GO.md)
- [Arquitectura — decisiones pendientes](docs/architecture/README.md)
- [QA — estrategia inicial](docs/qa/README.md)

## Regla de trabajo

Antes de cambiar código, documentar objetivo, justificación, fuentes consultadas, archivos afectados, riesgos, implementación, pruebas y rollback. Marcar los estados separadamente: **documentado**, **implementado**, **probado** y **validado en el entorno objetivo**. No declarar producción lista sin evidencia y aprobación GO.
