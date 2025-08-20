# Guía de Implementación y Estabilidad

Este documento establece las directrices y prioridades para el desarrollo de esta aplicación, con el fin de evitar errores recurrentes y asegurar un entorno de trabajo estable.

## Principios Fundamentales

1.  **La Estabilidad es la Prioridad #1:** Ninguna nueva funcionalidad debe comprometer el arranque o el funcionamiento básico de la aplicación. Los componentes esenciales (como el layout, la cabecera, el logo) deben ser estáticos y no depender de llamadas a bases de datos o servicios externos que puedan fallar.

2.  **El Acceso del Usuario es Innegociable:** Los flujos de autenticación (registro e inicio de sesión) son críticos y deben funcionar sin fallos. Esto incluye tanto el inicio de sesión con Google como el registro/inicio de sesión manual con correo electrónico y contraseña. En el modo de demostración, estos flujos deben simularse de forma robusta para permitir el acceso completo a la aplicación, utilizando un `mockUser` con privilegios de administrador (`isAdmin: true`) para garantizar el acceso a todas las áreas protegidas.

3.  **Aislar los Problemas de Configuración:** Los errores relacionados con credenciales de Firebase (`PERMISSION_DENIED`, `auth/api-key-not-valid`) son problemas de entorno (`.env`). La solución para el desarrollo es usar un **Modo de Demostración** robusto que evite las llamadas a Firebase, permitiendo que el desarrollo continúe sin bloqueos. No se debe intentar "arreglar" un problema de credenciales modificando el código de la aplicación.

4.  **Principio de Diagnóstico Metódico (La Lección del Error Infinito):** Ante un error de compilación o ejecución, se debe actuar con disciplina:
    *   **No Repetir una Solución Fallida:** Si una corrección no funciona, es un indicativo de que el diagnóstico es incorrecto. Insistir en la misma solución es un error. Hay que detenerse y reevaluar.
    *   **Ampliar el Foco del Diagnóstico:** Un error reportado en un archivo (ej. `Page.tsx`) puede ser un síntoma de un problema en una dependencia (ej. un `componente` importado). Se debe investigar el árbol de dependencias del archivo que falla.
    *   **Confiar en la Evidencia:** La evidencia es el resultado del compilador. Si la corrección no resuelve el problema, la premisa está mal. La prioridad absoluta es encontrar la **causa raíz**, no aplicar parches superficiales en el lugar equivocado.

## Acciones Prohibidas

-   **No hacer que componentes críticos dependan de la base de datos:** El logo, la navegación principal y otros elementos del layout no deben realizar llamadas asíncronas para su renderizado inicial.
-   **No desactivar el Modo de Demostración a menos que se esté probando la conexión a Firebase a propósito:** El modo de demostración es la garantía de un entorno estable.
-   **No caer en el "diagnóstico en túnel":** No insistir en una solución que ya ha demostrado ser ineficaz.

## El Estado Funcional Base

La aplicación se considera "funcional" cuando cumple con lo siguiente:
- El servidor de Next.js se inicia sin errores.
- La página de inicio carga correctamente.
- Es posible iniciar sesión con el usuario de prueba en modo de demostración. Esto incluye la simulación de los botones de "Iniciar Sesión con Google" y el formulario de email/contraseña.
- El usuario de prueba tiene acceso al Panel de Administración (`/admin`) y a los Catálogos (`/catalogos`).

Cualquier cambio que rompa uno de estos puntos debe ser revertido inmediatamente.
