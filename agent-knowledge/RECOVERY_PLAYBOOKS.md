# Procedimientos de Recuperación (Recovery Playbooks)

## Playbooks para Situaciones Críticas

### 1. Build Recovery en Next.js
**Situación**: El build de Next.js falla y no se puede desplegar.

**Procedimiento**:

1. **Identificar el error**:
   ```bash
   npm run build
   # O
   next build
   ```

2. **Errores comunes y soluciones**:
   - **JSX múltiples roots**: Verificar que cada componente retorna un único elemento
   - **Imports faltantes**: Verificar que todas las dependencias están instaladas
   - **Type errors**: Ejecutar `npm run type-check` si está disponible

3. **Limpiar y reconstruir**:
   ```bash
   # Limpiar cache y node_modules
   rm -rf .next node_modules
   npm install
   npm run build
   ```

4. **Verificar en modo desarrollo**:
   ```bash
   npm run dev
   # Revisar errores en consola del navegador y terminal
   ```

5. **Si persiste**: Revisar KNOWN_FAILURES.md para errores conocidos.

---

### 2. Vercel Cache Issues
**Situación**: Los cambios no se reflejan en producción después del deploy.

**Procedimiento**:

1. **Verificar que el deploy se completó**:
   - Revisar el dashboard de Vercel
   - Confirmar que el commit correcto está desplegado

2. **Limpiar cache de build**:
   ```bash
   vercel --force
   # O en producción
   vercel --prod --force
   ```

3. **Limpiar cache en dashboard**:
   - Ir a Settings → Data Cache
   - Limpiar cache manualmente si está disponible

4. **Verificar headers de cache**:
   - Revisar `next.config.js` para configuraciones de cache
   - Agregar headers `Cache-Control` en rutas críticas si es necesario

5. **Validar en modo incógnito**:
   - Abrir la aplicación en ventana incógnita
   - Verificar que los cambios se reflejan

6. **Si persiste**: Revisar PATTERNS.md para el patrón de "Deploy en Bloque Único".

---

### 3. JSX Root Errors
**Situación**: Error de compilación por múltiples elementos JSX sin contenedor.

**Procedimiento**:

1. **Identificar el componente problemático**:
   - El error de build indicará el archivo y línea
   - Buscar componentes que retornan múltiples elementos

2. **Solución inmediata**:
   ```tsx
   // ❌ Incorrecto
   return (
     <div>Element 1</div>
     <div>Element 2</div>
   );
   
   // ✅ Correcto
   return (
     <>
       <div>Element 1</div>
       <div>Element 2</div>
     </>
   );
   ```

3. **Verificar todos los componentes**:
   - Buscar patrones de múltiples returns
   - Asegurar que cada componente tiene un único root

4. **Prevenir**: Revisar KNOWN_FAILURES.md antes de escribir JSX complejo.

---

### 4. Auth State Recovery
**Situación**: El estado de autenticación queda bloqueado o inconsistente.

**Procedimiento**:

1. **Identificar el síntoma**:
   - ¿Estado en "pending" indefinidamente?
   - ¿Usuario autenticado pero sin permisos?
   - ¿Sesión perdida inesperadamente?

2. **Para estado "pending"**:
   - Verificar timeouts en llamadas a API de auth
   - Revisar que los hooks de auth manejan errores
   - Agregar logging para identificar dónde se bloquea

3. **Para sesión perdida**:
   - Verificar expiración de tokens
   - Revisar configuración de cookies/session storage
   - Confirmar que el refresh token funciona

4. **Recuperación manual**:
   ```typescript
   // Limpiar estado de auth
   localStorage.clear(); // O sessionStorage
   // Recargar página
   window.location.reload();
   ```

5. **Prevenir**: Implementar timeouts y estados de error explícitos. Ver KNOWN_FAILURES.md.

---

## Principios de Recuperación

1. **No entrar en pánico**: Seguir el playbook paso a paso
2. **Documentar la solución**: Si encuentras una solución nueva, actualizar este documento
3. **Prevenir recurrencia**: Después de resolver, actualizar KNOWN_FAILURES.md si es un nuevo error conocido

