# Protocolo de Reinicio Correcto

## Propósito

Este documento define el protocolo correcto para reiniciar trabajo en un proyecto, especialmente aplicable después de un colapso de proceso o al iniciar una nueva fase. Previene la repetición del caos observado en Pulley v1.

## Protocolo de Reinicio

### Paso 1: Abrir Knowledge

**Acción**: Leer conocimiento existente antes de cualquier otra acción.

**Proceso**:
1. Leer `agent-knowledge/README.md` completo
2. Leer `agent-knowledge/constraints.md` completo
3. Leer `agent-knowledge/CORE_RULES.md`
4. Leer `agent-knowledge/DECISION_PRINCIPLES.md`
5. Consultar `decisions/` para decisiones aplicables al contexto actual
6. Consultar `failures/` para errores conocidos relevantes
7. Consultar `patterns/` para patrones aplicables

**Criterio de completitud**: Knowledge leído y contexto del proyecto entendido.

**Tiempo estimado**: 5-10 minutos

**Prohibición**: No avanzar al siguiente paso sin completar este.

---

### Paso 2: Actualizar Aprendizajes

**Acción**: Registrar cualquier aprendizaje nuevo o actualización de conocimiento.

**Proceso**:
- Si se descubrió un nuevo error → Registrar en `failures/`
- Si se tomó una nueva decisión → Registrar en `decisions/`
- Si se identificó un nuevo patrón → Registrar en `patterns/`
- Si se necesita una nueva constraint → Actualizar `constraints/`

**Criterio de completitud**: Conocimiento actualizado con aprendizajes recientes.

**Tiempo estimado**: 2-5 minutos (solo si hay aprendizajes nuevos)

**Nota**: Si no hay aprendizajes nuevos, este paso se omite pero se verifica explícitamente.

---

### Paso 3: Definir Prompt de Comienzo

**Acción**: Crear un prompt de comienzo explícito con objetivo único y claro.

**Contenido obligatorio**:
```
OBJETIVO: [objetivo único y específico]
FASE: [Fase 0/1/2/3 según decisions/pulley-process-reset-v2.md]
CONTEXTO: [contexto relevante del estado actual]
RESTRICCIONES: 
- [restricciones de la fase actual]
- [constraints aplicables de constraints/]
- [decisiones relevantes de decisions/]
```

**Ejemplo**:
```
OBJETIVO: Resolver bug crítico donde formulario de creación no ejecuta onSubmit
FASE: Fase 1 - Estabilidad Técnica
CONTEXTO: El formulario renderiza correctamente pero el handler onSubmit nunca se ejecuta. E2E tests fallan.
RESTRICCIONES:
- No hacer cambios de diseño visual (Fase 1)
- No mejorar UX hasta que funcione básicamente (Fase 1)
- Aplicar patrón de pulley-form-standard.md
- Verificar que E2E pasa después del fix
```

**Criterio de completitud**: Prompt de comienzo definido con todos los elementos requeridos.

**Tiempo estimado**: 2-3 minutos

**Prohibición**: No iniciar trabajo técnico sin prompt de comienzo definido.

---

### Paso 4: Recién Ahí Tocar Código

**Acción**: Solo después de completar pasos 1-3, iniciar trabajo técnico.

**Proceso**:
1. Verificar que pasos 1-3 están completos
2. Iniciar trabajo técnico según el objetivo definido
3. Respetar restricciones de la fase actual
4. No desviarse del objetivo único

**Criterio de completitud**: Trabajo técnico completado según objetivo.

---

## Señales de Alerta Temprana de Caos

Si aparecen estas señales durante el trabajo, **detener inmediatamente**:

### Señal 1: Múltiples Objetivos

**Síntoma**: Se mencionan o trabajan múltiples objetivos en la misma conversación.

**Ejemplo**: "Arreglemos el bug del formulario y también mejoremos el diseño del modal"

