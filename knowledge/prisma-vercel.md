# Prisma + Next.js App Router + Vercel

## Patrón Oficial (OBLIGATORIO)

En entornos serverless como Vercel, cada invocación de función puede crear múltiples instancias de Prisma Client si no se implementa correctamente. El patrón oficial utiliza `globalThis` como singleton para evitar la creación de múltiples conexiones a la base de datos.

### Implementación Correcta

```typescript
import { PrismaClient } from '@prisma/client';

const globalForPrisma = globalThis as unknown as {
  prisma: PrismaClient | undefined;
};

export const prisma =
  globalForPrisma.prisma ??
  new PrismaClient({
    log: process.env.NODE_ENV === 'development' ? ['query', 'error', 'warn'] : ['error'],
  });

if (process.env.NODE_ENV !== 'production') globalForPrisma.prisma = prisma;
```

### Explicación

- `globalThis` persiste entre invocaciones en el mismo proceso de Vercel
- En desarrollo, reutiliza la instancia para evitar múltiples conexiones
- En producción, Vercel puede reutilizar el proceso, por lo que el singleton previene conexiones duplicadas
- El check `??` asegura que solo se crea una instancia por proceso

## Síntomas de mal uso

- **500 genéricos en producción**: Múltiples instancias de Prisma Client intentan conectarse simultáneamente, excediendo el pool de conexiones
- **Funciona en local, falla en Vercel**: El entorno local no replica el comportamiento serverless; múltiples instancias pasan desapercibidas
- **Runner pasa, producción no**: Los tests unitarios no ejercitan el patrón de invocación serverless; la producción sí

## Antipatrones

### Lazy init manual

```typescript
// ❌ INCORRECTO
let prisma: PrismaClient;

export function getPrisma() {
  if (!prisma) {
    prisma = new PrismaClient();
  }
  return prisma;
}
```

**Problema**: No persiste entre invocaciones en serverless. Cada función puede crear su propia instancia.

### Instanciar Prisma en import-time

```typescript
// ❌ INCORRECTO
export const prisma = new PrismaClient();
```

**Problema**: En serverless, cada módulo puede importarse en contextos aislados, creando múltiples instancias.

### Múltiples PrismaClient por request

```typescript
// ❌ INCORRECTO
// En diferentes archivos
export const db = new PrismaClient();
export const client = new PrismaClient();
```

**Problema**: Cada import crea una nueva conexión, saturando el pool de conexiones de la base de datos.

## Checklist antes de deploy

- [ ] Patrón oficial aplicado (singleton con `globalThis`)
- [ ] Envs disponibles en runtime (`DATABASE_URL` verificada en Vercel dashboard)
- [ ] Migraciones aplicadas (`prisma migrate deploy` ejecutado o automático)
- [ ] Pool de conexiones configurado apropiadamente (`connection_limit` en `DATABASE_URL` si es necesario)
- [ ] Logs de Prisma deshabilitados en producción (solo `error` level)
- [ ] Verificación de conexión en health check endpoint

