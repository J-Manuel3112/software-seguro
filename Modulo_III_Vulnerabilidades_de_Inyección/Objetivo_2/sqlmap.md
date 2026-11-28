# Objetivo 2: Automatización vs Explotación Manual (sqlmap)

## Escenarios donde sqlmap falla y el atacante manual triunfa

### 1. Inyecciones en encabezados HTTP (Headers) o Cookies
**El fallo de sqlmap:** Por defecto, sqlmap se enfoca en los parámetros GET y POST visibles en la URL o en el cuerpo del formulario. Si la aplicación tiene un punto de inyección en un encabezado HTTP o dentro de una cookie de sesión , sqlmap puede pasarlo por alto si no se configura explícitamente con el flag `--headers` o `--cookie`. Muchas veces, la herramienta asume que la cookie es un token estático y no la testea.

**La ventaja del manual:** Un atacante manual, al interceptar la petición con Burp Suite, observa la estructura de todos los encabezados y cookies. Si nota que el servidor reacciona de forma distinta al modificar manualmente un `User-Agent` o una cookie (ejemplo: un retraso de tiempo o un cambio en la respuesta), puede explotar esa vulnerabilidad y extraer datos sin que sqlmap lo detecte.

### 2. Inyecciones con lógica de negocio compleja o validaciones personalizadas
**El fallo de sqlmap:** sqlmap utiliza firmas y patrones genéricos para detectar inyecciones. Si la aplicación implementa una lógica de negocio particular, como un filtro WAF (Web Application Firewall) que bloquea caracteres específicos, o un sistema de validación personalizado que ofusca los errores, sqlmap puede fallar al no reconocer el punto de inyección.

**La ventaja del manual:** Un atacante manual puede analizar el código JavaScript del frontend para entender cómo se construyen las consultas y cómo se filtran los datos. Puede crear payloads personalizados que pasen tranquilamente el filtro y funcionen. sqlmap no tiene la creatividad para adaptarse a una lógica de negocio única en tiempo real.