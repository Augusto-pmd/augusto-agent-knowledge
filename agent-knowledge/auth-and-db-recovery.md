---

# Auth & DB — PMD Sistema Base v1

## Decisiones de Autenticación (BLOQUE 2)

- La expiración de JWT debe configurarse siempre mediante variables de entorno.

- El transporte oficial de autenticación es Authorization Bearer (no cookies).

- Las cookies, si existen, deben configurarse con httpOnly: true para reducir riesgo de XSS.

- El endpoint /auth/login debe tener rate limiting activo en producción.

- El algoritmo JWT debe declararse explícitamente (HS256).

## Playbook de Recovery — PostgreSQL en Render

- PostgreSQL en Render requiere SSL explícito en todos los entornos productivos.

- La DATABASE_URL debe incluir obligatoriamente el parámetro ?sslmode=require.

- Además del parámetro en la URL, el cliente (TypeORM) debe habilitar SSL.

- Si aparece el error:

  - getaddrinfo ENOTFOUND → hostname inválido o URL truncada.

  - Connection terminated unexpectedly → falta SSL en la URL o en el cliente.

- La DATABASE_URL no debe escribirse a mano:

  - siempre copiar External Database URL desde Render cuando hay fallos de red interna.

Este archivo registra decisiones técnicas cerradas y un recovery validado en producción real.

