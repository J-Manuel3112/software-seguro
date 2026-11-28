# Objetivo 1: El Arte de la Evasión de Filtros (WAF Bypass)

## ¿Por qué es inútil filtrar solo la etiqueta `<script>`?

Es inútil porque el desarrollador asume que el único vector de ataque para ejecutar JavaScript es la etiqueta `<script>`. Sin embargo, los navegadores modernos permiten ejecutar código JavaScript a través de múltiples eventos y atributos HTML. Un filtro basado en listas negras es fácilmente evadible porque:

1.  **Existen otros vectores:** Hay decenas de atributos HTML que ejecutan código cuando ocurre un evento.
2.  **El contexto importa:** El atacante puede inyectar su payload de distintas formas, sin necesidad de usar `<script>`.
3.  **Codificaciones alternativas:** El atacante puede usar codificaciones para ofuscar los caracteres y evadir el filtro.

---

## Payloads alternativos (sin usar `<script>`)

### Payload 1: Atributo `onerror` en una imagen rota
El navegador intenta cargar una imagen que no existe (`src=x`), lo que genera un error de carga (`onerror`). El código JavaScript dentro del atributo `onerror` se ejecuta automáticamente.

```html
<img src=x onerror="alert('XSS ejecutado')">
```
### Payload 2: Atributo onload en un SVG
El elemento <svg> es un formato de imagen vectorial que soporta eventos. Cuando el SVG termina de cargar, ejecuta el código JavaScript.

```html
<svg onload="alert('XSS ejecutado')">
```