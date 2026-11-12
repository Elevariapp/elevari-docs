# Arquitectura — decisiones y referencias

## Arquitectura documentada actualmente

El documento `INFRASTRUCTURE.md` del repo [`Elevariapp/Elevari-repotransfer`](https://github.com/Elevariapp/Elevari-repotransfer/blob/b-main/INFRASTRUCTURE.md) describe Next.js/React/TypeScript, API REST Fastify, Prisma, PostgreSQL, Redis, MinIO, Docker Compose y Nginx. Los nuevos flujos de negocio deben pasar por la API oficial.

## Recomendación de infraestructura y servicios

La recomendación, límites y tareas de verificación para AWS, Cloudflare, Wompi y el asistente están en [`PLATFORM_DECISIONS.md`](PLATFORM_DECISIONS.md). Es una decisión documental propuesta, no prueba de infraestructura desplegada ni de integración ya verificada.

## Bloqueos de decisión

- `README_ADMIN.md` prescribe Supabase directo y contradice la infraestructura/roadmap que describen el retiro del legacy para los nuevos flujos. No desarrollar Admin hasta confirmar el código vigente y una decisión aprobada.
- El roadmap propone Expo/React Native; confirmar stack y repositorio móvil antes de crear código.
- Separación de repositorios y ubicación final de documentos canónicos requieren aprobación.
- No introducir proveedor cloud, Kubernetes, serverless, Terraform/Pulumi, GraphQL ni nuevos servicios sin aprobación explícita. El propietario ahora autoriza considerar su instancia AWS y Cloudflare; el despliegue concreto sigue condicionado a inventario, capacidad, costos y revisión de seguridad.
- El asistente de reservas multicanal se especifica en [`../product/BOOKING_ASSISTANT.md`](../product/BOOKING_ASSISTANT.md). No desarrollar hasta revisar contratos, pagos, reservas, cancelaciones y facturación efectivos.

## ADR

Decisiones aceptadas se registran en `docs/adr/NNNN-titulo-corto.md` con contexto, alternativas, decisión, consecuencias, owner y estado. No crear un ADR de decisión técnica sin responsables revisores.
