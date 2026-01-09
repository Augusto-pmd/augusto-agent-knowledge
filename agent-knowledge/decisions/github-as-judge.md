# Decisión: GitHub como Juez Técnico Final

## Fecha

Registrado al establecer el flujo CI obligatorio con GitHub rulesets.

## Contexto

El proyecto requiere un mecanismo de validación técnica automática que prevenga merges de código que no cumpla con estándares mínimos de calidad y funcionalidad.

## Decisión

**GitHub (rulesets + CI) es la autoridad técnica final para validar cambios al código.**

### Principios

1. **GitHub como Juez**: Los rulesets de GitHub y los status checks de CI determinan si un cambio puede integrarse a `main`.

2. **CI Obligatorio**: Ningún merge a `main` ocurre sin que el CI esté en estado verde (SUCCESS).

3. **Cursor/IA Ejecutan, No Validan**: 
   - Cursor y las IAs ejecutan el trabajo técnico (código, commits, PRs)
   - GitHub valida técnicamente mediante CI y rulesets
   - La validación técnica final es automática, no humana ni de IA

4. **Sin Excepciones**: No hay bypass manual de CI. Si el CI falla, el código no se integra.

## Justificación

- **Objetividad**: La validación automática elimina sesgos y errores humanos
- **Consistencia**: Todos los cambios pasan por el mismo proceso de validación
- **Trazabilidad**: GitHub registra el estado de cada check para auditoría
- **Prevención**: Evita integración de código roto o que no compila

## Aplicación

Esta decisión aplica a:
- Todos los Pull Requests contra `main`
- Todos los cambios de código en el repositorio
- Cualquier flujo de trabajo que modifique código

## Referencias

- `patterns/flow-c1-issue-to-pr.md` - Patrón de trabajo que implementa esta decisión
- `recovery/required-status-checks-empty.md` - Solución cuando los checks no están registrados

## Status

Decisión: CERRADA
Última actualización: Establecimiento de CI obligatorio con GitHub rulesets
Vigencia: Permanente
