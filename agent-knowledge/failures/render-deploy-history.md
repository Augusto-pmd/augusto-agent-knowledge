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
---
