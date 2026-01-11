# Patrón: Flujo C1 - Issue a Pull Request

## Propósito

Este patrón describe el flujo estándar de trabajo desde la identificación de un problema hasta su resolución e integración, usando GitHub como sistema de validación técnica.

## Flujo Completo

```
Issue (fix-me) → Rama → PR → CI → Merge
```

## Descripción del Patrón

### 1. Issue (fix-me)

**Objetivo**: Identificar y documentar un problema único.

**Reglas**:
- Una issue = un problema
- El problema debe ser específico y accionable
- La issue describe qué está roto o qué falta

**Ejemplo**:
- "El formulario de login no valida emails correctamente"
- "Falta documentación sobre el proceso de deploy"

### 2. Rama

**Objetivo**: Crear una rama para la solución.

**Reglas**:
- Una rama = una solución
- El nombre de la rama debe ser descriptivo
- La rama se crea desde `main` (actualizada)

**Ejemplo**:
- `fix/login-email-validation`
- `docs/deploy-process`

### 3. Pull Request

**Objetivo**: Proponer la solución para revisión y validación.

**Reglas**:
- El PR debe referenciar la issue original
- El PR debe tener descripción clara de los cambios
- El PR debe estar dirigido a `main`

**Ejemplo**:
```
Fixes #123

- Agrega validación de formato de email en el formulario de login
- Actualiza tests para cubrir casos edge
```

### 4. CI (Continuous Integration)

**Objetivo**: Validación técnica automática.

**Reglas**:
- **CI define verdad**: Si el CI pasa, el código es técnicamente válido
- Si el CI falla, se corrige el código, no se bypassa el CI
- El CI debe estar en verde (SUCCESS) antes de merge

**Qué valida el CI**:
- El código compila
- Los tests pasan (si existen)
- El lint pasa (si existe)
- El build pasa (si existe)

### 5. Merge

**Objetivo**: Integrar la solución a `main`.

**Reglas**:
- Solo se hace merge si el CI está en verde
- GitHub rulesets bloquean merge si el CI no pasa
- Después del merge, la issue se cierra automáticamente (si se usa "Fixes #123")

## Reglas Operativas

### Una Issue = Un Problema

- No mezclar múltiples problemas en una sola issue
- Si un problema tiene múltiples partes, crear issues separadas o una issue con subtareas claras

### Una Rama = Una Solución

- No mezclar múltiples soluciones en una rama
- Si una solución requiere múltiples cambios, todos deben estar relacionados al mismo problema

### CI Define Verdad

- Si el CI pasa: el código es técnicamente válido
- Si el CI falla: el código tiene problemas técnicos que deben resolverse
- No hay excepciones: el CI es la autoridad técnica final

## Cuándo Usar Este Patrón

**Modo Normal de Trabajo**: Este patrón se usa para TODO cambio de código.

**Casos de Uso**:
- Corrección de bugs
- Implementación de features
- Mejoras de documentación
- Refactors
- Actualizaciones de dependencias

**No usar este patrón para**:
- Cambios que no requieren validación técnica (ej: actualizar README con información no técnica)
- Aunque incluso estos deberían pasar por CI si existe

## Ventajas

1. **Trazabilidad**: Cada cambio está vinculado a un problema documentado
2. **Validación Automática**: El CI previene integración de código roto
3. **Historial Claro**: El historial de Git muestra el flujo completo
4. **Revisión Controlada**: Los PRs permiten revisión antes de merge

## Referencias

- `decisions/github-as-judge.md` - Decisión que establece GitHub como autoridad técnica
- `recovery/required-status-checks-empty.md` - Solución cuando el CI no se registra

## Status

Patrón: ACTIVE
Última actualización: Establecimiento del flujo C1
Vigencia: Patrón estándar para todo trabajo de desarrollo
