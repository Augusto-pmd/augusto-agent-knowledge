---

# Infra & Deploy — Fuente de Verdad

## PMD Sistema Base v1

En PMD Sistema Base v1 se establece como regla inquebrantable:

- La URL de backend en producción debe confirmarse siempre desde el Render Dashboard.  
- El frontend debe consumir explícitamente `${BACKEND_URL}/api` cuando existe `setGlobalPrefix('api')`.  
- El puerto efectivo del backend es el inyectado por la plataforma de deploy (Render); el fallback en código solo es contingencia.  
- No se consideran válidos defaults inseguros en variables críticas (ej.: `JWT_SECRET`).  
- El método de deploy válido es el que se usa efectivamente en producción, no el documentado como ideal.

Este archivo registra una **decisión técnica cerrada** surgida de la auditoría Infra & Deploy del sistema PMD.

