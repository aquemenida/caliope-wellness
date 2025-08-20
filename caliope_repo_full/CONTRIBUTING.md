# Contributing to Caliope App

Gracias por contribuir a Caliope. Por favor, sigue estas pautas antes de enviar un pull request.

## Principios innegociables

- **Estabilidad primero:** El servidor debe arrancar y la pantalla de inicio debe cargar sin errores.
- **Demo Mode:** Usa `DEMO_MODE=true` durante el desarrollo de la UI. No desactives este modo salvo pruebas reales.
- **Flujo de autenticación:** Las páginas protegidas (`/admin`, `/catalogos`) deben validar `user.isAdmin` y mostrar "Acceso Denegado" cuando corresponda.
- **Evitar regresiones:** Cualquier cambio que rompa el Estado Funcional Base debe revertirse inmediatamente.

## Checklist obligatorio en Pull Requests

Todas las contribuciones deben completar el checklist incluido en `.github/pull_request_template.md`. Si alguna casilla no aplica, explica por qué en el apartado de notas del PR.

## Código y estructura

- Mantén los componentes del frontend pequeños, reutilizables y sin dependencias frágiles.
- No dinamices elementos globales como el logo, el layout o la cabecera.
- Coloca las variables de entorno en `.env.local` (usa `.env.example` como referencia).

## Automatización y CI/CD

- Los pipelines de GitHub Actions son obligatorios. No desactives ningún workflow sin justificación.
- Configura los secretos necesarios (`FIREBASE_SERVICE_ACCOUNT_BASE64`, `GCP_PROJECT_ID`, etc.) en la configuración del repositorio.

Gracias por ayudar a que Caliope sea un producto robusto y de alta calidad.