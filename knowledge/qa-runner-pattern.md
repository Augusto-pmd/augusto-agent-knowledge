# QA Runner E2E como Gate de Deploy

## Principio

Un runner E2E que valida el sistema completo contra producción es superior a validar solo UI o tests unitarios porque:

1. **Ejercita el stack completo**: Backend, base de datos, red, frontend y su interacción real
2. **Detecta problemas de integración**: Errores que solo aparecen cuando todos los componentes trabajan juntos
3. **Valida configuración real**: Variables de entorno, URLs, permisos y configuraciones de producción
4. **Evita falsos positivos**: Los tests unitarios pueden pasar mientras el sistema real falla
5. **Proporciona evidencia objetiva**: Resultados binarios (pass/fail) sin interpretación subjetiva

## Estructura recomendada

### Node + fetch

```typescript
// qa-runner.ts
import fetch from 'node-fetch';

interface TestResult {
  name: string;
  passed: boolean;
  error?: string;
  duration: number;
}

const BASE_URL = process.env.PRODUCTION_URL || 'https://app.example.com';

async function runTest(name: string, testFn: () => Promise<void>): Promise<TestResult> {
  const start = Date.now();
  try {
    await testFn();
    return {
      name,
      passed: true,
      duration: Date.now() - start,
    };
  } catch (error) {
    return {
      name,
      passed: false,
      error: error instanceof Error ? error.message : String(error),
      duration: Date.now() - start,
    };
  }
}

async function testLogin() {
  const response = await fetch(`${BASE_URL}/api/auth/login`, {
    method: 'POST',
    headers: { 'Content-Type': 'application/json' },
    body: JSON.stringify({
      email: process.env.TEST_EMAIL,
      password: process.env.TEST_PASSWORD,
    }),
  });

  if (!response.ok) {
    throw new Error(`Login failed: ${response.status} ${response.statusText}`);
  }

  const data = await response.json();
  if (!data.accessToken) {
    throw new Error('Missing accessToken in response');
  }
}

async function main() {
  const results: TestResult[] = [];

  results.push(await runTest('Login flow', testLogin));
  // Agregar más tests...

  const passed = results.filter(r => r.passed).length;
  const total = results.length;

  console.log(`\nResults: ${passed}/${total} tests passed\n`);

  results.forEach(result => {
    const status = result.passed ? '✓' : '✗';
    console.log(`${status} ${result.name} (${result.duration}ms)`);
    if (result.error) {
      console.log(`  Error: ${result.error}`);
    }
  });

  // Reporte JSON
  const report = {
    timestamp: new Date().toISOString(),
    url: BASE_URL,
    summary: { passed, total, failed: total - passed },
    results,
  };

  console.log('\nJSON Report:');
  console.log(JSON.stringify(report, null, 2));

  process.exit(passed === total ? 0 : 1);
}

main().catch(console.error);
```

### Asserts claros

Cada test debe tener aserciones explícitas que validen:
- Status codes HTTP esperados
- Estructura de respuestas (presencia de campos críticos)
- Valores esperados en datos de negocio
- Tiempos de respuesta razonables

### Reporte JSON

El reporte JSON permite:
- Integración con CI/CD pipelines
- Análisis histórico de resultados
- Alertas automatizadas
- Dashboard de salud del sistema

## Regla obligatoria

**Nunca deployar si el runner falla.**

Esta regla debe estar codificada en el pipeline de CI/CD:

```yaml
# .github/workflows/deploy.yml
- name: Run QA Runner
  run: npm run qa:runner
  env:
    PRODUCTION_URL: ${{ secrets.PRODUCTION_URL }}
    TEST_EMAIL: ${{ secrets.TEST_EMAIL }}
    TEST_PASSWORD: ${{ secrets.TEST_PASSWORD }}

- name: Deploy
  if: success()  # Solo si el runner pasó
  run: vercel --prod
```

## Beneficios

- **Evita regresiones**: Detecta problemas antes de que lleguen a usuarios
- **Alinea backend y frontend**: Valida que ambos trabajan correctamente juntos
- **Da evidencia objetiva**: Resultados medibles y reproducibles
- **Reduce tiempo de debugging**: Identifica exactamente qué componente falla
- **Confianza en deploys**: Permite deploys automáticos con seguridad

