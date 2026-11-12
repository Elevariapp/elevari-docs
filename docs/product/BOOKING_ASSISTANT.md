# Asistente Elevari de reservas, pagos y atención

**Estado:** alcance de producto/arquitectura propuesto para discovery; no está implementado ni autorizado para producción.  
**Canales previstos:** Web, aplicación móvil y WhatsApp Business.  
**Fuente de verdad:** API/backend de Elevari; los canales no ejecutan reglas de negocio propias.

## Objetivo del servicio

Acompañar al cliente desde la búsqueda/consulta de una propiedad hasta una reserva confirmada, compra/pago, atención de cancelaciones permitidas y acceso a la factura correspondiente. El mismo servicio conversacional podrá presentarse en Web, app y WhatsApp Business, sujeto a las capacidades aprobadas de cada canal.

El asistente ayuda y coordina operaciones, pero no sustituye la política de reservas/cancelaciones, la validación del proveedor de pago, el sistema de facturación ni a soporte humano.

## Estado conocido y verificación requerida

El responsable del producto indica que **Wompi ya está activa** y que más adelante facilitará las credenciales necesarias. La documentación revisada de `Elevari-repotransfer` describe Bold/Stripe y Alegra, no confirma por sí sola el estado actual de Wompi. Antes de integrar el bot, verificar en el código/configuración autorizada:

- si Wompi está integrada en backend o en un flujo externo;
- ambientes sandbox/producción y webhooks implementados;
- forma de validar notificaciones, estado final, reintentos e idempotencia;
- asociación entre pago, booking e invoice;
- coexistencia o sustitución frente a Bold/Stripe según una decisión aprobada.

No incluir claves en este documento, chat, issues, pull requests, repositorios ni aplicaciones cliente. Cuando toque configurar credenciales, usar solo el mecanismo de secretos aprobado, con permisos mínimos y nunca comunicar aquí su valor. Si una clave se expone accidentalmente, revocarla/rotarla inmediatamente con el proveedor.

## Capacidades previstas

### Descubrimiento y pre-reserva

- Responder preguntas con información aprobada de propiedades, servicios, ubicación, disponibilidad y políticas.
- Solicitar fechas y ocupación; validar entradas y pedir aclaración si falta información.
- Consultar disponibilidad/precio mediante endpoints autorizados del backend.
- Presentar precio total, impuestos/cargos si aplican, política de cancelación y condiciones antes de pedir confirmación.

### Crear reserva y pagar

- Crear la reserva solo después de confirmación explícita del cliente y mediante API.
- Evitar duplicados mediante idempotencia del backend; el bot no reintenta a ciegas una operación con resultado desconocido.
- Iniciar/compartir el flujo seguro de checkout de Wompi únicamente según la integración oficial ya verificada y aprobada.
- Nunca solicitar, recibir, almacenar o reenviar número de tarjeta, CVV, contraseña bancaria, OTP, token de pago ni claves de proveedor en texto conversacional.
- No declarar una compra pagada por una respuesta de canal, redirect, captura de pantalla o afirmación del usuario. El backend actualiza el estado solo tras validar una notificación/consulta confiable del proveedor.
- Comunicar estados inequívocos: reserva pendiente, pago pendiente, confirmada, pago fallido o requiere atención; no llamar “confirmada” a una reserva no confirmada por API.

### Cancelación, cambios y reembolsos

- Consultar la reserva y comprobar identidad/permisos antes de revelar o cambiar datos.
- Explicar la política aplicable y sus consecuencias antes de iniciar la operación.
- Pedir confirmación explícita del cliente para la cancelación.
- Ejecutar solo operaciones habilitadas por API y reglas de negocio vigentes; usar idempotencia y registrar auditoría.
- Distinguir cancelación de reembolso. No prometer monto/tiempo de reembolso salvo que la API/proveedor confirme la información.
- Escalar a persona cambios fuera de política, disputas, reservas de terceros, pagos ambiguos y cualquier excepción.

### Factura y seguimiento

- Obtener estado y enlace/documento de factura desde la integración de facturación autorizada; comprobar permisos antes de exponerla.
- No generar factura solo por iniciar checkout ni por recibir texto del cliente. El evento de pago confirmado y las reglas fiscales/configuración de Elevari gobiernan emisión.
- Registrar entrega, reintento o fallo de factura según la API y dar opción de atención humana.

## Arquitectura funcional propuesta

```text
Web chat ─────┐
App chat ──────┼──> Adaptador de canal ──> Servicio/asistente ──> API oficial Elevari
WhatsApp ─────┘                                  │                    ├─ propiedades/disponibilidad
                                                 │                    ├─ reservas/cancelaciones
                                                 │                    ├─ estado de pago Wompi (por validar)
                                                 │                    └─ factura (integración vigente por validar)
                                                 └──> soporte humano / handoff
```

La diagramación es conceptual. No prescribe un nuevo microservicio, proveedor LLM, framework ni infraestructura. Se debe inspeccionar la API y decidir el diseño técnico mediante ADR antes de implementar. Los adaptadores de canal normalizan identidad, consentimiento, mensajes y callbacks; la autorización y transiciones críticas siguen en backend.

## Seguridad, privacidad y confianza

