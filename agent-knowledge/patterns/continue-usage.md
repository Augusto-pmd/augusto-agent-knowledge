# Patrón: Uso de Continue en el Proyecto

## Propósito

Este patrón define cuándo y cómo usar Continue en el proyecto PMD, estableciendo límites claros para su uso.

## Cuándo Usar Continue

Continue se usa **solo para tareas mecánicas**:

### Tareas Apropiadas
- **Helpers**: Funciones auxiliares y utilidades
- **Boilerplate**: Código repetitivo y estructura básica
- **Tests**: Tests unitarios y de integración
- **Refactors pequeños**: Refactorizaciones menores y localizadas

### Ejemplos
- Crear funciones helper para formateo de datos
- Generar estructura básica de componentes
- Escribir tests para funciones existentes
- Renombrar variables y funciones en archivos pequeños

## Cuándo NO Usar Continue

Continue **NO se usa para**:

### Tareas NO Apropiadas
- **Decisiones de arquitectura**: Cambios estructurales mayores
- **Auth**: Lógica de autenticación y autorización
- **Permisos**: Gestión de permisos y roles
- **Modelos de dominio**: Cambios en entidades y modelos de negocio

### Ejemplos
- Decidir estructura de módulos o servicios
- Implementar lógica de autenticación
- Cambiar permisos de acceso
- Modificar entidades TypeORM o modelos de dominio

## Flujo Obligatorio

El flujo correcto cuando se usa Continue:

1. **ChatGPT (IAS) decide**: Define qué se debe hacer y cómo
2. **Continue ejecuta**: Genera el código según la decisión
3. **Cursor aplica**: Revisa y aplica los cambios
4. **GitHub versiona**: Commits y PRs según el flujo estándar

## Regla de Seguridad

**Ante duda, no usar Continue.**

Si hay incertidumbre sobre si Continue es apropiado para una tarea:
- Usar ChatGPT (IAS) para decisión
- Usar Cursor para ejecución manual
- Documentar la decisión si es necesario

## Justificación

- **Seguridad**: Previene cambios inadecuados en código crítico
- **Calidad**: Mantiene decisiones arquitectónicas consistentes
- **Control**: Asegura que las decisiones importantes pasen por ChatGPT (IAS)
- **Eficiencia**: Usa Continue solo donde es realmente útil

## Referencias

- `decisions/tooling-stack.md` - Stack técnico completo del proyecto
- `constraints/ci-governance.md` - Reglas de CI y validación

## Status

Patrón: ACTIVE
Última actualización: Estado final verificado del ecosistema PMD
Vigencia: Aplicable a todo el proyecto PMD
