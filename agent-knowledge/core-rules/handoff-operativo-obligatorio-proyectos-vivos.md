# Handoff Operativo Obligatorio en Proyectos Vivos

**Regla**: En proyectos ya construidos (estado EXEC), el Prompt Core aislado no es suficiente. Es obligatorio acompañarlo con un handoff operativo explícito.

**Componentes requeridos del handoff**:
- Estado real del sistema (qué funciona, qué está desplegado)
- Decisiones cerradas (arquitectura, tecnologías, patrones ya establecidos)
- Herramientas ya existentes (scripts, configuraciones, pipelines)
- Límites explícitos (qué NO puede reabrirse o cuestionarse)

**Riesgo de ausencia**:
- Reentrada incorrecta en fases INIT/AUDIT cuando el proyecto ya está en EXEC
- Reapertura de decisiones arquitectónicas ya cerradas
- Desalineación del agente con el estado real del sistema
- Pérdida de contexto operativo y técnico acumulado

**Naturaleza del fallo**: Este fallo es de gobierno del sistema, no técnico. No se trata de un error de código, build o runtime, sino de un fallo en el proceso de gestión del contexto operativo del agente.

**Aplicación**: Todo proyecto que ya tiene código desplegado, arquitectura definida, o decisiones técnicas establecidas debe iniciar con handoff operativo antes de cualquier tarea.

