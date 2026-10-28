### Laboratorio: Apagar la IA

* **Vulnerabilidad:** IDOR (Insecure Direct Object Reference).
* **Descripción:** Se simuló un ataque proveniente de una IA, para poder desactivarla tenía que hallar el código de desactivación de 16 dígitos que estaba dentro de un html(se nos brindó el acceso a dos los cuales no contenían el código).
* **Desarrollo:** 
    1. Gracias a los dos html se pudo descubrir la estructura de la URL, también se supo que cada html era identificado con un código HASH MD5 el cual, al transformar me daba un número.
    2. Debido a que el tamaño de las respuestas HTML era uniforme, se desarrolló un script en Python para iterar las URLs conforme a números en secuencia convertidos a HASH MD5, filtrar el contenido de cada HTML y buscar el código mediante una Expresión Regular (`\b\d{16}\b`) el cuál me devuelve el código de 16 dígitos, junto con el número de orden en el que se encuentra.
    3. Se localizó el archivo que contenía el código de desactivación de 16 dígitos, se buscó el equivalente en MD5 del número de orden que me dió, así se pudo visualizar el html completo con el código de desactivación.
* **Resultados:**
    - **ID Encontrado:** `11520` - **HASH MD5:** `cdf49f5251e7b3eb4f009483121e9b64`
    - **Código de 16 dígitos:** `5524663362514956`
    - **Hash MD5 enviado al juez:** `a8e0e8ff02dde0f62fdf4de5142d7de0`
    - **Estado:** Resuelto exitosamente.


---


### Laboratorio: El mejor secreto

* **Vulnerabilidad:** Fuerza Bruta
* **Descripción:** Se obtuvo un video de un jefe de estado tecleando una contraseña de 12 dígitos en un teclado numérico modificado. Las etiquetas físicas no coincidían con los dígitos reales y cada tecla correspondía a un valor único. El objetivo era descifrar el archivo `secreto.zip`.
* **Desarrollo:** 
    1. Se analizó el video cuadro por cuadro para extraer la secuencia exacta de 12 pulsaciones, identificando las repeticiones y agrupándolas por posiciones según el patrón: `A B C C D A D D D D E C` (donde A, B, C, D, E representan 5 variables distintas).
    2. Mediante un análisis combinatorio, se logró concluir que la cantidad de combinaciones posibles era **30240**, utilizando dígitos del 0 al 9 sin repetir.
    3. Se desarrolló un script en Python utilizando la librería `pyzipper` para automatizar un ataque de diccionario, probando las combinaciones generadas contra el archivo `secreto.zip` protegido con cifrado AES.
- **Resultados:**
    - **Patrón analizado:** `ABCCDADDDDEC`
    - **Total de combinaciones probadas:** `30240`
    - **Contraseña correcta encontrada:** `547795999937`
    - **Estado:** Resuelto exitosamente.


---


### Laboratorio: Votación
* **Vulnerabilidad:** Broken Access Control
* **Descripción:** En una página de votación entre dos universidades(UTN y HARVARD), se observa que Harvard tiene muchos más votos que la UTN. Sabiendo que la página sólo admite un voto, se debe hacer que la UTN obtenga más votos que Harvard.
* **Desarrollo:** 
    1. Se observó que la página contaba con una cookie llamada `voto`, la cual restringía los múltiples. Al no ser del tipo HttpOnly, se podía suprimir para poder realizar otro voto.
    2. Se identificó también el parametro enviado al formulario para identificar la opción `opUniversidad`.
    3. Se desarrolló y ejecutó un Script que automatiza el ciclo de borrado de la cookie y el envío de una petición POST con el parámetro de la universidad que se busca alterar el número de votación.
- **Resultados:**
    - **Parámetro de voto:** opUniversidad=1 (UTN).
    - **Estado:** Resuelto exitosamente.


---


### Laboratorio: Votación (Versión mejorada)
* **Vulnerabilidad:** Broken Access Control
* **Descripción:** La problemática es la misma, sólo que ahora también no se permite realizar multiples votaciones desde una misma IP.
* **Desarrollo:** 
    1. Se analizó el comportamiento del servidor ante la nueva restricción, la cual arrojaba un bloqueo al intentar emitir votos consecutivos desde una misma IP.
    2. Se identificó que el backend confiaba en cabeceras HTTP modificables del cliente (específicamente X-Forwarded-For y X-Real-IP) para registrar el origen de la conexión.
    3. Se desarrolló una versión mejorada del script anterior, la cual ahora también cambia la IP de la cabecera por una aleatoria por cada intento.
- **Resultados:**
    - **Parámetro de voto:** opUniversidad=1 (UTN)
    - **Estado:** Resuelto exitosamente.