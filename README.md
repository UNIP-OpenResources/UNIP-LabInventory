# UNIP-LabInventory

LabInventory es un sistema de trazabilidad y control de inventario para reactivos químicos peligrosos en laboratorios ambientales. El proyecto busca registrar existencias, movimientos, ubicación, caducidad, riesgos asociados, fichas de seguridad y responsabilidades operativas, con énfasis en control institucional, prevención de incidentes y cumplimiento documental.

Este repositorio corresponde a un proyecto académico en desarrollo progresivo. Las decisiones técnicas descritas aquí establecen una línea base profesional para orientar la implementación durante las sesiones del módulo.

## 1. Descripción del Proyecto

El objetivo de LabInventory es proveer una plataforma para administrar reactivos químicos desde su ingreso al laboratorio hasta su consumo, vencimiento, traslado o disposición final. El sistema deberá facilitar el seguimiento de sustancias peligrosas, alertar sobre caducidades, controlar cantidades disponibles y mantener evidencia auditable de cada operación.

Alcance funcional inicial:

- Registro de reactivos químicos, lotes, proveedores y fichas de seguridad.
- Control de inventario por laboratorio, gabinete, estante o ubicación específica.
- Seguimiento de cantidades disponibles, unidades de medida y umbrales mínimos.
- Alertas por caducidad, bajo stock, incompatibilidad o condiciones de almacenamiento.
- Trazabilidad de ingresos, consumos, traslados, ajustes y bajas.
- Clasificación de peligrosidad y compatibilidad de almacenamiento.
- Gestión de usuarios, roles y permisos operativos.
- Reportes para auditoría, control interno y revisión de seguridad.

Usuarios previstos:

- Coordinadores de laboratorio.
- Técnicos de laboratorio.
- Responsables de seguridad y salud en el trabajo.
- Docentes e investigadores autorizados.
- Auditores o personal administrativo con permisos de consulta.

## 2. Stack Tecnológico

Stack propuesto para el desarrollo:

- Frontend: React, TypeScript, Vite y CSS modular o Tailwind CSS según la evolución del diseño.
- Backend/API: Node.js con NestJS o Express, priorizando una arquitectura modular.
- Base de datos principal: PostgreSQL.
- ORM y migraciones: Prisma o Drizzle ORM.
- Autenticación: JWT con refresh tokens o proveedor institucional compatible.
- Validación: Zod, class-validator o esquema equivalente.
- Generación de reportes: PDF/CSV mediante servicios controlados del backend.
- Pruebas: Vitest/Jest para unitarias, Supertest para API y Playwright para flujos críticos.
- Calidad de código: ESLint, Prettier, TypeScript strict y convenciones de commits.
- Observabilidad: logs estructurados y trazabilidad por solicitud.

La selección definitiva de librerías deberá mantenerse documentada en este README a medida que el repositorio incorpore código real.

## 3. Estructura del Repositorio

Estructura base recomendada:

```text
UNIP-LabInventory/
├── README.md
├── docs/
│   ├── architecture.md
│   ├── api.md
│   ├── data-model.md
│   └── safety-rules.md
├── apps/
│   ├── web/
│   └── api/
├── packages/
│   ├── shared/
│   └── config/
├── prisma/
│   ├── schema.prisma
│   └── migrations/
├── tests/
│   ├── unit/
│   ├── integration/
│   └── e2e/
└── scripts/
```

Convención:

- `apps/web`: interfaz de usuario.
- `apps/api`: servicios HTTP, autenticación, inventario, alertas y reportes.
- `packages/shared`: tipos, validadores, contratos y utilidades compartidas.
- `docs`: arquitectura, flujos, reglas de seguridad, API y modelo de datos.
- `prisma` o carpeta equivalente: esquema de base de datos y migraciones.
- `tests`: pruebas automatizadas agrupadas por nivel.

## 4. Comandos de Desarrollo y Verificación

Comandos esperados una vez inicializado el stack:

```bash
npm install
npm run dev
npm run build
npm run lint
npm run format
npm run typecheck
npm run test
npm run test:e2e
npm run db:migrate
npm run db:seed
```

