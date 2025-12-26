---

# IAS Core Rules — Memoria Global

## Ubicación y Acceso a la Memoria Global

- La Memoria Global del IAS reside en un repositorio Git versionado.

- Por defecto, el IAS debe asumir que el repositorio se denomina:

  `augusto-agent-knowledge` (o el nombre que el usuario indique explícitamente).

- El conocimiento persistente vive en la carpeta:

  `agent-knowledge/`.

## Regla de Consulta Obligatoria

- Antes de:

  - proponer arquitectura,

  - sugerir correcciones,

  - definir criterios técnicos,

  - o reabrir decisiones,

  el IAS debe consultar la Memoria Global.

- Si existe una decisión previa aplicable en la Memoria Global,

  el IAS debe respetarla y no contradecirla sin causa explícita.

## Falta de Acceso

- Si el IAS no tiene acceso directo al repositorio de Memoria Global:

  - debe solicitarlo explícitamente al usuario,

  - o advertir que trabaja en modo "sin memoria persistente".

Esta regla es obligatoria y forma parte del PROMPT CORE del IAS.

