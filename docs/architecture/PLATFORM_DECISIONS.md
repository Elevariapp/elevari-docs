# Recomendación de plataforma Elevari — AWS, Cloudflare y Wompi

> **Estado:** recomendación técnica basada en las opciones autorizadas por el propietario; pendiente de ratificación operativa, inventario de la instancia y verificación de integraciones.  
> **No implica:** que el servidor esté configurado, endurecido, dimensionado o listo para producción.

## Decisión recomendada

1. **AWS:** usar la instancia AWS existente como destino primario de la plataforma, conservando inicialmente la arquitectura Docker Compose documentada (Web, Fastify API, PostgreSQL, Redis, MinIO y Nginx), siempre que el inventario confirme capacidad, almacenamiento, respaldos y aislamiento adecuados.
2. **Cloudflare:** usarlo delante del origen como capa de DNS/proxy y controles de borde aprobados. No considerarlo sustituto del firewall de AWS, TLS correcto hasta el origen, autenticación/autorización, rate limiting de API ni protección de secretos.
3. **Wompi:** tratarlo como proveedor de pago elegido para Colombia, según indicación del propietario de que ya está activa. Antes de conectarlo al asistente o mover tráfico, inspeccionar y probar la integración real y resolver cualquier diferencia con la documentación existente que menciona Bold/Stripe.
4. **Hugging Face Free Tier:** usar únicamente para prototipo y evaluación de modelos/demostraciones con información pública o sintética. No enviar datos de huéspedes, reservas, pagos ni PII. No depender de una cuota gratuita como garantía de disponibilidad de producción.
5. **Fly.io:** no añadirlo en este momento. Evitar repartir servicios y datos entre proveedores mientras la instancia AWS existente pueda satisfacer la operación. Reconsiderar solo con una necesidad medida, revisión de costos/seguridad y decisión documentada.
6. **Asistente en producción:** preferir primero un flujo determinista/orquestado por API para consultar, cotizar y actuar sobre booking/pago/factura. Si luego se incorpora inferencia de modelo, el modelo interpreta lenguaje y redacta respuestas; nunca es autoridad de precio, disponibilidad, identidad, pago, cancelación o emisión de factura.

## Diagrama lógico recomendado

```text
Web / App / WhatsApp Business
             │
             ▼
      API oficial Elevari (Fastify)
       ├── propiedades y disponibilidad
       ├── booking y cancelación autorizada
       ├── estado de pago Wompi (validado por backend)
       └── factura (integración vigente y autorizada)
             │
             ▼
 AWS existente: Docker Compose detrás de Cloudflare
```

WhatsApp requiere un adaptador/canal oficial aprobado y Web/App pueden tener presentación propia; la lógica transaccional vive en la API. El diagrama no prescribe un microservicio, proveedor LLM ni nuevos componentes desplegados.

## Condiciones antes de usar la instancia AWS

Completar y registrar, sin incluir secretos en documentación:

- Región, tipo de instancia, CPU/RAM, disco disponible, sistema operativo, uso actual y límites/costos.
- Servicios y contenedores actuales, dominios, puertos, reglas de security group/firewall y acceso administrativo.
- Estado de TLS entre Cloudflare y origen, DNS/proxy, origen directo y certificado/renovación según configuración aprobada.
- Persistencia, cifrado/acceso de volúmenes y copias de seguridad fuera del disco del host cuando corresponda.
- Capacidad de restauración verificada, monitoreo, alertas, logs y runbooks.
- Configuración de secretos de producción, rotación y permisos mínimos.
- Consumo adicional que introduciría el asistente; no despliegue si reduce la reserva de recursos necesaria para DB/API/Web.

Si la instancia no tiene capacidad o controles suficientes, detener y proponer alternativas AWS para aprobación; no migrar automáticamente a Fly.io ni a otro proveedor.

## Límites de seguridad para Cloudflare y el origen

