# Reglas Fundamentales (Core Rules)

## Principios Operativos

### 1. Build Verde Antes de Features
**Regla**: Nunca agregar nuevas funcionalidades si el build actual está roto.

- Verificar que el build pasa antes de cualquier cambio
- Resolver errores de compilación antes de agregar código nuevo
- Mantener el estado del proyecto siempre deployable

### 2. Un Problema Activo a la Vez
**Regla**: Enfocarse en resolver un único problema antes de pasar al siguiente.

- Evitar cambios simultáneos en múltiples áreas
- Completar y verificar una solución antes de iniciar otra
- Mantener el contexto claro y manejable

### 3. Flujo de Trabajo: Prompt → Terminal → Deploy → Verificación
**Regla**: Seguir un flujo ordenado y verificable.

1. **Prompt**: Entender el requerimiento completamente
2. **Terminal**: Implementar y probar localmente
3. **Deploy**: Desplegar en entorno de desarrollo/staging
4. **Verificación**: Confirmar que funciona en el entorno desplegado

### 4. Layouts Solo en layout.tsx (App Router)
**Regla**: En Next.js App Router, los layouts deben estar únicamente en archivos `layout.tsx`.

- No importar componentes de layout en páginas individuales
- Usar la jerarquía de layouts del App Router
- Evitar duplicación de sidebars o headers

### 5. Deploy Válido Solo en Entorno del Owner
**Regla**: Los deploys finales y validaciones de producción solo se realizan en el entorno del propietario del proyecto.

- No realizar deploys a producción sin autorización explícita
- Validar en entornos de desarrollo/staging primero
- Respetar los permisos y ownership del proyecto

### 6. Corrección Respetuosa Basada en Experiencia
**Regla**: Al corregir código o sugerir cambios, hacerlo de forma respetuosa y fundamentada.

- Explicar el "por qué" basado en experiencias documentadas
- Referenciar KNOWN_FAILURES.md o PATTERNS.md cuando aplique
- Mantener un tono profesional y constructivo

