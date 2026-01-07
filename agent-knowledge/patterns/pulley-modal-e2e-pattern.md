# Pulley Modal E2E Pattern

## Purpose

Define el patrón obligatorio para modales con formularios y su testing E2E, derivado de fallos críticos de timing y DOM.

## Problem

Modales que renderizan formularios condicionalmente causan:
- Race conditions en E2E tests
- Elementos no encontrados
- Timing issues (funciona a veces, falla otras)
- Submit que no se ejecuta porque el form no existe

## Pattern

### Montaje Inmediato del Form

**Regla**: El form dentro del modal debe estar montado en el DOM antes de cualquier interacción.

```tsx
// ✅ CORRECTO: Form siempre montado
<Modal isOpen={isOpen}>
  <form 
    data-testid="modal-form"
    noValidate
    onSubmit={handleSubmit}
    style={{ display: isOpen ? 'block' : 'none' }}
    // O usar visibility: hidden en lugar de display: none
  >
    {/* Campos siempre en DOM */}
  </form>
</Modal>

// ❌ INCORRECTO: Form condicional
{isOpen && (
  <Modal>
    <form data-testid="modal-form">
      {/* Solo existe cuando isOpen es true */}
    </form>
  </Modal>
)}
```

**Alternativa con visibility**:

```tsx
<Modal>
  <form
    data-testid="modal-form"
    style={{ 
      visibility: isOpen ? 'visible' : 'hidden',
      position: isOpen ? 'static' : 'absolute'
    }}
  >
    {/* Campos siempre en DOM */}
  </form>
</Modal>
```

### data-testid Obligatorio

**Regla**: Todos los elementos interactivos del modal deben tener `data-testid`.

```tsx
<Modal>
  <form data-testid="modal-form">
    <input 
      data-testid="modal-field-amount"
      name="amount"
    />
    <button 
      type="submit"
      data-testid="modal-submit"
    >
      Guardar
    </button>
    <button 
      type="button"
      onClick={onClose}
      data-testid="modal-close"
    >
      Cancelar
    </button>
  </form>
</Modal>
```

**Razón**: Permite esperas explícitas y selectores confiables en E2E.

## E2E Testing Pattern

### Esperas Correctas

**Regla**: Esperar explícitamente a que elementos estén disponibles antes de interactuar.

```typescript
test('debe abrir modal y completar formulario', async ({ page }) => {
  // 1. Abrir modal
  await page.click('[data-testid="open-modal-button"]');
  
  // 2. Esperar a que el form esté visible (no solo en DOM)
  await page.waitForSelector('[data-testid="modal-form"]', { 
    state: 'visible' 
  });
  
  // 3. Verificar que campos están disponibles
  await expect(page.locator('[data-testid="modal-field-amount"]')).toBeVisible();
  
  // 4. Interactuar
  await page.fill('[data-testid="modal-field-amount"]', '1000,50');
  
  // 5. Submit
  await page.click('[data-testid="modal-submit"]');
  
  // 6. Verificar resultado
  await expect(page.locator('[data-testid="success-message"]')).toBeVisible();
});
```

### Evitar Timing Falso

**❌ INCORRECTO**: Esperas arbitrarias

```typescript
// ❌ MAL: Espera fija que puede fallar
await page.waitForTimeout(1000);
await page.click('[data-testid="modal-submit"]');
```

**✅ CORRECTO**: Esperas basadas en estado del DOM

```typescript
// ✅ BIEN: Espera a que el elemento esté listo
await page.waitForSelector('[data-testid="modal-submit"]', { 
  state: 'visible',
  enabled: true 
});
await page.click('[data-testid="modal-submit"]');
```

### Patrón Completo de Test

```typescript
test('flujo completo de modal con form', async ({ page }) => {
  // Setup: ir a página
  await page.goto('/movements');
  
  // 1. Abrir modal
  await page.click('[data-testid="new-movement-button"]');
  
  // 2. Esperar modal montado y visible
  const modalForm = page.locator('[data-testid="modal-form"]');
  await expect(modalForm).toBeVisible();
  
  // 3. Verificar que campos están interactuables
  const amountField = page.locator('[data-testid="modal-field-amount"]');
  await expect(amountField).toBeEnabled();
  
  // 4. Completar formulario
  await amountField.fill('1000,50');
  
  // 5. Submit (esperar que botón esté habilitado)
  const submitButton = page.locator('[data-testid="modal-submit"]');
  await expect(submitButton).toBeEnabled();
  await submitButton.click();
  
  // 6. Verificar éxito (modal puede cerrarse o mostrar mensaje)
  await expect(page.locator('[data-testid="success-message"]')).toBeVisible();
  
  // 7. Verificar que el modal se cerró o el form se reseteó
  await expect(modalForm).not.toBeVisible();
});
```

## Common Pitfalls

### 1. Interactuar antes de que el form esté listo

```typescript
// ❌ INCORRECTO
await page.click('[data-testid="open-modal"]');
await page.fill('[data-testid="field"]', 'value'); // Puede fallar si form no está listo

// ✅ CORRECTO
await page.click('[data-testid="open-modal"]');
await page.waitForSelector('[data-testid="field"]', { state: 'visible' });
await page.fill('[data-testid="field"]', 'value');
```

### 2. No verificar que elementos están habilitados

```typescript
// ❌ INCORRECTO
await page.click('[data-testid="submit"]'); // Puede estar disabled

// ✅ CORRECTO
const submit = page.locator('[data-testid="submit"]');
await expect(submit).toBeEnabled();
await submit.click();
```

### 3. Asumir que el modal se cerró

```typescript
// ❌ INCORRECTO
await page.click('[data-testid="submit"]');
// Asume que el modal se cerró

// ✅ CORRECTO
await page.click('[data-testid="submit"]');
await expect(page.locator('[data-testid="modal-form"]')).not.toBeVisible();
// O verificar que aparece mensaje de éxito
```

## Checklist

Antes de considerar un modal completo:

- [ ] Form está montado en DOM antes de interacción
- [ ] `data-testid` en todos los elementos interactivos
- [ ] E2E test espera explícitamente a que elementos estén visibles
- [ ] E2E test verifica que elementos están habilitados antes de interactuar
- [ ] No hay `waitForTimeout` arbitrarios
- [ ] E2E test valida el flujo completo (abrir → completar → submit → verificar)

## Status

Pattern: ACTIVE
Version: v1.0
Aplicable a: Todos los modales con formularios en proyectos IAS

