# Antipatrones Comunes en Sistemas Full-Stack

## Asumir enums sin migración

### El Problema

Los enums en bases de datos relacionales (PostgreSQL, MySQL) son tipos de datos especiales que requieren migración explícita. Asumir que un enum existe o que sus valores están disponibles sin verificar la migración causa errores en producción.

### Por qué siempre fallan en producción

1. **Entornos limpios**: Producción puede tener un schema más antiguo que desarrollo
2. **Migraciones no aplicadas**: El código asume el enum, pero la migración nunca se ejecutó
3. **Valores faltantes**: El código usa valores del enum que no fueron agregados en la migración
4. **Orden de ejecución**: Las migraciones pueden fallar si dependen de datos existentes

### Solución

```typescript
// ❌ INCORRECTO: Asumir que el enum existe
const status: 'pending' | 'approved' | 'rejected' = 'pending';
await prisma.order.create({ data: { status } });

// ✅ CORRECTO: Verificar migración y usar tipos generados
import { OrderStatus } from '@prisma/client';
const status: OrderStatus = OrderStatus.PENDING;
await prisma.order.create({ data: { status } });
```

**Checklist**:
- [ ] Migración creada y aplicada en todos los entornos
- [ ] Tipos TypeScript generados desde Prisma incluyen el enum
- [ ] Valores del enum validados en runtime si es crítico
- [ ] Rollback plan si la migración falla

## Probar en entornos distintos

### El Problema

Ejecutar el runner E2E contra una URL y probar manualmente la UI contra otra URL diferente genera inconsistencias y falsos positivos.

### Por qué runner y UI deben apuntar a la misma URL

1. **Configuración divergente**: Cada entorno puede tener variables de entorno, bases de datos o configuraciones diferentes
2. **Estado inconsistente**: Los datos en un entorno no reflejan el estado del otro
3. **Falsos positivos/negativos**: El runner puede pasar mientras la UI falla, o viceversa
4. **Debugging imposible**: No se puede reproducir el problema porque los entornos difieren

### Solución

```bash
# ❌ INCORRECTO
QA_RUNNER_URL=https://staging.example.com
# Usuario prueba manualmente en https://production.example.com

# ✅ CORRECTO
QA_RUNNER_URL=https://staging.example.com
# Usuario prueba manualmente en https://staging.example.com
# O mejor aún: el runner automatiza todo
```

**Regla**: El runner E2E debe validar exactamente el mismo entorno que se desplegará a producción, o el mismo que el usuario está probando manualmente.

## Confiar en build exitoso

### El Problema

Un build exitoso (`npm run build`, `tsc --noEmit`) solo valida que el código compila correctamente. No valida que el código funciona en runtime.

### Por qué compilar no implica funcionar

1. **Errores de runtime**: Referencias a variables de entorno faltantes, conexiones a servicios externos, validaciones de datos
2. **Problemas de configuración**: Build pasa, pero las configuraciones de producción son incorrectas
3. **Dependencias faltantes**: El código compila, pero las dependencias no están instaladas en producción
4. **Problemas de tipos vs valores**: TypeScript valida tipos, no valores reales en runtime

### Solución

```bash
# ❌ INCORRECTO: Solo build
npm run build
vercel --prod

# ✅ CORRECTO: Build + Tests + Runner
npm run build
npm run test
npm run qa:runner
vercel --prod
```

**Checklist post-build**:
- [ ] Tests unitarios pasan
- [ ] Tests de integración pasan
- [ ] Runner E2E pasa contra staging/producción
- [ ] Health checks responden correctamente
- [ ] Variables de entorno verificadas

## Usar mocks para validar producción

### Cuándo sirven los mocks

Los mocks son útiles para:
- **Tests unitarios**: Aislar unidades de código sin dependencias externas
- **Desarrollo local**: Simular servicios externos no disponibles localmente
- **Tests de integración controlados**: Validar lógica de negocio con datos predecibles
- **Performance testing**: Simular carga sin afectar sistemas reales

### Cuándo NO sirven para validar producción

Los mocks NO deben usarse para:
- **Validar configuración real**: Mocks no ejercitan variables de entorno, URLs, permisos reales
- **Detectar regresiones de integración**: Mocks no reflejan cambios en APIs externas o servicios
- **Validar contratos**: Mocks pueden estar desactualizados respecto a la implementación real
- **Gate de deploy**: Mocks pasan mientras producción falla

### Solución

```typescript
// ❌ INCORRECTO: Mock en runner de producción
jest.mock('./api', () => ({
  login: jest.fn().mockResolvedValue({ accessToken: 'fake' }),
}));

// ✅ CORRECTO: Runner real contra producción
const response = await fetch(`${PRODUCTION_URL}/api/auth/login`, {
  method: 'POST',
  body: JSON.stringify({ email, password }),
});
```

**Regla**: Los mocks son para desarrollo y tests unitarios. La validación de producción requiere llamadas reales a servicios reales.