Política de verificación:

- Todo cambio funcional debe ejecutar al menos lint, typecheck y pruebas relacionadas.
- Los cambios de base de datos deben incluir migración reproducible.
- Los cambios de API deben validar contrato, errores y casos límite.
- Los cambios de interfaz deben revisarse en escritorio y móvil cuando afecten layout.
- Los flujos de inventario y baja de reactivos deben probar trazabilidad y permisos.

## 5. Estándares y Convenciones de Frontend

Principios:

- Interfaz sobria, precisa y orientada a operación de laboratorio.
- Componentes reutilizables, pequeños y con responsabilidades explícitas.
- Accesibilidad como requisito: etiquetas, navegación por teclado, contraste suficiente y estados de foco visibles.
- Estados visibles para carga, vacío, error, alerta crítica, vencimiento próximo y permisos insuficientes.
- Separación entre componentes visuales, lógica de datos y reglas de dominio.
- Presentación clara de riesgo químico, estado de inventario y caducidad.

Convenciones:

- Usar TypeScript estricto.
- Evitar lógica de negocio compleja dentro de componentes de presentación.
- Centralizar contratos de API y validaciones compartidas.
- Nombrar componentes con PascalCase y hooks con prefijo `use`.
- Mantener formularios con validación explícita y mensajes accionables.
- Usar confirmaciones para operaciones críticas como bajas, ajustes y eliminación lógica.
- Evitar componentes visuales que resten claridad a información de seguridad.

## 6. Estándares y Convenciones de API y Backend

Principios:

- API modular orientada al dominio de inventario químico.
- Endpoints versionados cuando el contrato pueda cambiar.
- Validación de entrada en todos los límites externos.
- Respuestas consistentes y errores normalizados.
- Separación entre controladores, casos de uso, repositorios y servicios de infraestructura.
- Auditoría obligatoria en movimientos de inventario y operaciones sobre sustancias peligrosas.

Convenciones sugeridas:

- Prefijo de API: `/api/v1`.
- Formato JSON para solicitudes y respuestas.
- Paginación obligatoria en listados.
- Filtros documentados y validados.
- Identificadores UUID para entidades críticas.
- Eliminación lógica para entidades con valor histórico.
- Logs estructurados sin información sensible.
- Operaciones críticas protegidas por autorización basada en roles.

Ejemplos de recursos de API:

```text
GET    /api/v1/reagents
POST   /api/v1/reagents
GET    /api/v1/reagents/:id
PATCH  /api/v1/reagents/:id
GET    /api/v1/lots
POST   /api/v1/inventory-movements
GET    /api/v1/alerts/expirations
GET    /api/v1/reports/inventory
```

## 7. Modelo de Datos y Migraciones

Entidades iniciales propuestas:

- `User`: usuario autenticado del sistema.
- `Role`: rol asignado al usuario.
- `Laboratory`: laboratorio o unidad operativa.
- `StorageLocation`: gabinete, estante, nevera, bodega o ubicación física.
- `Reagent`: sustancia o producto químico registrado.
- `ReagentLot`: lote específico con cantidad, proveedor, fecha de ingreso y caducidad.
- `SafetyDataSheet`: ficha de seguridad asociada al reactivo.
- `HazardClass`: clasificación de peligrosidad.
- `InventoryMovement`: ingreso, consumo, traslado, ajuste, baja o disposición final.
- `AlertRule`: regla para alertas de caducidad, stock o incompatibilidad.
- `AuditLog`: registro de acciones relevantes.

Reglas de migración:

- Toda modificación del esquema debe crear una migración versionada.
- Las migraciones deben ser reversibles cuando la herramienta lo permita.
- No se deben modificar migraciones ya aplicadas en entornos compartidos.
- Los datos semilla deben ser mínimos, trazables y aptos para desarrollo.
- Los cambios de modelo deben actualizar documentación, validaciones y contratos de API.
- Las unidades de medida y estados deben manejarse con catálogos controlados o enums documentados.

