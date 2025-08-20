# Guía de Construcción de UI para Caliope

Este documento es el manual de estilo y la guía técnica estricta para el desarrollo de cualquier componente de frontend, página o panel de control en la aplicación Caliope. Su propósito es garantizar la consistencia, estabilidad y calidad del producto final, basándose en las lecciones aprendidas.

## 1. Principios Fundamentales (No Negociables)

1.  **La Estabilidad es la Prioridad #1**: Ningún componente o página debe comprometer el arranque o el funcionamiento básico de la aplicación. Los elementos críticos (layout, cabecera, logo) **deben** ser estáticos.
2.  **El Acceso de Usuario es Sagrado y Debe Funcionar Siempre**: El flujo de autenticación completo (registro e inicio de sesión, tanto con Google como con email/contraseña) es la funcionalidad más crítica de la aplicación. Debe ser robusto y funcionar perfectamente en `DEMO_MODE = true` usando el `mockUser`. La conexión real a Firebase es secundaria durante el desarrollo de la UI.
3.  **Diseñar para el Modo de Demostración**: Todas las páginas y componentes deben funcionar primero y perfectamente en `DEMO_MODE = true`. Esto implica usar `mockUser` y datos locales de los archivos JSON.
4.  **Permisos Claros y Seguros**: Las páginas protegidas (ej. `/admin`, `/catalogos`) **deben** verificar los permisos usando el hook `useAuth` y la propiedad `user.isAdmin`. Deben mostrar un mensaje de "Acceso Denegado" claro si el usuario no es administrador, en lugar de fallar o mostrar una página en blanco.

## 2. Stack Tecnológico y Arquitectura

-   **Framework**: Next.js con App Router.
-   **Lenguaje**: TypeScript.
-   **Componentes UI**: **ShadCN/UI**. Se debe dar prioridad al uso de los componentes existentes en `src/components/ui`.
-   **Estilos**: **Tailwind CSS**. No se deben usar otras librerías de CSS.
-   **Iconos**: **`lucide-react`**. Antes de usar un icono, se debe verificar que exista en la librería para evitar componentes rotos.

## 3. Guía de Estilo y Componentes

-   **Colores**: Los colores se definen como variables HSL en `src/app/globals.css`. **No usar colores hardcodeados** (ej. `bg-red-500`). Usar las variables de Tailwind (ej. `bg-primary`, `text-destructive`).
-   **Imágenes**:
    -   Para placeholders, usar `https://placehold.co/<width>x<height>.png`.
    -   El logo de la aplicación es **estático**. Se debe usar la URL definida en `src/components/ui/logo.tsx`. No se debe intentar hacerlo dinámico.
-   **Componentes Reutilizables**: Construir componentes pequeños y específicos (ej. `WelcomeHeader`, `SidePanel`) en lugar de páginas monolíticas.
-   **Formularios y Tablas**: Utilizar los componentes de ShadCN (`Table`, `Input`, `Button`, `LoginForm`, `RegisterForm`) para mantener la consistencia visual y funcional.

## 4. Manejo de Datos

-   **Fuente de Datos Principal (para UI)**: Para el desarrollo y la demostración, la fuente de verdad para los catálogos son los archivos locales `src/lib/wellness-services.json` y `src/lib/wellness-products.json`. El `mockUser` es la fuente para el estado de autenticación.
-   **Obtención de Datos**:
    -   Las páginas y componentes deben ser `Client Components` (`'use client'`) para interactuar con los hooks y el estado.
    -   Los datos se obtienen a través de `Server Actions` definidas en `src/app/admin/actions.ts` que leen los archivos JSON.
    -   Esto asegura que la UI funciona independientemente del estado de la conexión a Firebase.

## 5. Estructura de Páginas de Administración (Dashboard)

Toda página de administración o panel de control (ej. `/admin`, `/catalogos`) debe seguir esta estructura:

1.  **Componente Principal (`'use client'`)**: La página principal que orquesta todo.
2.  **Hook `useAuth`**: Obtener `user` y `loading` al principio del componente.
3.  **Estado de Carga**: Mostrar un loader (`<Loader2 className="animate-spin" />`) mientras `authLoading` sea `true`.
4.  **Verificación de Permisos**: Una vez que la carga finaliza, verificar si `!user.isAdmin`. Si es `true`, renderizar un componente de `Acceso Denegado` con un `Link` para volver al inicio. No permitir que el resto de la página se renderice.
5.  **Obtención de Datos**: Si el usuario es administrador, llamar a las Server Actions para obtener los datos necesarios (ej. `getWellnessServices`). Mantener un estado de carga de datos (`isDataLoading`).
6.  **Renderizado del Contenido**: Usar componentes de ShadCN como `Card`, `Tabs`, y `Table` para mostrar la información de manera clara y organizada.

Siguiendo estas directrices de manera estricta, aseguraremos un desarrollo fluido y un producto final robusto y sin errores recurrentes.
