# Objetivo 3: Anatomía de una Condición de Carrera (Race Condition)

## ¿Qué son las vulnerabilidades de tiempo de procesamiento (Race Conditions / TOCTOU)?

Son fallos que ocurren cuando el resultado de una operación debe seguir un orden de ejecuciones estrictamente concurrentes. Esto permite que los atacantes realicen acciones no autorizadas aprovechando que hay un espacio entre la acción y la verificación, se pueden alterar valores, datos, etc 

Estas vulnerabilidades son muy altamente peligrosas en aplicaciones web que reciben múltiples peticiones al mismo tiempo.

## El Ataque con Turbo Intruder

Herramientas como Turbo Intruder están diseñadas para este tipo de ataques. A diferencia del Intruder normal, Turbo Intruder usa un motor asíncrono que permite enviar decenas o cientos de peticiones HTTP exactamente al mismo tiempo, sin esperar a que la anterior reciba respuesta ocasionando errores o fallos de seguridad críticos .

Así es como el atacante logra canjear un código de un solo uso múltiples veces:

1.  **Paso 1:** El atacante intercepta la petición `POST` que se envía al aplicar el código de descuento.
2.  **Paso 2:** Configura Turbo Intruder para enviar 50 peticiones idénticas de forma simultánea.
3.  **Paso 3:** El servidor recibe las 50 peticiones casi en el mismo milisegundo. Como las 50 llegan antes de que el servidor termine de procesar y guardar el cambio en la base de datos, la verificación se ejecuta 50 veces seguidas con éxito para todas las peticiones (el código todavía parece válido).
4.  **Paso 4:** El cupón se ingresa 50 veces. El sistema aplica el descuento 50 veces y descuenta el saldo/inventario 50 veces, aunque el código solo debía usarse una vez.

