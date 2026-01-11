# Patrón: Checklist de Alineación de Base de Datos

## Propósito

Este patrón proporciona un checklist para verificar la alineación entre entities y base de datos antes de hacer deploy cuando se modifican entities.

## Cuándo Usar Este Patrón

**Antes de hacer deploy cuando se modifican entities.**

Este checklist debe ejecutarse siempre que:
- Se agregan campos nuevos a entities
- Se modifican tipos de campos en entities
- Se crean nuevas entities
- Se cambian relaciones entre entities

## Checklist Pre-Deploy

### 1. ¿La DB es nueva o viva?

- [ ] **DB nueva**: No tiene tablas existentes → Migraciones aplican
- [ ] **DB viva**: Tiene tablas y datos existentes → SQL manual requerido

**Regla**: Si la DB es viva, seguir protocolo de `decisions/db-live-protocol.md`

### 2. ¿Hay columnas nuevas en entities?

- [ ] Verificar si entities incluyen campos nuevos vs. DB real
- [ ] Comparar estructura de entities con schema de PostgreSQL
- [ ] Documentar todas las diferencias detectadas

**Herramienta**: Conectar a DB y ejecutar:
```sql
SELECT column_name, data_type 
FROM information_schema.columns 
WHERE table_name = 'nombre_tabla';
```

### 3. ¿Existe SQL manual preparado?

- [ ] SQL idempotente preparado (`ALTER TABLE IF NOT EXISTS`)
- [ ] SQL verificado contra estructura real de DB
- [ ] SQL documentado para ejecución manual

**Ejemplo**:
```sql
ALTER TABLE users ADD COLUMN IF NOT EXISTS phone VARCHAR(50);
ALTER TABLE works ADD COLUMN IF NOT EXISTS post_closure_enabled_by_id UUID;
```

### 4. ¿Se evitó migrationsRun en DB viva?

- [ ] Verificar que `migrationsRun: false` en producción
- [ ] Confirmar que no se intentarán migraciones base en DB viva
- [ ] Asegurar que el código no depende de migraciones automáticas

## Regla Operativa

**Primero alinear DB, luego desplegar código.**

### Orden Correcto

1. **Alineación de DB**:
   - Ejecutar SQL manual en PostgreSQL de Render
   - Verificar que cambios se aplicaron correctamente
   - Confirmar estructura alineada con entities

2. **Deploy de código**:
   - Desplegar código con entities actualizadas
   - Verificar que backend arranca sin errores
   - Confirmar que queries funcionan correctamente

### Orden Incorrecto

❌ Desplegar código primero, luego alinear DB
- Causa errores 500 (columnas faltantes)
- Backend no arranca correctamente
- Requiere rollback o fix de emergencia

## Ejemplo de Uso

### Escenario: Agregar campo `phone` a entity `User`

1. **Checklist**:
   - [x] DB viva (tiene tabla `users` existente)
   - [x] Columna nueva en entity: `phone: string`
   - [x] SQL preparado: `ALTER TABLE users ADD COLUMN IF NOT EXISTS phone VARCHAR(50);`
   - [x] `migrationsRun: false` en producción

2. **Ejecutar SQL en Render**:
   - Conectar vía DBeaver / psql
   - Ejecutar SQL manual
   - Verificar columna creada

3. **Deploy código**:
   - Desplegar código con entity `User` actualizada
   - Verificar que backend arranca
   - Confirmar que queries funcionan

## Referencias

- `decisions/db-live-protocol.md` - Protocolo para bases de datos vivas
- `constraints/infra-render.md` - Limitaciones de infraestructura en Render
- `failures/render-deploy-history.md` - Casos reales de desalineación

## Status

Patrón: ACTIVE
Última actualización: Checklist establecido después de incidente de desalineación
Vigencia: **Protocolo fijo para PMD**
