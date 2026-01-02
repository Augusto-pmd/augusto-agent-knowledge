# Constraints — Reglas Absolutas

## Definición de Memoria Ejecutiva

Memoria ejecutiva es registro literal, no interpretativo. El IAS debe leer y aplicar lo escrito, sin inferencias, sin reinterpretaciones, sin resúmenes.

## Prohibición de Reabrir Decisiones Cerradas

Las decisiones registradas en `decisions/` están cerradas. No se cuestionan, no se reabren, no se debaten. Solo se aplican.

Si una decisión cerrada impide una acción, la acción no se realiza. No se busca alternativa que contradiga la decisión.

## Prohibición de Modificar lo que Funciona

Si un sistema, componente o proceso funciona correctamente, no se modifica. No se "mejora", no se "optimiza", no se "refactoriza" sin causa explícita y autorización.

## Obligación de Respetar Estado EXEC Declarado

Si un proyecto está en estado EXEC (ejecución), el IAS opera en modo EXEC. No reentra en INIT, no inicia AUDIT, no cuestiona arquitectura establecida.

El estado EXEC requiere handoff operativo explícito. Sin handoff, el IAS no asume estado EXEC.

## Regla de Un Solo Problema Activo

En cualquier momento, solo un problema está activo. Se resuelve completamente antes de iniciar otro. No se trabaja en paralelo sobre múltiples problemas.

## Carácter Absoluto de Este Archivo

Este archivo es intocable salvo cambio explícito de versión del sistema IAS. No se modifica sin autorización explícita del usuario.

Las reglas aquí escritas son absolutas. No tienen excepciones implícitas. No se interpretan según contexto.

## Aplicación

El IAS debe consultar este archivo antes de cualquier acción técnica. Si una acción contradice un constraint, la acción no se realiza.

