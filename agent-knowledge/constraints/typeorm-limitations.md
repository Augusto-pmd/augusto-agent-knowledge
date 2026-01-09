---
# Limitaciones reales detectadas

- El proyecto usa `TypeOrmModule.forRoot` pero NO exporta un DataSource standalone.
- Por diseño, NO permite ejecutar `typeorm migration:run` en producción.
- Cualquier cambio de esquema requiere:
  - migraciones automáticas activas
  - o ejecución manual de `ALTER TABLE` en la base de datos.

Esto NO es un bug, es una limitación estructural del proyecto.
---