- Cloudflare protege el borde, pero el origen AWS también necesita firewall/security groups con exposición mínima y administración restringida.
- Evitar que servicios internos como PostgreSQL, Redis, MinIO y puertos de depuración queden públicos; validar el estado efectivo de la instancia y Compose.
- Definir TLS Cloudflare–origen y comprobarlo desde el entorno real; no asumir seguridad del origen por tener proxy Cloudflare activo.
- Configurar y probar rate limits/controles de abuso en las rutas públicas de API según el tráfico y los servicios aprobados.
- Preservar IP/headers de proxy solo mediante configuración confiable; no autorizar usuarios basándose en headers arbitrarios.
- No incluir tokens Wompi, credenciales AWS, secretos JWT o llaves WhatsApp en cliente, logs, issues, PRs o commits.

Estas son verificaciones de configuración defensiva, no evidencia de que el entorno actual tenga alguna exposición concreta.

## Wompi y credenciales

- El propietario indica que Wompi ya está activa; la documentación revisada anteriormente no la confirmaba y mencionaba Bold/Stripe. Inspeccionar código/env templates/configuración sin revelar valores para conocer el estado real.
- Mantener claves separadas por sandbox/producción y limitar permisos. Al momento de configurar, usar el mecanismo de secretos AWS aprobado por el equipo (por ejemplo, un gestor de secretos/parameter store, previa decisión de costo y operación); nunca recibir claves por este chat.
- Confirmar el flujo de checkout, autenticidad de eventos/webhooks según documentación de Wompi, consulta de estado, idempotencia, reintentos, conciliación y asociación inequívoca con reserva.
- El backend es la única fuente de estado pagado. Un redirect, mensaje de WhatsApp, texto del usuario o captura no confirma un cobro.
- Probar solo en sandbox/staging antes de habilitar operaciones reales.
- No sustituir Bold/Stripe ni declarar coexistencia hasta que el dueño apruebe esa decisión después de revisar consumidores y configuración.

## Hugging Face: uso permitido en esta etapa

**PoC/evaluación:** prompts, herramientas simuladas y documentos públicos/sintéticos; medir latencia, calidad, límites, privacidad y costos.

**No permitido para PoC con free tier:** datos reales de huésped, PII, tokens, solicitudes con reservas reales, detalles de pago, facturas personales, claves o registros productivos.

**Para producción:** escoger una opción de inferencia después de modelar requisitos de privacidad, tratamiento/retención, disponibilidad, latencia, presupuesto, región, contrato y plan de fallback. Puede evaluarse alojamiento controlado en AWS o un servicio contratado, pero no se presume ninguno aprobado. El componente de lenguaje se mantiene desacoplado de la lógica transaccional.

## Costos y decisión de escala

Antes de producción, comparar costos de instancia/almacenamiento/backups/egress, Cloudflare según plan, Wompi según contrato, WhatsApp Business y modelo/inferencia. La presente recomendación no estima importes sin la región, carga, volúmenes y condiciones contractuales. Registrar responsable de presupuesto y límites/alertas.

## Checklist de decisión técnica

- [ ] Inventario y acceso autorizado a la instancia AWS completados.
- [ ] Capacidad/costo para cargas existentes y nuevas validado.
- [ ] Firewall, puertos, TLS origen y Cloudflare comprobados.
- [ ] Backup y restauración probados; datos persistentes protegidos.
- [ ] Integración Wompi actual inspeccionada y documentada sin secretos.
- [ ] Sandbox, webhooks y idempotencia probados de forma no productiva.
- [ ] WhatsApp Business y proveedor/modelo de producción aún se deciden y aprueban por separado.
- [ ] Free tier de Hugging Face restringido a PoC con datos sintéticos/públicos.
- [ ] No se introduce Fly.io sin una decisión de arquitectura nueva.
- [ ] Framework GO/NO-GO aplicado y responsable autorizado registra resultado.

## Estado de validación

**Decisión recomendada:** AWS existente + Cloudflare como borde + Wompi para pagos; Hugging Face gratis solo para prototipo; no añadir Fly.io por ahora.  
**No verificado:** instancia AWS, configuración Cloudflare, Wompi en código, flujos webhook, capacidad, costos, dominios, secretos y operación real.  
**Pruebas realizadas:** ninguna de infraestructura/pagos.  
**Siguiente paso:** completar `INV-01`, `SEC-01`, `ENV-01`, `PAY-01` y `BOT-0` del backlog; mantener producción en NO-GO hasta obtener evidencia.
