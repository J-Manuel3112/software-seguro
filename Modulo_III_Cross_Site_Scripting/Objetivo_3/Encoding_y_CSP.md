# Objetivo 3: Defensa en Profundidad (Encoding y CSP)

## ¿Cómo mitigar definitivamente el riesgo de XSS?

Para mitigar el XSS de forma definitiva, un arquitecto de seguridad debe implementar una estrategia, aplicando dos capas de seguridad independientes: la codificación de salida (a nivel de código) y la Content Security Policy (a nivel de cabeceras HTTP). Si una capa falla, la otra sigue protegiendo al usuario.

---

### Capa 1: Output Encoding (Codificación de Salida a nivel de código)

La codificación de salida es la defensa principal contra el XSS. Consiste en convertir los caracteres especiales (`<`, `>`, `"`, `'`, `&`) en sus entidades HTML correspondientes antes de que el servidor envíe la respuesta al navegador.

---

### Capa 2: Content Security Policy (CSP) a nivel de cabeceras HTTP

La CSP es una capa de seguridad adicional que se configura en el servidor y se envía al navegador a través de una cabecera HTTP (`Content-Security-Policy`). Su función es indicarle al navegador qué fuentes de contenido están permitidas y cuáles deben bloquearse, incluso si un atacante logra inyectar un payload XSS, el navegador se negará a ejecutarlo porque el script no proviene de una fuente autorizada.

---
