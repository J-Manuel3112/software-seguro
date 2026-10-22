# Informe de Laboratorio: Turnero 

## 1. Descripción del Desafío
La idea de este desafío era borrarle todos los turnos médicos al usuario "xdalvik", que los había reservado a propósito para molestar, pero sin tocar los turnos de los demás pacientes. Para hacer esto, la consigna me daba un usuario estándar llamado "Luis" con credenciales válidas para entrar al sistema.



## 2. Análisis y Hallazgos Técnicos

### Vulnerabilidad
Lo que noté que me ayudó para esta actividad es que el sistema tiene una vulnerabilidad de control de acceso (conocida técnicamente como IDOR). 

### Lo que entendí que hace el sistema
Básicamente, el backend verifica que haya una sesión activa (en este caso, la mía con Luis), pero **no le importa quién manda la orden de borrar un turno**. Recibe la orden HTTP `DELETE`, va a la base de datos y lo borra directamente sin más. Es decir, omite validar si el turno que se intenta borrar le pertenece a la persona que mandó la petición.



## 3. Solución

1. 
   - Entré a la página con el usuario de Luis y abrí la herramienta "Inspeccionar" del navegador, yendo a la pestaña de Red. 
   - Presioné en "cancelar" en uno de mis propios turnos para ver qué acciones se realizaban por detrás. Ahí descubrí la estructura: hacía un `GET /api/1/appointments` (donde 1 era mi ID de usuario) para listar los turnos, y al borrar mandaba un `DELETE /api/1/appointments/1`.

2. 
   - Como ya sabía que mi ID era el 1, empecé a tantear números a mano cambiando la URL. 
   - Después de recibir un par de mensajes que me indicaban "usuario no existente", pude dar con la ID del usuario "xdalvik", que resultó ser la `101`.

3. 
   - Mandé una petición a `/api/101/appointments`. Al abrir la estructura JSON de la respuesta, pude ver todos los turnos de este atacante. 
   - Anoté los números de ID característicos de sus turnos, que eran: `10`, `12` y `13`.

4. 
   - Presionando en "Ver y editar" sobre la petición original en la pestaña de Red, armé y mandé tres peticiones `DELETE` manuales sobre cada uno de los turnos de xdalvik:
     - `DELETE /api/101/appointments/10`
     - `DELETE /api/101/appointments/12`
     - `DELETE /api/101/appointments/13`
   - En cada caso el sistema me respondió "turno eliminado correctamente", logrando cumplir la consigna sin afectar a nadie más.

---


# Informe de Laboratorio: Presupuesto

## 1. Descripción del Desafío
El objetivo de este laboratorio era reacomodar los números de un presupuesto de gastos para que cumplan con una lista de requisitos estadísticos estrictos: un promedio total de 8000, un promedio de gastos esenciales de 16375, uno de varios de 6000, con un mínimo de 500 y un máximo de 50000. Además, todo debía quedar marcado como "revisado". Para ingresar, usé el usuario "acceso".



## 2. Análisis y Hallazgos Técnicos

### Vulnerabilidad
El sistema maneja la información del presupuesto a través de una API. Me di cuenta de que para alterar los promedios, tenía que modificar los valores de cada ítem directamente comunicándome con el servidor. El problema de seguridad acá es que la API expone la estructura interna de los datos y permite alterar campos críticos (como el monto o el estado de revisión) simplemente descubriendo la ruta de edición correcta, sin validaciones extra en la interfaz.

---

## 3. Solución

1. 
   - Hice clic en uno de los botones de "revisar" dentro de la tabla para ver qué mandaba el navegador. 
   - Revisando las peticiones, encontré que el endpoint `/api/gastos/` me devolvía un JSON espectacular con todos los detalles de cada ítem (ID, título, monto, si estaba revisado o no, y el tipo de gasto). 

2. 
   - Con esa data, fui a la pestaña Red, le di a "Ver y editar" a la petición y cambié el método a `POST` (ya que el código fuente mostraba que los datos se mandaban así).
   - Primero intenté pegarle a `/api/gastos/1` para modificar el primer ítem, pero no hizo nada. 
   - Tanteando un poco más las rutas, descubrí(gracias al dato de un error) que la dirección correcta para modificar datos era agregando la acción al final: `/api/gastos/1/editar/`.

3. 
   - Para que el servidor me acepte el cambio, tuve que agregar a mano un encabezado en la petición: `Content-Type: application/json`, esto para que el servidor lo lea como lo que es, un JSON.
   - En el cuerpo mandé la estructura JSON necesaria:
     ```json
     {
       "monto": "6000",
       "revisado": true
     }
     ```
   - *Nota: El `true` era fundamental porque la consigna pedía dejar todo revisado (valor booleano).*
   - La respuesta del servidor fue positiva y el cambio se hizo

4. 
   - Como el JSON del principio ya me decía qué tipo de gasto era cada ítem, hice los cálculos matemáticos a mano para saber qué valor exacto ponerle a cada ID y que los promedios cierren perfecto.
   - Fui repitiendo la petición a cada ruta (`/api/gastos/2/editar/`, `/api/gastos/3/editar/`, etc.) con sus respectivos valores hasta resolver la consigna.

