# Decisión: Tooling Stack del Proyecto

## Fecha

Registrado al establecer el estado final verificado del ecosistema PMD.

## Contexto

El proyecto PMD requiere definir las herramientas base, CI, y herramientas locales para mantener consistencia y evitar conflictos.

## Decisión

Stack técnico y herramientas definidas para el ecosistema PMD.

## Herramientas Base

### ChatGPT (IAS)
- **Función**: Decisión, criterio y gobierno
- **Uso**: Toma de decisiones técnicas y arquitectónicas

### Cursor
- **Función**: Ejecución
- **Uso**: Edición de código y ejecución de cambios

### GitHub
- **Función**: Versionado, CI y control
- **Uso**: Control de versiones, CI/CD, y rulesets

## Backend

### CI
- **CI validado**: Build y typecheck bloqueantes
- **Lint**: No bloqueante
- **Auditorías**: Semgrep activo (no bloqueante)
- **Super-Linter**: Activo (no bloqueante)

### Package Manager
- **Package manager**: Yarn
- **Lockfile**: `yarn.lock`

## Frontend

### CI
- **CI validado**: Build bloqueante
- **Lint**: No bloqueante
- **TypeScript global**: EXCLUIDO del CI (decisión consciente)
- **Tests y e2e**: Excluidos del typecheck en CI

### Package Manager
- **Package manager**: npm
- **Lockfile**: `package-lock.json`

## Herramientas Locales Verificadas

### Continue
- Uso: Tareas mecánicas (helpers, boilerplate, tests, refactors pequeños)
- **NO se usa para**: Decisiones de arquitectura, auth, permisos, modelos de dominio
- Ver: `patterns/continue-usage.md` para detalles

### GitLens
- Uso: Visualización de historial y cambios de Git

### Jest Runner
- Uso: Ejecución de tests unitarios

### SonarLint
- Uso: Code Metrics / Complexity
- Análisis de código en tiempo de desarrollo

### DocThis (AI Doc Generator)
- Uso: Generación automática de documentación de código

### CodeVisualizer (Dependency Graph)
- Uso: Visualización de dependencias y estructura del código

## Justificación

- **Consistencia**: Herramientas definidas previenen conflictos
- **Eficiencia**: Stack conocido facilita desarrollo
- **Calidad**: CI y auditorías aseguran estándares mínimos
- **Mantenibilidad**: Herramientas locales verificadas mejoran productividad

## Referencias

- `constraints/ci-governance.md` - Reglas de CI y package managers
- `patterns/continue-usage.md` - Uso de Continue en el proyecto

## Status

Decisión: CERRADA
Última actualización: Estado final verificado del ecosistema PMD
Vigencia: Aplicable a todo el ecosistema PMD
