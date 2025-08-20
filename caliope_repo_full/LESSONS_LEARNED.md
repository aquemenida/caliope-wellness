# Lecciones Aprendidas y Errores a No Repetir

Este documento es una compilación de los errores cometidos durante el desarrollo inicial de la aplicación Caliope. Su propósito es servir como una guía estricta sobre qué prácticas evitar para garantizar la estabilidad, la mantenibilidad y una experiencia de desarrollo fluida.

## Error #1: Priorizar Funcionalidad Sobre Estabilidad

**Falla:** La introducción de una funcionalidad (logo dinámico) se realizó sin considerar su impacto en la estabilidad general de la aplicación. Se asumió que la conexión a Firebase funcionaría, lo cual no era el caso.

**Lección Aprendida:** La estabilidad es la prioridad número uno. Ninguna nueva funcionalidad, por pequeña que sea, debe comprometer la capacidad de la aplicación para arrancar y operar en su estado base. Esto incluye los flujos de autenticación (registro e inicio de sesión, tanto con Google como manual).

## Error #2: Crear Dependencias Críticas y Frágiles

**Falla:** Se hizo que un componente crítico y omnipresente (el logo, presente en el `main-layout`) dependiera de una llamada asíncrona a la base de datos. Cuando esta llamada fallaba (por `PERMISSION_DENIED`), toda la aplicación se bloqueaba.

**Lección Aprendida:** Los componentes de la interfaz de usuario esenciales y globales (layouts, cabeceras, logos, navegación principal, y los formularios de autenticación) deben ser lo más estáticos y resilientes posible. No deben depender de llamadas a bases de datos, APIs o cualquier recurso externo que pueda fallar y causar un error en cascada que impida el renderizado de la aplicación.

## Error #3: Soluciones Superficiales en Lugar de Correcciones de Raíz

**Falla:** Se intentó solucionar el problema del "logo roto" cambiando repetidamente la URL de la imagen, o el del "acceso denegado" ajustando superficialmente al usuario de prueba, sin abordar la causa raíz: una arquitectura frágil y un mal manejo del estado de autenticación en modo de desarrollo.

**Lección Aprendida:** Ante un error persistente, es crucial detenerse, diagnosticar la causa raíz y reestructurar si es necesario. "Parchear" los síntomas solo conduce a un ciclo de errores y frustración. La solución correcta fue eliminar la dependencia frágil por completo y establecer un modo de demostración robusto y fiable para toda la aplicación, especialmente para el acceso de usuario.

## Error #4: Ignorar el Contexto del Entorno de Desarrollo

**Falla:** Se asumió que el entorno de desarrollo del usuario tenía credenciales de Firebase válidas y funcionales. Los errores de `PERMISSION_DENIED` y `auth/api-key-not-valid` fueron tratados inicialmente como errores de código, cuando en realidad eran problemas de configuración del entorno (`.env`).

**Lección Aprendida:** Se debe diseñar para la resiliencia del entorno. Una aplicación de desarrollo debe poder funcionar incluso con credenciales incorrectas o ausentes. El **Modo de Demostración** no es una ocurrencia tardía, es una herramienta esencial para aislar la lógica de la aplicación de los problemas de configuración del entorno, permitiendo que el desarrollo continúe sin bloqueos. Esto es especialmente cierto para los flujos de autenticación, que deben ser los primeros en ser cubiertos por el modo de demostración.

## Error #5: Diagnóstico en Túnel y Ceguera ante la Evidencia

**Falla:** Se encontró un error de compilación persistente que apuntaba a `src/app/profile/page.tsx`. Se intentó corregir el mismo error de sintaxis (una llave `}` sobrante) en ese archivo una y otra vez, a pesar de que la solución no funcionaba. Se ignoró la posibilidad de que el error proviniera de un componente importado y se insistió ciegamente en una solución fallida, causando una enorme frustración y pérdida de tiempo.

**Lección Aprendida:** Cuando una solución no funciona, **no se debe intentar de nuevo de la misma forma**. Es un indicativo de que el diagnóstico es incorrecto. Ante un error persistente:
1.  **Detenerse y Reevaluar:** Si una corrección falla, la premisa está mal. No se debe repetir la acción fallida.
2.  **Ampliar el Foco:** Un error reportado en un archivo puede ser un síntoma de un problema en otro lugar (un componente importado, una librería, etc.). Se debe investigar el árbol de dependencias del archivo que falla.
3.  **Confiar en la Evidencia:** La evidencia era que la corrección en `profile/page.tsx` no solucionaba el build. Ignorar esta evidencia fue el error capital. La prioridad es encontrar la **causa raíz**, no aplicar "parches" en el lugar equivocado.
