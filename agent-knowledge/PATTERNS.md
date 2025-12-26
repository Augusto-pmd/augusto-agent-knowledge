# Patrones de Diseño y Soluciones Probadas

## Patrones Documentados

### 1. Sidebar Único en Layout Autenticado
**Contexto**: Aplicaciones con autenticación que requieren un sidebar de navegación.

**Patrón**:
```tsx
// app/(authenticated)/layout.tsx
export default function AuthenticatedLayout({
  children,
}: {
  children: React.ReactNode;
}) {
  return (
    <div className="flex">
      <Sidebar />
      <main className="flex-1">{children}</main>
    </div>
  );
}
```

**Reglas**:
- El sidebar se define UNA VEZ en el layout de la ruta autenticada
- Las páginas hijas (`page.tsx`) NO importan el Sidebar
- Usar grupos de rutas `(authenticated)` para separar rutas con y sin sidebar

**Beneficios**: Evita duplicación, mantiene consistencia, facilita mantenimiento.

---

### 2. Proxy API con request.text()
**Contexto**: Necesidad de hacer forward de requests con body a APIs externas.

**Problema**: `request.json()` consume el stream, dejando el body vacío para el forward.

**Patrón**:
```typescript
// app/api/proxy/route.ts
import { NextRequest, NextResponse } from 'next/server';

export async function POST(request: NextRequest) {
  // Leer el body como texto para preservarlo
  const bodyText = await request.text();
  
  // Forward a la API externa
  const response = await fetch('https://external-api.com/endpoint', {
    method: 'POST',
    headers: {
      'Content-Type': 'application/json',
    },
    body: bodyText,
  });
  
  return NextResponse.json(await response.json());
}
```

**Alternativa**: Si necesitas parsear y validar antes de forward, reconstruir el body:
```typescript
const body = await request.json();
// Validar/transformar body
const response = await fetch(url, {
  body: JSON.stringify(body),
  // ...
});
```

**Beneficios**: Preserva el body completo, permite validación intermedia si es necesario.

---

### 3. Mappers DTO-Exactos por Entidad
**Contexto**: Transformación entre modelos de dominio y DTOs de API.

**Patrón**:
```typescript
// Un mapper por entidad, con funciones bidireccionales
export const UserMapper = {
  toDTO: (user: User): UserDTO => ({
    id: user.id,
    email: user.email,
    name: user.name,
    // Mapeo explícito, sin asumir estructura
  }),
  
  fromDTO: (dto: UserDTO): User => ({
    id: dto.id,
    email: dto.email,
    name: dto.name,
    // Transformaciones necesarias
  }),
};
```

**Reglas**:
- Un mapper por entidad de dominio
- Funciones explícitas `toDTO` y `fromDTO`
- No asumir que las estructuras son idénticas
- Documentar transformaciones no triviales

**Beneficios**: Separación clara de responsabilidades, fácil mantenimiento, evita acoplamiento.

---

### 4. Deploy en Bloque Único con Clear Cache
**Contexto**: Deploys en Vercel o plataformas similares que usan cache agresivo.

**Problema**: Cambios no se reflejan debido a cache de build o de runtime.

**Patrón**:
1. Realizar todos los cambios relacionados en un único commit
2. Deploy en bloque (no incremental)
3. Limpiar cache después del deploy:
   - Cache de build: `vercel --force` o limpiar en dashboard
   - Cache de runtime: Headers `Cache-Control: no-cache` en rutas críticas
4. Verificar en modo incógnito o con headers de no-cache

**Comando típico**:
```bash
# Deploy forzado (limpia cache)
vercel --force

# O en producción
vercel --prod --force
```

**Beneficios**: Evita estados inconsistentes, asegura que los cambios se reflejen correctamente.

---

## Principios de Aplicación de Patrones

- **Usar patrones probados**: Preferir soluciones documentadas sobre experimentación
- **Documentar variaciones**: Si un patrón se adapta, documentar la variación
- **Validar en producción**: Los patrones deben estar validados en entornos reales

