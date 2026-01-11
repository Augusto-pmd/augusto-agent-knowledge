# Decisión: Protocolo PMD — Base de Datos Viva

## Fecha

Registrado después del incidente de desalineación entre entities y base de datos de producción.

## Contexto

El proyecto PMD backend (NestJS + TypeORM) se despliega en Render con PostgreSQL. La base de datos ya estaba viva y contenía tablas creadas previamente. Se detectaron errores 500 causados por columnas faltantes (error PostgreSQL 42703). Las entities estaban más avanzadas que la DB. El uso de `migrationsRun: true` resultó incorrecto para una DB viva.

## Principio Central

**En una DB viva, la verdad es la estructura real, no el historial de migraciones.**

## Decisión

Protocolo para manejar bases de datos vivas en producción.

## Reglas

### 1. Nunca usar migrationsRun: true en una DB viva

- `migrationsRun: true` solo aplica a bases de datos nuevas
- En DB viva, puede intentar recrear tablas existentes (error 42P07)
- No debe activarse en producción con datos existentes

### 2. Nunca reejecutar migraciones base si las tablas ya existen

- Las migraciones históricas solo aplican a DB nuevas
- Si las tablas ya existen, las migraciones fallarán
- No asumir que las migraciones se pueden reejecutar

### 3. Las migraciones históricas solo aplican a DB nuevas

- Migraciones base: solo para creación inicial de esquema
- Migraciones incrementales: solo para cambios futuros en DB nuevas
- DB viva: requiere SQL manual controlado

### 4. Para DB viva, usar SQL manual idempotente

- Usar `ALTER TABLE IF NOT EXISTS` para columnas nuevas
- Ejecutar SQL directamente sobre la DB de producción (Render)
- Verificar estructura real antes de ejecutar cambios
- Documentar todos los cambios manuales aplicados

## Caso Real Documentado

### Errores 500 por Columnas Faltantes

**Columnas faltantes detectadas**:
- `works.post_closure_enabled_by_id`
- `users.phone`

**Error PostgreSQL**:
```
QueryFailedError: column X does not exist
SQLSTATE: 42703
```

**Causa**:
- Entities incluyen campos que no existen en la DB real
- TypeORM genera SELECTs con campos inexistentes
- Sin migraciones automáticas, el sistema no puede autocorregirse

### Error 42P07 por Recreación de Tablas

**Tabla afectada**:
- `roles`

**Error PostgreSQL**:
```
Error 42P07: relation "roles" already exists
```

**Causa**:
- `migrationsRun: true` intentó ejecutar migraciones base
- La tabla `roles` ya existía en la DB viva
- TypeORM intentó crear una tabla existente

## Resolución Aplicada

### Proceso Correcto

1. **Auditar estructura real de PostgreSQL**:
   - Conectar a la DB de Render vía cliente externo (DBeaver / psql)
   - Verificar columnas existentes vs. entities

2. **Ejecución manual de SQL en PostgreSQL de Render**:
   ```sql
   ALTER TABLE users ADD COLUMN IF NOT EXISTS phone VARCHAR(50);
   ALTER TABLE works ADD COLUMN IF NOT EXISTS post_closure_enabled_by_id UUID;
   ```

3. **Alineación DB ↔ entities**:
   - Estructura de DB alineada con entities
   - Entities reflejan la realidad de la DB

4. **Resultado**:
   - Backend vuelve a responder 200/201 sin errores 500
   - Sistema operativo y estable

## Justificación

- **Seguridad**: Previene pérdida de datos en DB viva
- **Precisión**: La estructura real es la fuente de verdad
- **Control**: SQL manual permite cambios controlados y verificables
- **Estabilidad**: Evita errores por migraciones reejecutadas

## Referencias

- `patterns/db-alignment-check.md` - Checklist para alineación de DB
- `constraints/infra-render.md` - Limitaciones de infraestructura en Render
- `failures/render-deploy-history.md` - Historial de fallas relacionadas

## Status

Decisión: CERRADA
Última actualización: Protocolo establecido después de incidente de desalineación
Vigencia: **Protocolo fijo para PMD**
