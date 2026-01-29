

# TicketContract — Contrato de dominio del Ticket

## Propósito
Definir, de forma estable y compartida, qué es un **Ticket** en el sistema `sandbox-tickets`, incluyendo sus campos mínimos, reglas y estados.
Este contrato permite que backend, frontend y realtime trabajen en paralelo sin asumir cosas distintas.

## Fuente de verdad
- Estados y transiciones: ver `docs/adr/ADR-001-ticket-status-model.md`.

## Entidad: Ticket

### Identidad
- `id`: integer (auto incremental)

### Relación (mínima)
- `user_id`: integer (usuario que creó el ticket)

### Atributos mínimos
- `title`: string (máx. 120)
- `description`: text (opcional)
- `status`: enum/string controlado (ver Estados permitidos)
- `priority`: enum/string controlado (ver Prioridades permitidas)
- `assigned_to_user_id`: integer (opcional; agente asignado)
- `created_at`, `updated_at`: timestamps

### Índices sugeridos (no obligatorios)
- index en `status`
- index en `assigned_to_user_id`
- index compuesto en (`status`, `assigned_to_user_id`)

## Estados permitidos
Tomados del ADR-001:
- `open`
- `in_progress`
- `waiting`
- `resolved`
- `closed`

## Transiciones válidas
Tomadas del ADR-001:
- `open` → `in_progress`
- `in_progress` → `waiting`
- `waiting` → `in_progress`
- `in_progress` → `resolved`
- `resolved` → `closed`

## Prioridades permitidas
Para el sandbox, se define una prioridad simple:
- `low`
- `normal`
- `high`
- `urgent`

Regla: si no se especifica, default = `normal`.

## Reglas de negocio (mínimas)
1. Un ticket siempre debe tener `title`.
2. `status` siempre debe ser uno de los estados permitidos.
3. Las transiciones de `status` deben validarse (no se permiten saltos).
4. `assigned_to_user_id` es opcional; si existe, debe referir a un usuario válido.
5. `priority` debe ser una de las prioridades permitidas.

## Permisos (alto nivel)
Esto NO sustituye Policies, pero define el contrato de intención:

- `admin`
  - Puede crear, leer, actualizar, asignar y cerrar tickets.
- `agent`
  - Puede leer tickets asignados, cambiar estado dentro de transiciones válidas, y responder mensajes.
  - Puede reasignar solo si se permite explícitamente en el Issue correspondiente (por defecto NO).
- `viewer` (usuario final)
  - Puede crear tickets propios, leer tickets propios, responder mensajes en tickets propios.

Nota: el detalle se implementará con Policies/Gates y puede refinarse en futuros ADRs.

## API / UI: campos esperados (mínimos)

### Crear ticket (input)
- `title` (required)
- `description` (optional)
- `priority` (optional)

Resultado esperado:
- ticket creado con `status = open`
- `user_id` = usuario autenticado

### Cambiar estado (input)
- `status` (required; debe ser transición válida desde el estado actual)

### Asignación (input)
- `assigned_to_user_id` (required; usuario existente)

## Eventos de dominio (para realtime)
El sistema puede emitir eventos (nombres propuestos):
- `TicketCreated`
- `TicketStatusChanged`
- `TicketAssigned`

Este contrato no define el mecanismo (broadcast/queue), solo el “qué”.

## Fuera de alcance (por ahora)
- SLA y vencimientos
- Etiquetas/tags
- Adjuntos
- Escalaciones automáticas
- Múltiples departamentos / colas

## Historial de cambios
- 2026-01-28: Versión inicial del contrato.