---


# Informe de Laboratorio: Ventas

## 1. Descripción del Desafío
Un amigo llamado Fernando necesitaba saber cuántas ventas hizo su competencia. El objetivo era descubrir ese número exacto, pasarlo por un generador de MD5 en internet y usar ese hash como código para superar el desafío. Las ventas estaban alojadas en la ruta `/ventas`.

## 2. Análisis y Hallazgos Técnicos
Al principio, entrar a la página me tiraba un error genérico 404 (Not Found). Cuando le agregué la extensión `/ventas` a la URL, me empezó a tirar un error 400 (Bad Request). Ahí deduje que estaba en el lugar correcto pero me faltaba pasarle algún parámetro. 

Empecé a tantear agregando `/?id=1` a la URL y me devolvió un **403 Forbidden**. Eso fue clave ya que me indicaba que la venta existía, pero no me dejaba verla por permisos. Seguí probando otros números de ID a mano y vi que se alternaban desordenadamente entre 403 (existe la venta) y 404 (venta inexistente). 
El problema de seguridad acá es que el sistema "filtra" información mediante los códigos de estado HTTP, permitiendo a un atacante inferir qué registros existen.

## 3. Solución

1. 
   - Como sabía que tenía que averiguar el número total contando solo los errores 403, decidí programar un pequeño script en JavaScript directamente en la consola del navegador para que haga el conteo de 403(venta):

   ```javascript
   async function contarVenta() {
     let limite = 2500; 
     let cantidad403 = 0;

     for (let i = 1; i <= limite; i++) {
       try {
         // Aquí hago la petición
         let respuesta = await fetch(`/ventas/?id=${i}`); 
         
         // Verifico que el error devuelto es 403 y sumo 1 al contador
         if (respuesta.status === 403) {    
           cantidad403++; 
         }
       } catch (e) {
         console.log(e);
       }
     }
     console.log(`Cantidad de códigos 403: ${cantidad403}`);
   }

   contarVenta();
   ``
   
2. 
    - Corrí el código, me tiró un número, lo pasé por md5hashgenerator.com, pero el código me dio incorrecto. Resultó que aún quedaban ventas más allá del límite, ventas que faltaban escanear.
    - Cambié el límite a 5000 y modifiqué el bucle para que arranque en let i = 2501. Lo volví a correr. Mientras lo hacía, me fijé en la pestaña de Red y vi que pasadas ciertas IDs, el servidor empezó a tirar más de mil peticiones con 404 de forma seguida. Ahí entendí (comprobé) que ya se habían terminado las ventas.
    
3. 
    - Sumé el número de esta segunda tanda con el de la primera, lo convertí a MD5 y el código funcionó perfecto, terminando la consigna.

---


# Informe de Laboratorio: Gran Rifa 2019

## 1. Descripción del Desafío
En este caso, la consigna era ayudar a un amigo ("John Backus") que se anotó para comprar una rifa en el sistema, pero no la pagó ni tenía intenciones de hacerlo. El objetivo era modificar internamente su estado de pago. Para entrar al sistema, usé las credenciales del usuario "guido".

## 2. Análisis y Hallazgos Técnicos
Al ingresar al sistema, vi un listado con todos los compradores. Al lado de cada uno había un botón de "editar", pero la interfaz web únicamente me dejaba cambiar el nombre de la persona, nada relacionado con el dinero. 
Acá el problema de seguridad es que el backend agarra todo el paquete de datos que le manda el usuario y lo actualiza directo en la base de datos sin filtrar qué campos se pueden tocar y cuáles son exclusivos de los administradores (como tendría que ser el estado de pago).

## 3. Solución

1. 
   - Abrí el panel de desarrollo (Inspeccionar) y fui a la pestaña Red. 
   - Al cargar la página, observé el método `GET` principal en la ruta `/api/numeros/`. En la respuesta vi el JSON con la estructura completa de cada casilla. 
   - Ahí descubrí dos cosas clave: la ID de John Backus era el `4`, y el parámetro interno que controlaba el pago se llamaba `"esta_pago"` (el cual recibía un valor booleano: true o false).

2. 
   - Para ver cómo se mandaban los datos al servidor, le di a "editar" en la casilla de John Backus y luego a "guardar" sin cambiar nada. 
   - Esto generó una petición `POST` hacia la dirección `/api/numeros/4/editar/`.

3. 
   - Agarré esa última petición `POST` y le di a "Ver y editar" para manipularla manualmente.
   - En el apartado del Cuerpo, donde solo figuraba el nombre del comprador, le agregué a mano el parámetro de pago para forzar el cambio. El JSON quedó así:
     ```json
     {
       "comprador": "John Backus",
       "esta_pago": true
     }
     ```
   - Al enviar esto, recibí una respuesta confirmando que los datos fueron cambiados. Así logré alterar el estado de pago únicamente de John Backus, exponiendo la tremenda facilidad de manipulación de datos que tenía el sistema.

---