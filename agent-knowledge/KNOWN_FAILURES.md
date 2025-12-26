# Errores Conocidos (Known Failures)

## Errores Documentados y Sus Causas

### 1. Importar Layouts en Páginas → Sidebar Duplicado
**Síntoma**: Aparecen múltiples sidebars o elementos de navegación duplicados en la página.

**Causa**: Importar componentes de layout directamente en páginas individuales en lugar de usar la jerarquía de layouts del App Router.

**Solución**: Usar `layout.tsx` en la jerarquía de rutas. Ver PATTERNS.md para el patrón correcto.

**Prevención**: Nunca importar componentes de layout en archivos `page.tsx`.

---

### 2. JSX con Múltiples Roots → Build Roto
**Síntoma**: Error de compilación: "Adjacent JSX elements must be wrapped in an enclosing tag" o build falla.

**Causa**: Retornar múltiples elementos JSX sin un contenedor padre (fragment o elemento wrapper).

**Solución**: Envolver múltiples elementos en un Fragment (`<>...</>`) o un elemento contenedor.

**Prevención**: Verificar que cada componente retorna un único elemento raíz.

---

### 3. request.json() + forward → Body Vacío
**Síntoma**: El body de la request llega vacío al endpoint destino cuando se usa `next/headers` forward.

**Causa**: Consumir el body con `request.json()` antes de hacer forward, dejando el stream vacío.

**Solución**: Usar `request.text()` y reconstruir el body, o usar un proxy API. Ver PATTERNS.md para el patrón de proxy.

**Prevención**: No consumir el body del request antes de hacer forward.

---

### 4. Auth en Pending Infinito
**Síntoma**: El estado de autenticación queda en "pending" indefinidamente, bloqueando la UI.

**Causa**: 
- Llamadas a APIs de autenticación que nunca resuelven
- Estados de carga no manejados correctamente
- Dependencias circulares en hooks de autenticación

**Solución**: Verificar timeouts, manejar estados de error, y asegurar que los hooks de auth siempre resuelven (éxito o error).

**Prevención**: Implementar timeouts y estados de error explícitos en la lógica de autenticación.

---

### 5. Permisos Inferidos en Frontend
**Síntoma**: Usuarios pueden acceder a funcionalidades que no deberían, o se bloquean incorrectamente.

**Causa**: Confiar en validaciones de permisos solo en el frontend sin verificación en el backend.

**Solución**: 
- Validar permisos siempre en el backend
- El frontend solo muestra/oculta UI, nunca autoriza acciones
- Implementar middleware de autorización en las APIs

**Prevención**: Nunca usar validaciones de permisos solo en el frontend. Siempre verificar en el servidor.

