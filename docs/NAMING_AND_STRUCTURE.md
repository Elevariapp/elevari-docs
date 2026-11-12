# Convenciones de nombres y estructura

## Nombres

- Repositorios: `elevari-<dominio>` en minúsculas y kebab-case; nombres definitivos requieren aprobación.
- Carpetas: minúsculas; kebab-case para nombres de dominio (`property-detail`), excepto convenciones impuestas por framework.
- React: componentes en PascalCase; hooks con prefijo `use`; tests junto al módulo o en `tests/` según decisión del repo.
- API: rutas REST versionadas y nombres de recursos coherentes con el contrato publicado. No agregar rutas hasta revisar las existentes.
- DB: nombres consistentes con schema/migraciones existentes; no renombrar tablas por estética.
- ADR: `NNNN-titulo-corto.md`, consecutivo, inmutable tras aceptación; reemplazar con ADR nuevo.
- Documentos: título explícito, estado (propuesto/aprobado/superado), responsable, fecha/revisión y enlaces a evidencia.

## Árbol propuesto por repositorio

Estos son mapas de carpetas planeados, no creación de directorios vacíos ni compromiso de reescribir el repo existente.

### `elevari-platform` — pendiente de creación/aprobación

```text
README.md
api/                 # Fastify existente solo tras migración aprobada
prisma/              # schema y migraciones, según estructura fuente
contracts/           # OpenAPI/contratos publicados
src/                 # conservar la organización que confirme el inventario
 tests/              # unitarias e integración (decidir según stack existente)
docs/                # guías locales; enlazar documentación canónica
```

### `elevari-web` — existente

```text
README.md
app/                 # Next.js App Router, confirmar contra repo fuente
components/          # componentes compartidos de interfaz
features/            # discovery, property, booking, account: organizar tras inventario
lib/api/             # cliente de la API oficial; no consultas directas a DB
public/
tests/               # unitarias, integración y E2E según tooling aprobado
docs/                # instrucciones locales y enlaces a elevari-docs
```

No se crea `features/` ni se mueve el Next.js existente hasta verificar cómo está organizado el repo fuente y aprobar la migración.

### `elevari-mobile` — pendiente de creación/aprobación

```text
README.md
src/
  app/                # solo si Expo Router se aprueba
  navigation/         # alternativa si se aprueba otra navegación
  features/           # auth, discovery, property, booking
  design-system/
  services/api/       # consumidor del contrato oficial
  platform/           # deep links, push, secure storage según diseño aprobado
tests/
docs/
```

Framework, navegación, almacenamiento y distribución requieren decisión técnica antes de crear el esqueleto.

### `elevari-docs` — existente

```text
README.md
docs/
  architecture/
  api/
  product/
  ux/
  qa/
  release/
  operations/
  adr/
```

### `elevari-ops` — pendiente de creación/aprobación

```text
README.md
docs/
  environments/
  runbooks/
  incidents/
  backup-restore/
  release/
```

Infraestructura como código, proveedores y scripts de despliegue no se incorporan hasta aprobación explícita.

## Reglas de estructura

- Organizar por responsabilidad/dominio donde el framework lo permita; respetar la estructura establecida por Next.js, Expo/Fastify u otra tecnología aprobada.
- No duplicar contratos, políticas ni documentos canónicos en varios repositorios: enlazar y versionar.
- No crear microservicios, paquetes compartidos o repositorios nuevos solo para lograr simetría visual.
- No versionar `.env` con secretos, llaves de firma, datos de producción, dumps ni artefactos generados.
