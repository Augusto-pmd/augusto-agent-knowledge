---
# Reglas definitivas backend producción

1. El build de producción se ejecuta con:
   `tsc -p tsconfig.build.json`

2. NO se utiliza Nest CLI en CI ni en producción.

3. Migraciones automáticas DESACTIVADAS en producción.

4. Las migraciones solo se aplican:
   - si existe DataSource CLI válido
   - o manualmente sobre la base de datos.

5. PostgreSQL en Render requiere SSL obligatorio:
   `ssl: { rejectUnauthorized: false }`

6. El entrypoint de producción es:
   `node dist/src/main.js`
---
