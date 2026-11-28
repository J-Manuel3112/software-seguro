# Objetivo 3: La Única Mitigación Definitiva (Prepared Statements)


## ¿Cómo funcionan las Consultas Parametrizadas (Prepared Statements)?

Las listas negras (bloquear palabras como `SELECT` o `UNION`) y el escape de comillas (convertir `'` en `\'`) son medidas **reactivas** que intentan adivinar qué payloads maliciosos puede enviar un atacante. Son ineficientes porque un atacante las puede evadir fácilmente, además son difíciles de mantener.

Los Prepared Statements no intentan limpiar ni filtrar los datos. En su lugar, separan la estructura de la consulta SQL de los datos enviados por el usuario a nivel de arquitectura. El proceso ocurre en dos fases:

### Fase 1: Preparación
El desarrollador escribe la consulta SQL usando marcadores de posición(?) en lugar de concatenar los datos. Por ejemplo:
```sql
SELECT * FROM usuarios WHERE username = ? AND password = ?
```
El motor de la base de datos recibe la plantilla, la analiza y compila.

### Fase 2: Ejecución
El programa envía los datos del usuario por separado de la consulta. El motor de la base de datos inserta esos datos en los huecos ? sin volver a analizar la estructura SQL.
Como la estructura ya está compilada, el motor trata los datos como texto puro, no como código. Si un atacante envía ' OR '1'='1, la base de datos no lo interpreta como una condición; simplemente lo trata como una contraseña literal, la compara y la rechaza.

## ¿Por qué neutraliza por completo cualquier intento de inyección?

Neutraliza la inyección porque elimina la raíz del problema: la mezcla entre código y datos. En una consulta concatenada tradicional, el motor de la base de datos no puede distinguir dónde termina la instrucción SQL y dónde empiezan los datos del usuario. Por eso, si el usuario envía comillas o comandos, el motor los interpreta como parte del código.