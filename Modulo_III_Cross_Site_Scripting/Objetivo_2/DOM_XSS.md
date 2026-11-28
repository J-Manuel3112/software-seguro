# Objetivo 2: Anatomía del DOM XSS

## ¿Por qué el DOM-Based XSS es indetectable para WAFs e IPS?

La razón principal por la que el DOM-Based XSS evade los sistemas de prevención (WAF/IPS) es que **el ataque nunca viaja al servidor**. Todo el proceso malicioso ocurre en el navegador del usuario (del lado del cliente), por lo que el tráfico de red no contiene ninguna firma sospechosa que el WAF pueda detectar.

### El papel del fragmento `#` en la URL

Cuando una URL contiene un fragmento (ej. `https://ejemplo.com/#<script>alert(1)</script>`), el navegador no envía esa parte al servidor. El fragmento se queda en el navegador y se utiliza para navegar dentro de la misma página (por ejemplo, para saltar a una sección específica). 

Los WAFs e IPS solo inspeccionan las peticiones HTTP que viajan al servidor. Como el payload malicioso va en el fragmento (`#`), nunca pasa por el firewall. Es invisible para el WAF.

---

### ¿Qué pasa cuando el JavaScript procesa location.hash de forma insegura?

Muchas aplicaciones web usan JavaScript para leer el fragmento de la URL y mostrar contenido dinámico. Por ejemplo, un script podría hacer:

```javascript
let contenido = location.hash.substring(1);
document.getElementById('div').innerHTML = contenido;
```
innerHTML le dice al navegador que interprete el contenido como HTML. Si el atacante coloca un payload en el fragmento de la URL, el script lo lee (location.hash), lo procesa y lo inyecta directamente en el DOM usando innerHTML. El navegador interpreta ese texto como código HTML y ejecuta el evento onerror, sin que el servidor ni el WAF se enteren.
