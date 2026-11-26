# Objetivo 4: Manejo Inseguro de Errores (Stack Traces)


### Mensaje de error genérico
Es una respuesta controlada y amigable que el servidor devuelve al usuario cuando ocurre un fallo. Por ejemplo: "Ocurrió un error inesperado. Por favor, intente más tarde.", no revela detalles internos, no muestra rutas de archivos, ni versiones del servidor. El usuario entiende que algo falló, pero un atacante no obtiene información útil.

### Stack Trace
Es el registro técnico completo de la ejecución del programa hasta el punto exacto donde falló. Por ejemplo un texto que contiene la ruta absoluta de un archivo, el número de línea y la función que se estaba ejecutando. Expone la estructura interna del código, los nombres de los archivos, las rutas del servidor y, a veces, la versión del framework o del lenguaje.

## ¿Por qué revelar un Stack Trace detallado facilita la explotación?

En un entorno de producción, un atacante que logra recibir un Stack Trace obtiene información técnica detallada gratis, que acelera drásticamente un ataque. Los principales riesgos son:

1. **Mapeo de la aplicación:** El atacante descubre qué framework está usando, qué librerías incluye y en qué rutas exactas están instaladas.
2. **Enumeración de archivos internos:** Conoce los nombres exactos de los archivos y puede probar ataques dirigidos a rutas específicas.
3. **Fuga de rutas absolutas:** El servidor revela dónde está instalado el proyecto. Esto permite al atacante intentar ataques de Path Traversal o LFI hacia archivos de configuración.
4. **Identificación de versiones vulnerables:** Los Stack Traces suelen incluir la versión exacta de las librerías. El atacante puede cruzar esa información con bases de datos de vulnerabilidades (CVE) y buscar exploits públicos.
5. **Ajuste de payloads:** Los errores de SQL o de validación permiten al atacante saber exactamente cómo está armada la consulta, lo que le facilita inyecciones SQL exitosas.

---