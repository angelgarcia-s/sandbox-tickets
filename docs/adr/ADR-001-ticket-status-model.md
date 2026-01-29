

# ADR-001 — Modelo de estados del Ticket

## Estado
Aceptado

## Contexto
El sistema necesita un modelo claro y consistente para representar el ciclo de vida de un Ticket.
Este modelo debe ser:
- Fácil de entender por backend y frontend
- Extensible a futuro (SLA, escalaciones, automatizaciones)
- Seguro para trabajo multi-agente (reglas claras, sin ambigüedad)

Sin una definición explícita, cada agente podría asumir estados distintos o transiciones inválidas.

## Decisión
Se define un **modelo de estados finito** para Ticket con transiciones controladas.

### Estados permitidos
- `open` — Ticket recién creado, sin atención aún
- `in_progress` — Ticket siendo atendido por un agente
- `waiting` — En espera de información del usuario
- `resolved` — Problema resuelto, pendiente de cierre
- `closed` — Ticket cerrado definitivamente

### Transiciones válidas
- `open` → `in_progress`
- `in_progress` → `waiting`
- `waiting` → `in_progress`
- `in_progress` → `resolved`
- `resolved` → `closed`

No se permiten saltos directos que rompan la lógica del flujo.

## Alternativas consideradas
1. **Estados libres (string sin control)**
   - Rechazado: genera inconsistencia y bugs silenciosos.
2. **Soft deletes para cierre**
   - Rechazado: elimina información útil para auditoría.
3. **State machine externa**
   - Postergado: innecesario para el sandbox.

## Consecuencias
- Backend debe validar transiciones antes de persistir cambios.
- Frontend solo puede mostrar acciones permitidas según el estado.
- Realtime / eventos deben asumir este modelo como fuente de verdad.

## Alcance
Este ADR aplica a:
- Lógica de dominio del Ticket
- UI de estados y acciones
- Eventos de dominio relacionados a cambios de estado

## Fecha
2026-01-28