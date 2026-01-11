# Pulley v1: Desglose del Proceso

## Resumen

Este documento registra el colapso del proceso de desarrollo durante Pulley v1, donde la conversación se volvió caótica debido a la mezcla simultánea de múltiples capas de trabajo sin orden ni priorización clara.

## Descripción del Problema

Durante el desarrollo de Pulley v1, la conversación de desarrollo se caracterizó por la mezcla simultánea de:

1. **Diseño experimental** - Exploración de estilos, layouts y componentes visuales
2. **Bugs críticos** - Errores que impedían funcionalidad básica (formularios no funcionaban, modales no se montaban)
3. **Refactors arquitectónicos** - Cambios estructurales en componentes y flujos de datos
4. **E2E testing** - Implementación y corrección de tests end-to-end
5. **UX y accesibilidad** - Mejoras de experiencia de usuario mientras el sistema estaba roto

## Síntomas Observados

### 1. Decisiones Contradictorias

- Se tomaban decisiones de diseño mientras se corregían bugs que impedían que el diseño se viera
- Se implementaban mejoras de UX en componentes que no funcionaban técnicamente
- Se escribían E2E tests para flujos que cambiaban constantemente por refactors simultáneos

### 2. UI Inestable

- Componentes que "funcionaban" visualmente pero fallaban en interacción
- Formularios que se veían bien pero no ejecutaban submit
- Modales que aparecían pero no tenían elementos interactivos en DOM
- Estados visuales que no reflejaban el estado real del sistema

### 3. Sensación de "No Funciona Nada"

- Cada corrección revelaba nuevos problemas
- No había una base estable desde la cual construir
- El sistema estaba en un estado de "todo roto" constante
- Imposible determinar qué estaba realmente funcionando

### 4. Contexto Perdido

- Múltiples objetivos activos simultáneamente
- Prioridades cambiantes sin justificación clara
- No había un estado "verde" conocido desde el cual partir
- Cada cambio afectaba múltiples capas sin coordinación

## Causa Raíz

**Mezcla de capas sin orden ni priorización**

El proceso no respetó una secuencia lógica de trabajo:

```
❌ INCORRECTO (lo que pasó):
- Diseño + Bugs + E2E + UX + Refactors = TODO AL MISMO TIEMPO
- Resultado: Caos, decisiones contradictorias, sistema inestable

✅ CORRECTO (lo que debería ser):
- Fase 0: Knowledge (entender el sistema)
- Fase 1: Estabilidad técnica (bugs críticos primero)
- Fase 2: UX base conservador (funcionalidad básica)
- Fase 3: Evolución estética (mejoras visuales)
```

## Lecciones Clave

### 1. No Mezclar Capas

- **Técnico** (bugs, estabilidad) debe resolverse antes de **UX** (diseño, experiencia)
- **E2E** debe validar funcionalidad estable, no funcionalidad en construcción
- **Refactors** no deben hacerse mientras hay bugs críticos activos

### 2. No Diseñar Mientras el Sistema Está Roto

- Si un formulario no hace submit, no tiene sentido mejorar su estilo visual
- Si un modal no se monta en DOM, no tiene sentido ajustar su animación
- La estética es irrelevante si la funcionalidad básica no existe

### 3. Un Objetivo por Conversación

- Cada sesión debe tener un objetivo único y claro
- No saltar entre objetivos sin completar el anterior
- Si aparece un problema nuevo, decidir: ¿resolverlo ahora o registrar para después?

### 4. Estado Verde Antes de Evolución

- Debe existir un estado conocido y funcional antes de agregar complejidad
- E2E debe pasar antes de agregar nuevas features
- Build debe estar verde antes de cualquier cambio

## Impacto

- **Tiempo perdido**: Correcciones que se deshacían por cambios simultáneos
- **Deuda técnica**: Sistema inestable sin base sólida
- **Frustración**: Imposible determinar progreso real
- **Riesgo**: Cada cambio podía romper múltiples cosas sin detección temprana

## Prevención

1. **Fases estrictas**: No saltar entre fases sin completar la anterior
2. **Knowledge primero**: Siempre leer conocimiento existente antes de tocar código
3. **Un objetivo**: Una conversación = un objetivo claro
4. **Estado verde**: Verificar que el sistema funciona antes de evolucionarlo
5. **Detención temprana**: Si la UI no renderiza, detener todo y resolver primero

## Status

Documento: ACTIVE
Última actualización: Registro del colapso de proceso en Pulley v1
Relacionado: `decisions/pulley-process-reset-v2.md`, `constraints/conversation-governance.md`, `patterns/restart-protocol.md`
