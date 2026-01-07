# Pulley Form and E2E Constraints

## Reglas Absolutas

Este documento define constraints obligatorios para formularios y validación E2E en sistemas gobernados por el IAS, derivados de fallos críticos documentados.

## Constraints de Formularios

### 1. noValidate obligatorio en todos los forms

**Regla**: Todo elemento `<form>` debe tener el atributo `noValidate`.

**Razón**: La validación HTML nativa intercepta el submit antes de que llegue al handler JavaScript, causando formularios "mudos" sin feedback.

**Implementación**:
```tsx
<form noValidate onSubmit={handleSubmit}>
  {/* campos */}
</form>
```

**Verificación**: E2E tests deben validar presencia de `noValidate`.

---

### 2. Validación SOLO en JavaScript

**Regla**: Toda validación de negocio debe implementarse en JavaScript, no en atributos HTML.

**Razón**: Control total sobre cuándo y cómo se validan los datos. Feedback explícito y visible.

**Prohibido**:
- `required` para validación de negocio
- `type="email"` para validación de formato
- `pattern` para validación de formato
- `min`/`max` para validación de rangos

**Permitido**:
- `type="email"` solo para UX (teclado móvil)
- Atributos HTML solo para accesibilidad, no para validación

**Implementación**:
```tsx
const validate = (data) => {
  const errors = {};
  if (!data.email || !isValidEmail(data.email)) {
    errors.email = 'Email inválido';
  }
  return errors;
};
```

---

### 3. type="number" prohibido para montos

**Regla**: No usar `type="number"` para campos de montos o valores monetarios.

**Razón**: 
- Problemas con decimales y separadores de miles
- Validación HTML nativa interfiere con formato personalizado
- Dificulta parseo de valores con formato (ej: "1.234,56")

**Alternativa**: Usar `type="text"` + parseo manual con función dedicada.

**Implementación**:
```tsx
<input
  type="text"
  value={formatNumber(value)}
  onChange={(e) => setValue(parseNumberAR(e.target.value))}
/>
```

---

### 4. parseNumberAR obligatorio

**Regla**: Todo parseo de números con formato argentino debe usar función `parseNumberAR`.

**Razón**: Consistencia en manejo de separadores de miles (.) y decimales (,).

**Implementación**:
```typescript
function parseNumberAR(value: string): number {
  // Remover separadores de miles (puntos)
  // Reemplazar coma decimal por punto
  // Parsear a número
  return parseFloat(value.replace(/\./g, '').replace(',', '.'));
}
```

**Verificación**: E2E tests deben validar que valores con formato se parsean correctamente.

---

### 5. Capas decorativas con pointer-events: none

**Regla**: Toda capa puramente decorativa (overlays, gradientes, efectos visuales) debe tener `pointer-events: none`.

**Razón**: Evitar que capas visuales intercepten eventos de mouse/touch destinados a elementos interactivos.

**Implementación**:
```css
.decorative-overlay {
  position: absolute;
  z-index: 10;
  pointer-events: none; /* Obligatorio */
}
```

**Excepción**: Si la capa es interactiva (ej: modal backdrop que cierra al click), no aplicar esta regla.

---

### 6. Modales deben estar montados antes de interacción

**Regla**: El form dentro de un modal debe estar montado en el DOM antes de cualquier intento de interacción.

**Razón**: Evitar race conditions donde E2E tests intentan interactuar con elementos que no existen aún.

**Implementación**:
```tsx
// ✅ CORRECTO: Form siempre montado
<Modal isOpen={isOpen}>
  <form data-testid="modal-form">
    {/* campos siempre en DOM */}
  </form>
</Modal>

// ❌ INCORRECTO: Form condicional
{isOpen && (
  <Modal>
    <form>{/* solo existe cuando isOpen */}</form>
  </Modal>
)}
```

**Alternativa**: Si el form debe ser condicional, usar `visibility: hidden` en lugar de renderizado condicional.

---

## Constraints de E2E

### 7. Playwright E2E obligatorio para flujos críticos

**Regla**: Todo flujo crítico de negocio debe tener tests E2E con Playwright.

**Razón**: 
- Validación end-to-end real del sistema completo
- Detección de problemas de integración
- Evidencia objetiva de que el feature funciona

**Flujos críticos incluyen**:
- Autenticación
- Creación/edición de entidades principales
- Flujos de pago o transacciones
- Cualquier flujo que afecte datos críticos

**Implementación**:
```typescript
test('debe crear movimiento correctamente', async ({ page }) => {
  await page.goto('/movements/new');
  await page.fill('[data-testid="amount"]', '1000,50');
  await page.click('[data-testid="submit"]');
  await expect(page.locator('[data-testid="success"]')).toBeVisible();
});
```

---

### 8. Ningún módulo se considera estable sin E2E PASS

**Regla**: Un módulo o feature no se considera estable ni completo hasta que todos sus tests E2E pasen.

**Razón**: 
- Build exitoso no implica funcionamiento correcto
- Tests unitarios no validan integración
- E2E es la única validación real del sistema completo

**Criterio de aceptación**:
- Todos los tests E2E del módulo pasan
- Tests cubren flujos happy path y error cases
- Tests son determinísticos (no flaky)

**Prohibido**:
- Marcar feature como "completo" sin E2E
- Deploy a producción sin E2E passing
- Considerar módulo estable solo con tests unitarios

---

## Verificación

Estos constraints deben verificarse en:
- Code review
- E2E test suite
- Pre-deploy checks

## Status

Constraint: ACTIVE
Aplicable a: Todos los proyectos gobernados por IAS
Versión: v1.0

