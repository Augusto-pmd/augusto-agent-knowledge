# Decisión: Cierre de Pulley v1 y Reinicio como Pulley v2.0

## Fecha

Registrado al cierre de Pulley v1

## Contexto

Pulley v1 alcanzó un estado de caos donde:
- La conversación mezclaba simultáneamente diseño, bugs, refactors, E2E y UX
- El sistema estaba inestable sin base funcional clara
- Decisiones contradictorias generaban más problemas
- No había un estado "verde" conocido desde el cual construir

La visión del proyecto era correcta, pero el proceso de desarrollo fue incorrecto.

## Opciones Consideradas

### Opción A: Continuar con Pulley v1 (rechazada)
- Intentar estabilizar el sistema actual
- Riesgo: Continuar con deuda técnica y proceso caótico
- Problema: No hay base estable desde la cual partir

### Opción B: Cerrar v1 y reiniciar como v2.0 (elegida)
- Cerrar formalmente Pulley v1
- Reiniciar como Pulley v2.0 con proceso estructurado
- Aplicar aprendizajes del colapso de v1
- Beneficio: Proceso limpio con fases estrictas

## Decisión

**Cerrar Pulley v1 formalmente e iniciar Pulley v2.0 con proceso estructurado en fases estrictas.**

## Justificación

### Visión Correcta, Proceso Incorrecto

- La visión arquitectónica y de producto de Pulley es válida
- Los problemas fueron de proceso, no de diseño
- El conocimiento técnico adquirido es valioso y debe preservarse
- Un reinicio con proceso correcto permitirá ejecutar la visión adecuadamente

### Necesidad de Base Limpia

- Pulley v1 no tiene un estado "verde" conocido
- Es más eficiente reiniciar con proceso correcto que intentar estabilizar el caos
- Las fases estrictas previenen la repetición del colapso

## Definición de Fases Estrictas

### Fase 0: Knowledge

**Objetivo**: Entender el sistema antes de tocar código.

**Actividades**:
- Leer `agent-knowledge/README.md`
- Leer `agent-knowledge/constraints.md`
- Consultar `decisions/` para decisiones aplicables
- Consultar `failures/` para errores conocidos
- Consultar `patterns/` para patrones aplicables
- Definir prompt de comienzo con objetivo único y claro

**Criterio de completitud**: Knowledge leído y prompt de comienzo definido.

**Prohibición**: No tocar código hasta completar esta fase.

---

### Fase 1: Estabilidad Técnica

**Objetivo**: Sistema funcional básico sin bugs críticos.

**Actividades**:
- Resolver bugs que impiden funcionalidad básica
- Verificar que formularios hacen submit
- Verificar que modales se montan correctamente
- Verificar que E2E tests pasan
- Build verde

**Criterio de completitud**: 
- Build pasa
- E2E tests pasan
- Funcionalidad básica verificada manualmente

**Prohibición**: 
- No diseño visual
- No mejoras de UX
- No refactors no críticos
- Solo estabilización técnica

---

### Fase 2: UX Base Conservador

**Objetivo**: Experiencia de usuario funcional y predecible.

**Actividades**:
- Implementar UX básica y conservadora
- Asegurar que errores se muestran visiblemente
- Asegurar feedback claro de acciones del usuario
- Validar accesibilidad básica
- E2E tests deben seguir pasando

**Criterio de completitud**:
- Usuario puede completar flujos principales
- Errores son visibles y comprensibles
- Feedback de acciones es claro
- E2E tests pasan

**Prohibición**:
- No diseño experimental
- No mejoras estéticas
- Solo UX funcional básica

---

### Fase 3: Evolución Estética

**Objetivo**: Mejoras visuales y de experiencia una vez que la base es sólida.

**Actividades**:
- Mejoras de diseño visual
- Animaciones y transiciones
- Refinamiento estético
- Optimizaciones de UX avanzadas

**Criterio de completitud**: Mejoras implementadas sin romper funcionalidad.

**Prohibición**: No romper funcionalidad existente.

---

## Prohibición de Saltar Fases

**Regla absoluta**: No se puede saltar una fase sin completarla.

- No se puede ir de Fase 0 a Fase 2
- No se puede ir de Fase 1 a Fase 3
- Cada fase debe completarse antes de iniciar la siguiente
- Si aparece un problema de una fase anterior, se detiene el trabajo y se regresa a esa fase

## Consecuencias

### Positivas

- Proceso predecible y ordenado
- Base sólida antes de agregar complejidad
- Menor riesgo de colapso por mezcla de capas
- Conocimiento aplicado desde el inicio

### Requisitos

- Disciplina estricta en seguir fases
- No ceder a tentación de "arreglar rápido" algo de otra fase
- Verificación explícita de completitud antes de avanzar

## Aplicación

Esta decisión aplica a:
- Todas las conversaciones futuras sobre Pulley v2.0
- Cualquier desarrollo relacionado con Pulley
- El proceso de trabajo del Knowledge Architect

## Referencias

- `failures/pulley-process-breakdown.md` - Descripción del problema que motivó esta decisión
- `constraints/conversation-governance.md` - Reglas para aplicar esta decisión
- `patterns/restart-protocol.md` - Protocolo de reinicio que implementa esta decisión

## Status

Decisión: CERRADA
Última actualización: Cierre formal de Pulley v1 e inicio de Pulley v2.0
Vigencia: A partir de esta fecha, todas las conversaciones sobre Pulley deben seguir este proceso