- Autenticar y autorizar las operaciones sensibles en el backend; no confiar en el contexto conversacional como prueba de identidad.
- Recopilar solo los datos mínimos para búsqueda/reserva/soporte y explicar su uso en cada canal.
- No enviar datos de pago ni secretos al modelo, logs, analítica, herramientas de terceros o canales de chat.
- Proteger sesiones y asociar cada consulta/acción a usuario, reserva y canal autorizado; prevenir acceso cruzado a reservas.
- Validar autenticidad de webhooks conforme al contrato/documentación del proveedor y protegerse ante repetición/doble entrega.
- Mantener auditoría de acciones relevantes: canal, actor, intención, operación API, resultado, consentimiento/confirmación y marca temporal; minimizar/redactar datos personales.
- Definir retención, eliminación, consentimiento de WhatsApp, privacidad, opt-out y tratamiento de conversaciones conforme a requisitos aplicables.
- Limitar acciones del asistente a herramientas/endpoints de lista permitida, entradas validadas y permisos específicos. No ejecutar código SQL ni operaciones arbitrarias.
- Proporcionar handoff humano, declarar incertidumbre y no inventar disponibilidad, precio, factura ni política.

## Métricas y calidad

Definir línea base y metas antes del piloto. Medir, sin recopilar información innecesaria:

- resolución correcta de consultas y tasa de escalamiento;
- conversión a cotización/reserva y abandono por etapa;
- errores de disponibilidad, booking duplicado, estado de pago y factura;
- acciones canceladas por el usuario antes de confirmar;
- latencia, disponibilidad, reintentos y fallas de proveedor/canal;
- satisfacción y quejas, con mecanismo de consentimiento y revisión.

No hay umbrales numéricos aprobados todavía. El framework general GO/NO-GO es obligatorio; un piloto de chatbot no hereda automáticamente el GO de Web/app.

## Fases de entrega del asistente

1. **BOT-0 Discovery:** confirmar Wompi/booking/facturación en código y políticas; mapear intents, datos, límites, canal y soporte humano.
2. **BOT-1 Diseño seguro:** contrato de herramientas/endpoints, autorización, idempotencia, auditoría, privacidad, consentimiento y pruebas de abuso/errores; ADR aprobado.
3. **BOT-2 Web/app en staging:** FAQ e información pública aprobada; después, lectura autenticada de booking y disponibilidad. Sin operar pagos reales.
4. **BOT-3 Reserva/cancelación en staging:** crear, consultar y cancelar usando API con confirmación, pruebas concurrentes, idempotencia y casos de autorización.
5. **BOT-4 Wompi/factura en sandbox:** conectar el checkout integrado existente tras revisión; validar estados, webhook/reintentos, asociación booking-pago-factura y fallos. Sin exponer credenciales.
6. **BOT-5 WhatsApp Business:** solo después de resolver proveedor/cuenta, opt-in, plantillas/reglas, webhooks, privacidad, handoff y QA en entorno controlado.
7. **BOT-6 Piloto limitado:** cohortes y límites autorizados, soporte humano y monitoreo; GO independiente por canal y capacidad.
8. **BOT-7 Producción gradual:** gates de seguridad, UX, operación, facturación, rollback y decisión formal GO/NO-GO.

Las fases pueden cambiar mediante decisión documentada. No se conectan canales ni se hacen cambios a pagos/cancelaciones solo por proporcionar claves.

## Criterios de aceptación mínimos antes de producción

- Respuestas de precio/disponibilidad/política provienen de fuentes autorizadas y pueden mostrar su vigencia.
- Ninguna reserva se confirma sin la transición confirmada por API.
- Pruebas de concurrencia e idempotencia impiden reservas duplicadas y acciones duplicadas.
- Los pagos se declaran exitosos solo después de validación backend/proveedor; fallos y timeouts tienen estados recuperables.
- La cancelación exige autorización, política aplicable y confirmación explícita; reembolso se modela por separado.
- La factura se consulta/emite con el estado confirmado y permisos correctos.
- Se prueban fallos de canal, webhook repetido/alterado, sesión expirada, usuario no autorizado, reserva de terceros y handoff humano.
- Auditoría/logging no expone secretos, datos de tarjeta ni información personal innecesaria.
- Privacidad/consentimiento, soporte, retención y opt-out están aprobados para cada canal.
- Hay evidencia de usabilidad, accesibilidad, pruebas, monitoreo y rollback en cada plataforma habilitada.

## Fuera de alcance hasta aprobación

- Elegir LLM/proveedor, RAG/framework o alojar un modelo.
- Dar al bot autonomía para aprobar reembolsos, cambiar precios, saltarse políticas, emitir cargos o modificar reservas fuera de endpoints aprobados.
- Copiar claves Wompi a commits, variables `NEXT_PUBLIC_*`, documentación o herramientas de conversación.
- Habilitar WhatsApp Business o producción sin validar consentimiento, configuración y requisitos vigentes.
- Sustituir/duplicar la integración de pagos existente sin inspección del código y ADR.

## Referencias internas

- [Backlog inicial](../DEVELOPMENT_BACKLOG.md)
- [Mapa de repositorios](../REPOSITORY_MAP.md)
- [Arquitectura y decisiones pendientes](../architecture/README.md)
- [QA](../qa/README.md)
- [Release GO/NO-GO](../release/GO_NO_GO.md)

## Estado de validación

**Documentado:** alcance funcional inicial y controles propuestos.  
**No verificado:** integración activa de Wompi, webhooks, reserva, cancelación, facturación, WhatsApp y funcionamiento multicanal.  
**No realizado:** desarrollo, pruebas, configuración de credenciales, transacciones sandbox/producción o despliegue.  
**Siguiente paso:** BOT-0 y tareas `PAY-01`, `PLAT-03`, `BOT-01` en el backlog.
