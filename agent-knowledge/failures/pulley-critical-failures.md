# Pulley Critical Failures

## Resumen

Este documento registra los fallos críticos detectados durante el desarrollo del sistema Pulley, sus síntomas, métodos de detección y medidas preventivas.

## 1. Validación HTML nativa bloqueando submit JS

### Síntomas

- El formulario no ejecuta el handler `onSubmit`
- No aparecen errores en consola
- El submit parece "no hacer nada"
- Funciona en algunos navegadores pero no en otros

### Causa

Los atributos HTML de validación nativa (`required`, `type="email"`, `pattern`, etc.) interceptan el submit antes de que llegue al handler JavaScript. Si la validación HTML falla, el navegador previene el submit y nunca se ejecuta `onSubmit`.

### Detección

- E2E tests fallan sin errores visibles
- Inspección manual muestra que `onSubmit` nunca se ejecuta
- Console.log dentro de `onSubmit` no aparece

### Prevención

- Usar `noValidate` en todos los `<form>`
- Implementar validación exclusivamente en JavaScript
- Manejar errores de forma explícita y visible

---

## 2. Formularios "mudos" sin errores visibles

### Síntomas

- Usuario completa formulario y hace submit
- Nada sucede visualmente
- No hay mensajes de error
- No hay feedback de éxito
- El usuario no sabe si el formulario funcionó

### Causa

- Validación HTML silenciosa bloqueando submit
- Errores de validación no se muestran en UI
- Estados de error no se renderizan
- Falta de manejo de estados de loading/error/success

### Detección

- Testing manual: usuario no puede determinar el estado del formulario
- E2E tests: no hay elementos de error visibles cuando deberían existir
- Accesibilidad: screen readers no anuncian errores

### Prevención

- Siempre mostrar errores de validación de forma visible
- Implementar estados de UI claros (idle, loading, error, success)
- Usar ARIA labels para accesibilidad
- E2E tests deben verificar presencia de mensajes de error

---

## 3. Modales no montados en DOM (submit inexistente)

### Síntomas

- E2E test intenta hacer submit en modal
- Error: elemento no encontrado
- El modal "aparece" visualmente pero el form no existe en DOM
- Submit falla con timeout

### Causa

- El modal se renderiza condicionalmente solo cuando está "abierto"
- El form dentro del modal no existe hasta que el modal se monta
- E2E intenta interactuar antes de que el DOM esté listo
- Race condition entre renderizado y interacción

### Detección

- E2E tests fallan con "element not found"
- Inspección de DOM muestra que el form no existe cuando el test intenta acceder
- Timing issues: funciona a veces, falla otras

### Prevención

- Montar el form del modal inmediatamente (oculto si es necesario)
- Usar `data-testid` para elementos críticos
- Esperar explícitamente a que elementos estén disponibles en E2E
- Evitar renderizado condicional de elementos interactivos críticos

---

## 4. Overlay visual interceptando eventos

### Síntomas

- Click en botón no funciona
- Hover no se activa
- Elementos parecen "no responder"
- E2E tests fallan al intentar hacer click

### Causa

- Capas decorativas (overlays, gradientes, efectos visuales) con `z-index` alto
- Estas capas no tienen `pointer-events: none`
- Interceptan eventos de mouse/touch antes de llegar a elementos interactivos
- El elemento visualmente visible no es el que recibe el evento

### Detección

- Inspección de elementos muestra que el click se registra en overlay, no en botón
- E2E tests fallan al intentar interactuar
- Funciona al hacer click en bordes pero no en centro (donde está el overlay)

### Prevención

- Aplicar `pointer-events: none` a capas puramente decorativas
- Verificar que elementos interactivos tengan `z-index` apropiado
- E2E tests deben validar que los clicks llegan a los elementos correctos

---

## 5. Prisma enum vs TEXT en PostgreSQL

### Síntomas

- Migración de Prisma falla en producción
- Error: tipo enum no existe
- Funciona en desarrollo, falla en producción
- Base de datos tiene columnas como TEXT en lugar de enum

### Causa

- Prisma schema define enum pero la migración no se aplicó correctamente
- PostgreSQL requiere creación explícita del tipo enum antes de usarlo
- Migraciones aplicadas en orden incorrecto
- Base de datos en producción tiene schema diferente a desarrollo

### Detección

- Migración falla con error de tipo no encontrado
- Inspección de schema de PostgreSQL muestra TEXT en lugar de enum
- Comparación entre schemas de desarrollo y producción

### Prevención

- Verificar que migraciones se aplican en todos los entornos
- Usar `prisma migrate deploy` en producción (no `prisma migrate dev`)
- Validar schema después de migraciones
- No asumir que enums existen sin verificar migración

---

## 6. Emma sin tabla en DB (backend incompleto)

### Síntomas

- Frontend intenta acceder a entidad "Emma"
- Error 500 en backend
- Logs muestran: tabla no existe
- Feature parece implementada pero no funciona

### Causa

- Modelo definido en código pero migración nunca creada/aplicada
- Backend tiene lógica para "Emma" pero la tabla física no existe
- Desarrollo parcial: código escrito pero infraestructura no completada

### Detección

- E2E tests fallan con errores de base de datos
- Logs de backend muestran errores de SQL
- Inspección de base de datos: tabla faltante
- QA de API falla al intentar usar endpoints relacionados

### Prevención

- Validar que todas las entidades tienen migraciones aplicadas
- E2E tests deben cubrir flujos completos end-to-end
- No considerar feature completa hasta que E2E pase
- Verificar existencia de tablas antes de implementar lógica de negocio

---

## Principios de Prevención

1. **E2E es la fuente de verdad**: Si E2E no pasa, el feature no está completo
2. **Validación explícita**: Nunca confiar en validación HTML nativa para lógica de negocio
3. **DOM listo**: Elementos interactivos deben existir antes de intentar interactuar
4. **Eventos no interceptados**: Capas decorativas no deben bloquear interacciones
5. **Schema sincronizado**: Código y base de datos deben estar alineados
6. **Completitud verificable**: Features completos solo cuando E2E pasa

## Status

Documento: ACTIVE
Última actualización: Registro de fallos críticos del sistema Pulley

