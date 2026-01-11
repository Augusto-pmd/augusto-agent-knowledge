# Recovery: Required Status Checks Cannot Be Empty

## Error

```
Required status checks cannot be empty
```

Este error aparece al intentar configurar un ruleset de GitHub que requiere status checks, pero GitHub no encuentra ningún check registrado para el branch target (típicamente `main`).

## Causa Raíz

**Los status checks no están registrados en el branch target.**

GitHub solo puede exigir status checks que ya se hayan ejecutado al menos una vez en el branch target. Si el workflow de CI nunca se ejecutó en `main`, GitHub no tiene checks registrados para ese branch.

### Por Qué Ocurre

1. El workflow de CI solo se ejecuta en Pull Requests, no en push a `main`
2. El workflow nunca se ejecutó en `main`, por lo que GitHub no tiene checks registrados
3. Al intentar crear un ruleset que exige checks, GitHub no encuentra checks disponibles

## Solución Validada

**Ejecutar el CI en push a `main` para registrar los checks.**

### Pasos Exactos para Resolverlo

1. **Verificar el workflow de CI**:
   - Abrir `.github/workflows/ci.yml`
   - Verificar que el bloque `on:` incluye `push` a `main`

2. **Si falta el trigger de push**:
   ```yaml
   on:
     pull_request:
       branches: ["main"]
     push:
       branches: ["main"]
   ```

3. **Hacer commit y push del cambio**:
   - Crear una rama (ej: `fix/add-push-trigger`)
   - Hacer commit del cambio
   - Crear Pull Request
   - Esperar que el CI pase
   - Hacer merge a `main`

4. **Forzar ejecución en main**:
   - Después del merge, el workflow se ejecutará automáticamente en `main`
   - Alternativamente, hacer un commit vacío en `main` para forzar ejecución:
     ```bash
     git commit --allow-empty -m "chore: trigger CI on main"
     git push origin main
     ```

5. **Verificar que el check se registró**:
   - Ir a Settings → Rules → Rulesets
   - Intentar crear/editar el ruleset
   - El check `CI / validate` (o el nombre del job) debería aparecer en la lista

6. **Configurar el ruleset**:
   - Seleccionar el check `CI / validate`
   - Guardar el ruleset
   - El error "Required status checks cannot be empty" ya no debería aparecer

## Verificación

Para verificar que los checks están registrados:

1. Ir a la pestaña **Actions** en GitHub
2. Verificar que hay al menos una ejecución del workflow en `main`
3. Verificar que el check aparece en el commit de `main`

## Prevención

Para evitar este problema en el futuro:

- **Siempre incluir `push` a `main` en el workflow de CI**:
  ```yaml
  on:
    pull_request:
      branches: ["main"]
    push:
      branches: ["main"]
  ```

- Esto asegura que el CI se ejecute en `main` y los checks queden registrados
- Los checks registrados pueden ser exigidos en rulesets

## Referencias

- `decisions/github-as-judge.md` - Decisión sobre GitHub como autoridad técnica
- `patterns/flow-c1-issue-to-pr.md` - Patrón de trabajo que requiere CI

## Status

Recovery Playbook: ACTIVE
Última actualización: Solución validada para error de status checks vacíos
Vigencia: Aplicable cuando se configura rulesets con CI obligatorio