## 8. Seguridad, Auth y Secrets

Lineamientos:

- Nunca versionar secretos, tokens, certificados privados ni archivos `.env` reales.
- Usar `.env.example` para documentar variables requeridas.
- Aplicar autenticación obligatoria en endpoints privados.
- Implementar autorización por roles para operaciones administrativas y movimientos críticos.
- Registrar responsable, fecha y motivo en ajustes, bajas y traslados.
- Proteger documentos adjuntos como fichas de seguridad con controles de acceso.
- Evitar exposición innecesaria de datos personales en reportes y logs.
- Registrar eventos de seguridad sin incluir contraseñas, tokens ni datos sensibles.

Variables esperadas:

```text
DATABASE_URL=
JWT_SECRET=
JWT_REFRESH_SECRET=
REPORT_STORAGE_PATH=
MAX_UPLOAD_SIZE=
```

## 9. Validaciones y Manejo de Errores

Validaciones mínimas:

- Campos obligatorios y tipos correctos.
- Fechas válidas: ingreso, apertura, caducidad y disposición.
- Cantidades no negativas y unidades de medida compatibles.
- Existencia de relaciones antes de crear referencias.
- Permisos suficientes para movimientos críticos.
- Estados permitidos para reactivos, lotes, alertas y movimientos.
- Restricciones para impedir consumo superior al stock disponible.
- Reglas de almacenamiento según peligrosidad o incompatibilidad definida.

Formato de error recomendado:

```json
{
  "error": {
    "code": "INSUFFICIENT_STOCK",
    "message": "La cantidad solicitada supera el inventario disponible del lote.",
    "details": {},
    "traceId": "request-trace-id"
  }
}
```

Criterios:

- Los mensajes deben ser claros para usuarios finales cuando se muestren en la interfaz.
- Los errores técnicos deben registrarse en logs, no exponerse directamente al cliente.
- Las operaciones críticas deben informar si el error es de validación, permisos o conflicto de estado.
- El frontend debe evitar confirmaciones ambiguas en acciones irreversibles o auditables.

## 10. Protocolos y Restricciones del Agente

Durante el desarrollo asistido por agente se deben cumplir las siguientes reglas:

- Leer el contexto del repositorio antes de modificar archivos.
- Mantener cambios pequeños, trazables y directamente relacionados con la tarea solicitada.
- No eliminar ni sobrescribir trabajo existente sin autorización explícita.
- No introducir dependencias nuevas sin justificar su necesidad.
- Respetar la arquitectura y convenciones ya establecidas en el proyecto.
- Actualizar documentación cuando cambien comandos, estructura, API o modelo de datos.
- Ejecutar verificaciones proporcionales al cambio realizado.
- Reportar claramente qué archivos fueron modificados y qué validaciones se ejecutaron.
- No versionar secretos ni datos reales de usuarios, laboratorios o inventarios.
- Evitar soluciones temporales sin registrarlas como deuda técnica.

## Consideraciones de Seguridad Química

Este sistema no reemplaza procedimientos institucionales, fichas de seguridad oficiales ni normativas aplicables. La información registrada debe apoyar la operación responsable del laboratorio, pero las decisiones sobre manipulación, almacenamiento y disposición de sustancias deben seguir protocolos internos y regulación vigente.

El sistema debe preservar trazabilidad, consistencia de inventario y claridad documental para facilitar auditorías y reducir riesgos operativos.

## Roadmap Inicial

Fases previstas:

1. Definición del modelo de datos y contratos principales.
2. Implementación de autenticación y roles.
3. CRUD de reactivos, lotes, laboratorios y ubicaciones.
4. Registro de movimientos de inventario.
5. Alertas por caducidad, bajo stock e incompatibilidad.
6. Adjuntos y fichas de seguridad.
7. Reportes operativos y de auditoría.
8. Hardening de seguridad y pruebas end-to-end.

## Estado del Proyecto

Estado actual: documentación inicial del repositorio.

El código fuente, comandos definitivos, migraciones y contratos de API se incorporarán progresivamente durante las sesiones de desarrollo.
