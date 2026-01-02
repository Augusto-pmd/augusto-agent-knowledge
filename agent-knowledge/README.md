# Memoria Ejecutiva — IAS

## Propósito

Este repositorio contiene la memoria ejecutiva rígida del IAS (Intelligent Agent System). Funciona como fuente única de memoria persistente confiable, no interpretativa.

## Regla de Lectura Obligatoria

**ANTES de cualquier acción técnica**, el IAS debe:

1. Leer este README.md
2. Leer constraints.md completo
3. Consultar decisions/ para decisiones cerradas aplicables
4. Consultar failures/ para errores conocidos relevantes
5. Consultar patterns/ para patrones aplicables

**La lectura es obligatoria, no opcional.**

## Jerarquía de Prioridad

1. **constraints.md** — Absoluto. No se discute, no se interpreta.
2. **decisions/** — Decisiones cerradas. No se reabren sin autorización explícita.
3. **failures/** — Errores conocidos. Evitar repetición.
4. **patterns/** — Patrones probados. Aplicar cuando corresponda.

## Prohibiciones Explícitas

- **NO inferir**: Si no está escrito, no existe.
- **NO reinterpretar**: El texto es literal, no metafórico.
- **NO resumir**: Citar archivos concretos, no interpretaciones.
- **NO mover archivos** entre carpetas sin autorización explícita del usuario.
- **NO crear contenido ficticio** en decisions/, failures/, patterns/ sin registro real aprobado.

## Obligación de Citar

Al tomar decisiones técnicas, el IAS debe:

- Citar archivos concretos de este repositorio.
- Indicar qué constraint, decision, failure o pattern aplica.
- No basarse en memoria de conversación si existe registro aquí.

## Estructura

```
agent-knowledge/
├── README.md          (este archivo)
├── constraints.md     (reglas absolutas)
├── decisions/         (decisiones cerradas)
├── failures/          (errores conocidos)
└── patterns/          (patrones probados)
```

## Actualización

- Solo se registra conocimiento real, no anticipaciones.
- Los conflictos entre registros detienen la ejecución.
- Ante conflicto, se informa y se solicita decisión del usuario.
