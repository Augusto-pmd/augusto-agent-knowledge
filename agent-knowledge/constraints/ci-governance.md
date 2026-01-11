# Constraint: CI Governance

## Principio General

**El CI solo bloquea lo que el proyecto está en condiciones de cumplir hoy.**

## Backend

### CI Bloqueante
- **Build**: Debe pasar para que el PR sea mergeable
- **Typecheck**: Debe pasar para que el PR sea mergeable

### CI No Bloqueante
- **Lint**: No bloquea el merge
- **Auditorías**: Semgrep y Super-Linter (no bloqueantes)

### Package Manager
- **Package manager**: Yarn
- **Lockfile**: `yarn.lock`
- **Prohibido**: Usar npm / package-lock.json

## Frontend

### CI Bloqueante
- **Build**: Debe pasar para que el PR sea mergeable

### CI No Bloqueante
- **Lint**: No bloquea el merge

### TypeScript
- **TypeScript global**: EXCLUIDO del CI (decisión consciente)
- **Tests y e2e**: Excluidos del typecheck en CI

### Package Manager
- **Package manager**: npm
- **Lockfile**: `package-lock.json`

## Regla Transversal

**Prohibido mezclar gestores de paquetes dentro del mismo repositorio.**

## Referencias

- `decisions/tooling-stack.md` - Herramientas y stack técnico del proyecto

## Status

Constraint: ACTIVE
Última actualización: Estado final verificado del ecosistema PMD
Vigencia: Permanente
