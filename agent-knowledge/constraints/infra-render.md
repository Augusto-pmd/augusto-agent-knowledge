# Constraint: Infraestructura Render

## Contexto

El proyecto PMD backend se despliega en Render (plataforma de hosting) con PostgreSQL como base de datos.

## Limitaciones de Infraestructura

### Render No Ejecuta Migraciones Automáticamente

- Render no tiene capacidad nativa para ejecutar migraciones automáticamente
- El deploy del código y la gestión de la base de datos son responsabilidades separadas
- No hay integración automática entre deploy y migraciones

### Deploy y DB son Responsabilidades Separadas

- **Deploy**: Render despliega el código de la aplicación
- **DB**: PostgreSQL en Render es un servicio separado
- **Migraciones**: Deben ejecutarse manualmente o mediante scripts externos

### Acceso SQL en Render

**Plan Basic**:
- El acceso SQL se realiza vía conexión externa
- No hay interfaz SQL nativa en el dashboard de Render
- Requiere herramientas externas:
  - `psql` (cliente de línea de comandos)
  - Cliente gráfico (DBeaver, pgAdmin, etc.)

**Conexión**:
- SSL obligatorio para conexiones externas
- Credenciales disponibles en dashboard de Render
- Host, puerto, database, usuario y contraseña requeridos

### Implicaciones para el Proyecto

1. **Migraciones automáticas no aplican**:
   - `migrationsRun: true` no es viable en Render
   - Requiere ejecución manual de SQL

2. **Base de datos viva**:
   - DB puede tener datos y estructura existente
   - Cambios de esquema requieren SQL manual controlado
   - Ver: `decisions/db-live-protocol.md`

3. **Alineación manual**:
   - Entities y DB deben alinearse manualmente
   - SQL idempotente requerido para cambios
   - Ver: `patterns/db-alignment-check.md`

## Referencias

- `decisions/db-live-protocol.md` - Protocolo para bases de datos vivas
- `patterns/db-alignment-check.md` - Checklist para alineación de DB
- `failures/render-deploy-history.md` - Historial de fallas relacionadas

## Status

Constraint: ACTIVE
Última actualización: Limitaciones de infraestructura en Render documentadas
Vigencia: Permanente mientras el proyecto use Render
