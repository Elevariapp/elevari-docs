# Backlog inicial de preparación de Elevari

**Estado general:** ninguna tarea se considera terminada por estar listada. Las tareas requieren asignación, alcance y evidencia. Prioridad P0 bloquea decisiones/dependencias; P1 es necesaria antes del release afectado; P2 puede planificarse después del MVP.

## P0 — Antes del primer incremento de desarrollo coordinado

- [ ] **INV-01 Inventario del código fuente:** mapear raíz, Web/API, scripts, dependencias, workflows, migraciones, tests y deploys del repo `Elevari-repotransfer`.
- [ ] **INV-02 Estado de la plataforma:** ejecutar pruebas existentes y registrar baseline de build, lint, test y despliegue local/staging.
- [ ] **ADR-01 Arquitectura Admin:** resolver documentación Supabase vs. API Fastify/Prisma con evidencia de implementación.
- [ ] **ORG-01 Repositorios:** aprobar nombres, visibilidad, owners, rama base y responsables de platform/mobile/ops; comprobar que no existen ya.
- [ ] **ORG-02 Fuente canónica:** aprobar `elevari-docs` como fuente oficial y definir cómo se adoptan documentos vigentes del repo actual.
- [ ] **API-01 Contrato:** inventariar rutas existentes y publicar contrato de autenticación, propiedades, disponibilidad, reservas y pagos.
- [ ] **ENV-01 Staging:** documentar acceso, variables requeridas sin valores sensibles, dependencias, deploy y smoke tests.
- [ ] **SEC-01 Secretos y exposición:** auditar configuración/secretos y puertos con acceso autorizado; registrar hallazgos sin copiar valores secretos.
- [ ] **RES-01 Reserva concurrente:** inspeccionar modelo e implementación; diseñar/probar casos concurrentes/idempotentes sobre entorno no productivo. No asumir bug antes de verificar.
- [ ] **PAY-01 Wompi:** el responsable indica que Wompi ya está activa. Verificar en código y entorno autorizado la integración, webhooks, estados, reintentos, idempotencia y modo actual; no pedir ni almacenar llaves en issues, PRs o chat.
- [ ] **UX-01 Alcance MVP:** aprobar audiencia, flujo huésped prioritario, requisitos y criterios de aceptación.
- [ ] **UX-02 Sistema de calidad:** aprobar roles, gates GO/NO-GO, proceso de UX/accesibilidad y captura de evidencia.

**Gate P0:** arquitectura aplicable, repositorios, owners, alcance MVP, contrato API inicial y staging utilizables. Sin eso, solo se permite trabajo documental/investigación que no dependa de esas decisiones.

## P1 — Plataforma y experiencia del MVP

- [ ] **PLAT-01 Integridad de booking:** pruebas de colisión, cancelación/re-reserva, reintentos y doble envío basadas en reglas reales.
- [ ] **PLAT-02 Pagos Wompi:** validar el flujo vigente de pago de extremo a extremo en sandbox/staging; verificar firma/autenticidad de notificaciones según documentación oficial aplicable, idempotencia, conciliación y estados finales. No ejecutar cargos reales durante pruebas.
- [ ] **PLAT-03 Facturación:** verificar el flujo de factura existente y su relación con el pago confirmado; no emitir una factura desde una mera captura de pantalla o mensaje del bot.
- [ ] **OPS-01 Backup/restore:** acordar retención, RPO/RTO y comprobar restauración aislada.
- [ ] **OPS-02 Salud/observabilidad:** comprobar health de Postgres, Redis, MinIO, API, Web y proxy; definir alertas y logs seguros.
- [ ] **UX-03 Investigación:** validar discovery, detalle, precio y checkout con usuarios representativos.
- [ ] **UX-04 Design system:** documentar tokens/componentes/estados y reglas responsive/nativas.
- [ ] **WEB-01 Web MVP:** implementar mejoras priorizadas en Web existente, no reconstruir sin auditoría.
- [ ] **MOB-01 Decisión técnica:** confirmar Expo/React Native, navegación, almacén seguro y publicación antes de crear repo.
- [ ] **BOT-01 Contrato del asistente:** definir intents, límites de acciones, consentimiento, escalamiento humano, registro de auditoría y política de retención antes de conectar canales.
- [ ] **BOT-02 Canal Web/App:** diseñar asistente integrado en Web y móvil como interfaces del mismo servicio/contrato; el backend conserva las validaciones y no se duplica lógica por canal.
- [ ] **BOT-03 Reserva y cancelación:** probar consulta de disponibilidad, cotización, creación y consulta de reserva; cancelación solo según política vigente y tras confirmación explícita del usuario. Reembolsos requieren estado autorizado del proveedor/API.
- [ ] **BOT-04 Pago y factura:** el asistente inicia o enlaza checkout Wompi en flujo seguro, consulta estado confirmado por backend y comunica resultado; luego consulta/emite o enlaza factura solo mediante integración autorizada existente. Nunca solicita datos de tarjeta o credenciales por chat.
- [ ] **BOT-05 WhatsApp Business:** validar integración oficial, opt-in, plantillas, ventana/reglas de mensajería aplicables, protección de datos, webhook, handoff a persona y monitoreo antes de habilitar producción.
- [ ] **QA-01 Pruebas:** unitarias, integración, E2E, contrato, accesibilidad y visual para journeys de release, incluidos errores y reintentos del asistente.

## P2 — Post-MVP, según evidencia y aprobación

- [ ] **MOB-02 Aplicación huésped:** explorar, detalle, disponibilidad, reserva/checkout con API compartida.
- [ ] **CS-01 Soporte humano:** definir canales, horarios, escalamiento y responsabilidades de atención.
- [ ] **CS-02 Asistente multicanal:** desplegar gradualmente Web/app/WhatsApp tras cerrar BOT-01–05 y completar gates de seguridad, UX y operación.
- [ ] **MKT-01 Marketing:** auditar capacidades ya marcadas como completadas en Roadmap; corregir gaps comprobados.
- [ ] **OPS-03 Go-live:** completar framework GO/NO-GO y decisión firmada por release.

## Registro de tareas

Para cada item registrar issue/PR, owner, repo, prioridad, estado, bloqueos, aceptación, evidencia de prueba y rollback. Ningún check se marca completo sin esa evidencia. No registrar claves ni datos de huéspedes en el backlog.
