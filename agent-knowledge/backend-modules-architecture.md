---

# Backend Architecture — PMD Sistema Base v1

## Decisión de Arquitectura (BLOQUE 3)

- La arquitectura backend se organiza por módulos de dominio, no por capas técnicas.

- Auth depende de Users; Users no depende de Auth.

- Los módulos de negocio no importan Auth ni Users; utilizan `req.user` como contrato de identidad.

- Roles y Organizations son dominios independientes.

- El directorio `common/` concentra infraestructura transversal:

  guards, decorators, filters, interceptors, helpers y constants.

- No se realiza refactor estructural sin evidencia de acoplamiento crítico o rotura funcional.

Esta arquitectura fue auditada y validada como:

- estable para producción,

- consistente internamente,

- y adecuada como base del IAS.

