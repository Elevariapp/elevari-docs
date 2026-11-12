# Arquitectura — decisiones y referencias

## Arquitectura documentada actualmente

El documento `INFRASTRUCTURE.md` del repo [`Elevariapp/Elevari-repotransfer`](https://github.com/Elevariapp/Elevari-repotransfer/blob/b-main/INFRASTRUCTURE.md) describe Next.js/React/TypeScript, API REST Fastify, Prisma, PostgreSQL, Redis, MinIO, Docker Compose y Nginx. Los nuevos flujos de negocio deben pasar por la API oficial.

## Bloqueos de decisión

- `README_ADMIN.md` prescribe Supabase directo y contradice la infraestructura/roadmap que describen el retiro del legacy para los nuevos flujos. No desarrollar Admin hasta confirmar el código vigente y una decisión aprobada.
- El roadmap propone Expo/React Native; confirmar stack y repositorio móvil antes de crear código.
- Separación de repositorios y ubicación final de documentos canónicos requieren aprobación.
- No introducir proveedor cloud, Kubernetes, serverless, Terraform/Pulumi, GraphQL ni nuevos servicios sin aprobación explícita.
- El repositorio de referencia documenta Bold/Stripe y Alegra, mientras el responsable indica que Wompi ya está activa. Confirmar con código/configuración y entorno autorizado qué integración/proveedor está realmente activo y si existe convivencia; no inferirlo ni sustituirlo sin decisión.

## Asistente de reservas multicanal — principio de arquitectura propuesto

La especificación inicial vive en [`docs/product/BOOKING_ASSISTANT.md`](../product/BOOKING_ASSISTANT.md).

- Web, app y WhatsApp Business son canales de conversación; no deben contener lógica independiente de reservas/pagos.
- El asistente invoca capacidades autorizadas del backend/API. La API confirma disponibilidad, cotiza, crea/cancela reservas, refleja el estado de pago y accede al estado de factura.
- Wompi es el proveedor de pago que el responsable declara activo, pendiente de verificación documental/técnica. Las llaves no se comparten por chat, issue, commit ni cliente; se cargan en el mecanismo de secretos aprobado por personal autorizado.
- El bot nunca debe pedir tarjeta, CVV, contraseña, token, OTP ni llaves. Debe conducir al checkout seguro que apruebe la integración vigente.
- Una respuesta de WhatsApp, retorno del navegador o captura del usuario no confirma un pago; solo el backend, tras validar la señal autenticada del proveedor, establece estado pagado.
- Cancelaciones/reembolsos siguen las políticas de Elevari y el estado de reserva/pago. El bot explica, solicita confirmación y ejecuta solo acciones permitidas por el backend. Reembolso no se asume equivalente a cancelación.
- Factura se comunica/consulta únicamente con estado confirmado por la integración de facturación aprobada. Evitar exponer factura o datos personales sin validar identidad/autorización.
- Debe existir transferencia a soporte humano para baja confianza, excepciones, disputas, cambios sensibles o solicitud del cliente.

## ADR

Decisiones aceptadas se registran en `docs/adr/NNNN-titulo-corto.md` con contexto, alternativas, decisión, consecuencias, owner y estado. No crear un ADR de decisión técnica sin responsables revisores.
