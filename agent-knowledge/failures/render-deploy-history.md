---
# Render Deploy – Historial de fallas reales

## Contexto
Proyecto: pmd-backend (NestJS + TypeORM)
Entorno: Render (PostgreSQL con SSL obligatorio)

## Fallas detectadas
1. Uso de Nest CLI (`nest build`, `npx`) en CI → falla en Render
2. Dependencia implícita de devDependencies → falla en producción
3. Código productivo con referencias a `jest` → falla `tsc`
4. Tipos Multer mal definidos → falla `tsc`
5. `migrationsRun: true` en producción → recreación de tablas
6. Proyecto sin DataSource standalone → no soporta migraciones CLI
7. Base de datos real desalineada con entidades (`users.phone` inexistente)

## Conclusión
El proyecto NO estaba preparado originalmente para:
- migraciones manuales vía CLI
- CI determinístico
- compilación estricta con `tsc` en producción

## [2026-01] Backend no arranca en Render – column users.phone does not exist

### Contexto
- Proyecto: pmd-backend (NestJS + TypeORM)
- Infraestructura: Render + PostgreSQL (SSL obligatorio)
- Estado:
  - Backend builda y arranca
  - Migraciones automáticas DESACTIVADAS
  - No existe DataSource CLI válido
  - DB real desalineada respecto al modelo

### Síntoma
- El backend arranca y registra rutas
- Falla en `AuthModule.onModuleInit`
- Error en Render:


QueryFailedError: column User.phone does not exist
SQLSTATE: 42703

- La falla ocurre durante `AuthService.ensureAdminUser`

### Causa raíz
- Desalineación de esquema (schema drift)
- La entidad `User` incluía el campo `phone`
- La tabla `users` en PostgreSQL NO tenía esa columna
- TypeORM genera SELECTs que incluyen el campo inexistente
- Al no usar migraciones, el sistema no puede autocorregirse

### Soluciones descartadas (NO volver a intentar)
- migration:run por CLI
- sync: true
- rebuilds forzados
- cambios defensivos en el modelo para ocultar el campo
- soluciones que ignoren la DB real

### Solución aplicada (correcta)
Corrección estructural manual en PostgreSQL:

```sql
ALTER TABLE users
ADD COLUMN phone VARCHAR(50);
```


Columna nullable

No destructiva

Compatible con la arquitectura vigente

Ejecutada vía cliente SQL externo (DBeaver)

SSL requerido para conexión

Resultado

El backend completa onModuleInit

ensureAdminUser deja de fallar

El sistema queda operativo

Backend estable en Render

Lección aprendida

En proyectos con:

migraciones desactivadas

sin DataSource CLI

y despliegue en Render

👉 cualquier cambio en entidades debe validarse contra el schema real de la DB
👉 una sola columna faltante puede bloquear todo el arranque

Regla operativa futura

Antes de deployar cambios en entidades:

auditar esquema real de PostgreSQL

documentar ALTERs manuales necesarios

nunca asumir sincronización automática
---