**Acción**: Detener, recordar que hay un objetivo único, decidir cuál es prioritario.

---

### Señal 2: Mezcla de Capas

**Síntoma**: Se trabaja simultáneamente en diseño + bugs + E2E + UX.

**Ejemplo**: "Mientras arreglamos el submit, cambiemos el color del botón y escribamos el test E2E"

**Acción**: Detener, recordar orden de fases, trabajar solo en la capa de la fase actual.

---

### Señal 3: UI Rota pero Se Continúa

**Síntoma**: La UI no renderiza o no funciona, pero se continúa con mejoras.

**Ejemplo**: "El modal no se monta, pero mientras tanto mejoremos su animación"

**Acción**: Detener, recordar regla: "Si la UI no renderiza, se detiene todo". Resolver primero.

---

### Señal 4: Salto de Fase

**Síntoma**: Se menciona o trabaja en una fase sin completar la anterior.

**Ejemplo**: "Estamos en Fase 1 pero mejoremos el diseño visual (Fase 3)"

**Acción**: Detener, verificar estado de fases, regresar a fase correcta si es necesario.

---

### Señal 5: Decisión Sin Knowledge

**Síntoma**: Se toma decisión técnica sin consultar knowledge existente.

**Ejemplo**: "Vamos a cambiar el patrón de formularios" sin leer `patterns/pulley-form-standard.md`

**Acción**: Detener, leer knowledge relevante, luego continuar.

---

### Señal 6: Sensación de "No Funciona Nada"

**Síntoma**: Múltiples problemas sin resolver, sistema inestable, imposible determinar progreso.

**Ejemplo**: "Arreglamos el submit pero ahora el modal no se abre, y el E2E falla, y el diseño se ve mal"

**Acción**: Detener completamente, aplicar protocolo de reinicio desde el principio.

---

## Qué Hacer Cuando Aparecen Señales

### Acción Inmediata

1. **Detener** todo trabajo técnico
2. **Identificar** qué señal apareció
3. **Evaluar** si se puede corregir en la misma conversación o requiere reinicio

### Si Se Puede Corregir en la Misma Conversación

1. Recordar el objetivo único
2. Recordar la fase actual y sus restricciones
3. Decidir qué trabajo es prioritario
4. Continuar solo con el trabajo prioritario
5. Registrar el otro trabajo para conversación futura

### Si Requiere Reinicio Completo

1. Aplicar `patterns/restart-protocol.md` desde el Paso 1
2. Definir nuevo prompt de comienzo con objetivo único
3. Verificar que se respetan todas las reglas de `constraints/conversation-governance.md`
4. Solo entonces continuar con trabajo técnico

---

## Verificación de Completitud

Antes de considerar una conversación completa, verificar:

- [ ] Knowledge fue leído al inicio
- [ ] Prompt de comienzo fue definido
- [ ] Objetivo único fue respetado
- [ ] Fase actual fue respetada (no se saltaron fases)
- [ ] Restricciones de fase fueron respetadas
- [ ] No se mezclaron capas (UX + bugs + E2E)
- [ ] Si la UI estaba rota, se resolvió primero
- [ ] Trabajo técnico completado según objetivo

Si alguna verificación falla, la conversación no está completa y requiere corrección.

---

## Aplicación

Este protocolo aplica a:
- Reinicio de trabajo después de un colapso de proceso
- Inicio de una nueva fase del proyecto
- Inicio de una nueva conversación sobre el proyecto
- Cuando aparecen señales de alerta de caos

## Referencias

- `decisions/pulley-process-reset-v2.md` - Define las fases que este protocolo implementa
- `constraints/conversation-governance.md` - Reglas que este protocolo aplica
- `failures/pulley-process-breakdown.md` - Describe el problema que este protocolo previene

## Status

Patrón: ACTIVE
Última actualización: Protocolo de reinicio para prevenir colapso de proceso
Vigencia: Aplicable a todos los reinicios de trabajo
