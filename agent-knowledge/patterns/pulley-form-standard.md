# Pulley Form Standard Pattern

## Purpose

Define la estructura estándar obligatoria para todos los formularios en sistemas gobernados por IAS, derivada de fallos críticos documentados.

## Structure

### Estructura Base

```tsx
<form 
  noValidate 
  onSubmit={handleSubmit}
  data-testid="form-name"
>
  {/* Campos del formulario */}
  
  {/* Mensajes de error visibles */}
  {errors.general && (
    <div role="alert" data-testid="error-general">
      {errors.general}
    </div>
  )}
  
  {/* Botón de submit */}
  <button 
    type="submit" 
    disabled={isSubmitting}
    data-testid="submit-button"
  >
    {isSubmitting ? 'Enviando...' : 'Enviar'}
  </button>
</form>
```

## Components

### 1. noValidate + onSubmit

**Obligatorio**: Todo form debe tener `noValidate` y manejar submit con `onSubmit`.

```tsx
<form noValidate onSubmit={handleSubmit}>
```

**Razón**: Evitar que validación HTML nativa intercepte el submit antes del handler JavaScript.

### 2. Validación JS

**Implementación**:

```tsx
const [errors, setErrors] = useState<Record<string, string>>({});
const [isSubmitting, setIsSubmitting] = useState(false);

const validate = (data: FormData): Record<string, string> => {
  const errors: Record<string, string> = {};
  
  if (!data.email || !isValidEmail(data.email)) {
    errors.email = 'Email inválido';
  }
  
  if (!data.amount || parseNumberAR(data.amount) <= 0) {
    errors.amount = 'Monto debe ser mayor a cero';
  }
  
  return errors;
};

const handleSubmit = async (e: React.FormEvent<HTMLFormElement>) => {
  e.preventDefault();
  
  // 1. Validar
  const formData = new FormData(e.currentTarget);
  const data = Object.fromEntries(formData);
  const validationErrors = validate(data);
  
  if (Object.keys(validationErrors).length > 0) {
    setErrors(validationErrors);
    return;
  }
  
  // 2. Limpiar errores previos
  setErrors({});
  setIsSubmitting(true);
  
  try {
    // 3. Ejecutar submit
    await submitData(data);
    
    // 4. Manejar éxito
    onSuccess?.();
  } catch (error) {
    // 5. Manejar error
    setErrors({
      general: error instanceof Error ? error.message : 'Error al enviar',
    });
  } finally {
    setIsSubmitting(false);
  }
};
```

### 3. Manejo de errores visibles

**Obligatorio**: Todos los errores deben mostrarse de forma visible.

```tsx
{/* Error general */}
{errors.general && (
  <div 
    role="alert" 
    className="error-message"
    data-testid="error-general"
  >
    {errors.general}
  </div>
)}

{/* Errores por campo */}
{errors.email && (
  <div 
    role="alert" 
    className="field-error"
    data-testid="error-email"
  >
    {errors.email}
  </div>
)}
```

**Requisitos**:
- `role="alert"` para accesibilidad
- `data-testid` para E2E tests
- Estilos visibles (color, contraste)
- Posicionamiento cerca del campo relacionado

### 4. Submit siempre ejecuta handler

**Garantía**: El handler `onSubmit` siempre se ejecuta cuando el usuario hace submit.

**Verificación**:
```tsx
const handleSubmit = async (e: React.FormEvent<HTMLFormElement>) => {
  e.preventDefault();
  console.log('Submit handler ejecutado'); // Debe aparecer siempre
  
  // ... resto de lógica
};
```

**Si el handler no se ejecuta**:
- Verificar que `noValidate` está presente
- Verificar que no hay validación HTML bloqueando
- Verificar que el botón tiene `type="submit"`

## Field Patterns

### Campo de texto estándar

```tsx
<div>
  <label htmlFor="email">Email</label>
  <input
    id="email"
    name="email"
    type="text" // No usar type="email" para validación
    data-testid="field-email"
  />
  {errors.email && (
    <div role="alert" data-testid="error-email">
      {errors.email}
    </div>
  )}
</div>
```

### Campo de monto (número con formato)

```tsx
<div>
  <label htmlFor="amount">Monto</label>
  <input
    id="amount"
    name="amount"
    type="text" // Nunca type="number"
    value={formatNumberAR(value)}
    onChange={(e) => {
      const parsed = parseNumberAR(e.target.value);
      setValue(parsed);
    }}
    data-testid="field-amount"
  />
  {errors.amount && (
    <div role="alert" data-testid="error-amount">
      {errors.amount}
    </div>
  )}
</div>
```

## E2E Testing

### Test estándar de formulario

```typescript
test('debe validar y enviar formulario', async ({ page }) => {
  await page.goto('/form-page');
  
  // 1. Intentar submit sin datos (debe mostrar errores)
  await page.click('[data-testid="submit-button"]');
  await expect(page.locator('[data-testid="error-email"]')).toBeVisible();
  
  // 2. Completar datos válidos
  await page.fill('[data-testid="field-email"]', 'test@example.com');
  await page.fill('[data-testid="field-amount"]', '1000,50');
  
  // 3. Submit exitoso
  await page.click('[data-testid="submit-button"]');
  await expect(page.locator('[data-testid="success"]')).toBeVisible();
  await expect(page.locator('[data-testid="error-general"]')).not.toBeVisible();
});
```

## Checklist

Antes de considerar un formulario completo:

- [ ] `noValidate` presente en `<form>`
- [ ] Handler `onSubmit` implementado
- [ ] Validación JavaScript implementada
- [ ] Errores se muestran visiblemente
- [ ] `data-testid` en elementos críticos
- [ ] Estados de loading/error/success manejados
- [ ] E2E test pasa
- [ ] Accesibilidad: `role="alert"` en errores, labels asociados

## Status

Pattern: ACTIVE
Version: v1.0
Aplicable a: Todos los formularios en proyectos IAS

