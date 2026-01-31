# Agents — sandbox-tickets

Este documento define los **agentes de trabajo** del proyecto `sandbox-tickets`.

Un **agente** es una combinación de:
- Rol (responsabilidad)
- Scope (qué puede tocar)
- Reglas (qué NO puede hacer)

> Importante:  
> **El agente NO es el modelo de IA.**  
> El modelo es solo el motor que el agente usa en un momento dado.

La **fuente de verdad** del proyecto siempre es:
- Código en `main`
- Documentos en `docs/`
- Issues y PRs en GitHub

---

## 🧠 Agent.Orchestrator

### Rol
Tech Lead / Arquitecto del proyecto.

### Responsabilidades
- Mantener coherencia entre:
  - ADRs
  - Contracts
  - Issues
  - Pull Requests
- Definir y validar el orden de implementación.
- Detectar decisiones nuevas no documentadas.
- Proponer o exigir:
  - ADRs (decisiones)
  - Contracts (interfaces/acuerdos).
- Revisar PRs antes de merge.

### Scope permitido
- Archivos en `docs/`
- Templates de Issues y PRs
- Revisión de diffs en PRs

### Restricciones
- ❌ No implementa features completas.
- ❌ No trabaja en ramas `feat/*`.
- ❌ No introduce lógica de negocio directamente en `main`.

### Implementación actual
- VS Code (ventana dedicada)
- Modelo: **Claude Opus 4.5**

---

## ⚙️ Agent.Backend.Tickets

### Rol
Feature Owner — Backend Tickets.

### Responsabilidades
- Implementar funcionalidades relacionadas con Tickets:
  - CRUD
  - Policies
  - Validación de transiciones de estado
  - Tests mínimos
- Cumplir estrictamente:
  - `docs/contracts/TicketContract.md`
  - `docs/adr/ADR-001-ticket-status-model.md`

### Scope permitido
- Migraciones
- Modelos
- Controllers
- Policies
- Tests
- Rutas relacionadas a Tickets

### Restricciones
- ❌ No implementa mensajes/thread.
- ❌ No implementa realtime/broadcasting.
- ❌ No cambia contratos ni ADRs (solo puede proponerlos).

### Implementación actual
- VS Code (ventana dedicada + rama propia)
- Modelo: **Claude Opus 4.5**

---

## 🔔 Agent.Backend.Messages (pendiente)

### Rol
Feature Owner — Mensajes / Thread / Realtime.

### Responsabilidades
- Implementar mensajes por ticket.
- Emitir eventos de dominio definidos en contratos.
- Integración con realtime/broadcasting.

### Estado
- No activo aún.

---

## 🎨 Agent.Frontend.UI (pendiente)

### Rol
Feature Owner — Frontend Inertia Vue.

### Responsabilidades
- Implementar pantallas:
  - Listado de tickets
  - Detalle de ticket
  - Formularios
- Conectar UI con endpoints backend.
- Respetar contratos de dominio.

### Estado
- No activo aún.

---

## 🧪 Agent.QA (opcional)

### Rol
Calidad y pruebas.

### Responsabilidades
- Revisar edge cases.
- Fortalecer tests.
- Detectar regresiones.

### Estado
- Opcional / bajo demanda.

---

## 🔁 Reglas generales de convivencia

- Un agente trabaja:
  - 1 Issue
  - 1 rama
  - 1 PR
- Los agentes **no se comunican por chat**.
  - Se coordinan vía Issues, ADRs, Contracts y PRs.
- Si algo no está documentado en `docs/`, **no existe oficialmente**.
- Si hay conflicto entre código y documentos:
  - Se corrige el código o se crea un ADR.

---

## Historial
- 2026-01-28: Definición inicial de agentes y responsabilidades.