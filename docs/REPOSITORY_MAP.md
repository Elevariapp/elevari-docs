# Mapa de repositorios Elevari

**Estado:** propuesta de organización, no constituye autorización para crear repositorios ni migrar código.

## Inventario observado

| Dominio | Repositorio | Estado observado | Responsabilidad propuesta |
|---|---|---|---|
| Web | `Elevariapp/elevari-web` | Existe; contiene README inicial | Experiencia Next.js de Elevari; consume la API oficial |
| Documentación | `Elevariapp/elevari-docs` | Existe; contiene documentación de preparación | Fuente propuesta para contratos, decisiones, QA y release |
| Plataforma/API | `Elevariapp/elevari-platform` | No encontrado / no accesible en la revisión | API Fastify, Prisma, migraciones, pagos, booking, factura y contratos |
| Móvil | `Elevariapp/elevari-mobile` | No encontrado / no accesible en la revisión | Cliente iOS/Android; Expo/React Native requiere confirmación |
| Operaciones | `Elevariapp/elevari-ops` | No encontrado / no accesible en la revisión | Runbooks y operación; no agregar proveedor/IaC sin aprobación |
| Código actual de referencia | `Elevariapp/Elevari-repotransfer` | Existe | Código actual y documentos de arquitectura/infraestructura que deben auditarse antes de separar componentes |

“ No encontrado / no accesible” no permite concluir que el nombre esté libre. Confirmar con la cuenta/organización antes de crear.

## Límites

- API es fuente de verdad para reglas de negocio, permisos, reservas, pagos y estado de facturación, sujeto a verificación del código vigente.
- Web, móvil y bot no duplican lógica de negocio, auth del servidor ni integración de pagos.
- El asistente de reservas puede exponerse en Web, app y WhatsApp Business como canales; referencia funcional: [`docs/product/BOOKING_ASSISTANT.md`](product/BOOKING_ASSISTANT.md).
- La API valida disponibilidad, precio, reservas, cancelaciones, pagos Wompi y facturas. Wompi se declara activa por el responsable pero requiere verificación técnica contra la documentación actual del repo.
- El asistente no recibe datos de tarjetas ni secretos. La compra se deriva al checkout seguro existente; las claves se configuran solo en el secret management aprobado, nunca por chat o repositorio.
- Plataforma posee schema/migraciones; clientes no acceden directamente a DB.
- Docs mantiene decisiones y especificaciones. Un ADR aprobado gobierna una decisión hasta que sea sustituido por otro ADR.
- Ops documenta y opera arquitectura aprobada; no introduce cloud, Kubernetes, serverless o Terraform/Pulumi sin aprobación.

## Reglas para separar repos

1. Inventariar código, dependencias, CI, variables, imágenes y responsables.
2. Definir contrato API y estrategia de versiones antes de desacoplar clientes/bot.
3. Elegir cuál repositorio contiene la documentación canónica; los demás enlazan a ella.
4. Acordar permisos, visibilidad, CODEOWNERS, protección de ramas y CI por repositorio.
5. Migrar en cambios revisables, manteniendo camino de vuelta y comprobando builds.
