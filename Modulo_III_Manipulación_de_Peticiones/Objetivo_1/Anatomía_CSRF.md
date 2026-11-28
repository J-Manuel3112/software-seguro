# Objetivo 1: Anatomía de un Ataque CSRF (Cross-Site Request Forgery)

## Flujo exacto de un ataque CSRF exitoso

Un ataque CSRF ocurre cuando un atacante engaña al navegador de la víctima para que envíe una petición no deseada a un sitio web en el que la víctima está autenticada. El flujo es el siguiente:

1.  **Paso 1:** La víctima inicia sesión en el sitio legítimo. El servidor le asigna una cookie de sesión válida que el navegador almacena.
2.  **Paso 2:** El atacante crea una página web maliciosa que contiene un formulario oculto o una imagen que apunta a una acción sensible en el sitio legítimo.
3.  **Paso 3:** El atacante convence a la víctima de visitar su sitio malicioso (por un enlace en un correo, un mensaje, etc.).
4.  **Paso 4:** Cuando la víctima carga la página maliciosa, el navegador ejecuta la petición al sitio legítimo. El navegador copia automáticamente la cookie de sesión de la víctima en esa petición, porque la cookie pertenece a su dominio.
5.  **Paso 5:** El servidor del banco recibe la petición con la cookie válida y la procesa como si la víctima la hubiera iniciado voluntariamente. La transferencia se ejecuta sin que la víctima lo sepa.

---

## ¿Por qué ocurre este fallo de seguridad?

Los navegadores están diseñados para adjuntar automáticamente las cookies a cualquier petición que se dirija al dominio al que pertenecen, sin importar desde dónde se originó la petición. 

El servidor no puede distinguir si la petición fue iniciada por el usuario legítimo desde el sitio, o por un script malicioso desde otro sitio. Solo ve una petición con una cookie válida y la acepta.

---

## ¿Cómo mitiga el atributo SameSite?

El atributo SameSite es una directiva que se añade a las cookies para controlar si el navegador las envía en peticiones que se originan desde otros dominios (cross-site). Tiene tres valores principales:

- SameSite=Strict :La cookie solo se envía si la petición se origina desde el mismo sitio. Si la víctima navega desde un sitio a otro diferente, la cookie no se enviará. Esto bloquea completamente el CSRF.

- SameSite=Lax: La cookie solo se envía en navegaciones de nivel superior (cuando el usuario hace clic en un enlace que lo lleva al sitio) pero no se envía en peticiones automatizadas (como formularios ocultos o imágenes). Es un equilibrio entre seguridad y usabilidad.
