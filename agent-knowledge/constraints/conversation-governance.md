# Reglas de Gobernanza de Conversaciones

## Propósito

Este documento establece reglas inquebrantables para todas las conversaciones de desarrollo, especialmente aplicables a Pulley v2.0 y proyectos futuros. Estas reglas previenen el colapso de proceso observado en Pulley v1.

## Reglas Absolutas

### 1. Knowledge SIEMPRE Primero

**Regla**: Antes de cualquier acción técnica, leer conocimiento existente.

**Proceso obligatorio**:
1. Leer `agent-knowledge/README.md`
2. Leer `agent-knowledge/constraints.md` completo
3. Consultar `decisions/` para decisiones aplicables al contexto
4. Consultar `failures/` para errores conocidos relevantes
5. Consultar `patterns/` para patrones aplicables

**Violación**: No se puede tocar código sin completar este proceso.

**Justificación**: El conocimiento existente previene repetir errores y aplica decisiones ya tomadas.

---

### 2. Prompt de Comienzo Obligatorio

**Regla**: Cada conversación debe comenzar con un prompt de comienzo explícito.

**Contenido mínimo del prompt**:
- Objetivo único y claro de la conversación
- Fase actual del proyecto (según `decisions/pulley-process-reset-v2.md`)
- Contexto relevante
- Restricciones aplicables

**Formato sugerido**:
```
OBJETIVO: [objetivo único]
FASE: [Fase 0/1/2/3 según corresponda]
CONTEXTO: [contexto relevante]
RESTRICCIONES: [restricciones de fase y constraints aplicables]
```

**Violación**: No iniciar trabajo técnico sin prompt de comienzo.

**Justificación**: Un objetivo claro previene mezcla de capas y objetivos múltiples.

---

### 3. Un Objetivo por Conversación

**Regla**: Cada conversación tiene un único objetivo. No se mezclan objetivos.

**Aplicación**:
- Si aparece un problema nuevo durante la conversación, decidir:
  - ¿Es crítico y bloquea el objetivo actual? → Resolver primero
  - ¿No es crítico? → Registrar para conversación futura
- No saltar entre objetivos sin completar el actual
- No mezclar: diseño + bugs + E2E + UX en la misma conversación

**Violación**: Mezclar múltiples objetivos en una sola conversación.

**Justificación**: Múltiples objetivos simultáneos generan caos y decisiones contradictorias.

---

### 4. No Mezclar UX + Bugs + E2E

**Regla**: No trabajar simultáneamente en UX, bugs y E2E.

**Orden correcto** (según fases en `decisions/pulley-process-reset-v2.md`):
1. **Primero**: Bugs críticos (Fase 1: Estabilidad Técnica)
2. **Segundo**: UX base funcional (Fase 2: UX Base Conservador)
3. **Tercero**: E2E debe validar funcionalidad estable, no funcionalidad en construcción

**Violación**: 
- Diseñar mientras hay bugs críticos
- Escribir E2E para flujos que cambian constantemente
- Mejorar UX en componentes que no funcionan técnicamente

**Justificación**: Mezclar estas capas genera inestabilidad y decisiones contradictorias.

---

### 5. Si la UI No Renderiza, Se Detiene Todo

**Regla**: Si la UI no renderiza o no funciona básicamente, detener todo trabajo y resolver primero.

**Aplicación**:
- Si un componente no se muestra → Detener y resolver
- Si un formulario no hace submit → Detener y resolver
- Si un modal no se monta → Detener y resolver
- Si hay errores de compilación → Detener y resolver

**Violación**: Continuar con diseño, UX o E2E mientras la UI básica no funciona.

**Justificación**: No tiene sentido mejorar algo que no existe o no funciona.

---

### 6. Verificación de Fase Antes de Avanzar

**Regla**: Antes de iniciar trabajo de una fase, verificar que la fase anterior está completa.

**Proceso**:
- Fase 1 requiere: Fase 0 completa (knowledge leído, prompt definido)
- Fase 2 requiere: Fase 1 completa (build verde, E2E pasando, funcionalidad básica estable)
- Fase 3 requiere: Fase 2 completa (UX base funcional, flujos completables)

**Violación**: Iniciar trabajo de una fase sin completar la anterior.

**Justificación**: Saltar fases reintroduce el caos de Pulley v1.

---

### 7. Registro de Desviaciones

**Regla**: Si es necesario desviarse de estas reglas, debe registrarse explícitamente.

**Proceso**:
- Explicar por qué la desviación es necesaria
- Registrar en `decisions/` si es una decisión significativa
- Documentar consecuencias esperadas

**Violación**: Desviarse sin registro ni justificación.

**Justificación**: Las desviaciones sin registro generan inconsistencias y pérdida de conocimiento.

---

## Señales de Alerta Temprana

Si aparecen estas señales, **detener y evaluar**:

1. **Múltiples objetivos mencionados** en la misma conversación
2. **Mezcla de capas**: diseño + bugs + E2E mencionados juntos
3. **UI rota pero se continúa con mejoras**: "arreglemos el estilo mientras arreglamos el bug"
4. **Saltos de fase**: mencionar Fase 3 cuando Fase 1 no está completa
5. **Decisión sin knowledge**: tocar código sin leer conocimiento existente
6. **Sensación de "no funciona nada"**: múltiples problemas sin resolver

**Acción ante señales**: Detener, evaluar, y aplicar `patterns/restart-protocol.md` si es necesario.

---

## Aplicación

Estas reglas aplican a:
- Todas las conversaciones sobre Pulley v2.0
- Cualquier proyecto que use este repositorio de conocimiento
- El Knowledge Architect en todas sus interacciones

## Referencias

- `decisions/pulley-process-reset-v2.md` - Define las fases que estas reglas gobiernan
- `failures/pulley-process-breakdown.md` - Describe el problema que estas reglas previenen
- `patterns/restart-protocol.md` - Protocolo de reinicio que aplica estas reglas

## Status

Documento: ACTIVE
Última actualización: Reglas de gobernanza para prevenir colapso de proceso
Vigencia: Permanente, aplicable a todas las conversaciones futuras
