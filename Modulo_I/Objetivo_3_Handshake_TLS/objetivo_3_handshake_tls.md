
## 1. ¿Qué es el TLS Handshake?
El "Handshake" (o apretón de manos) es el paso que ocurre antes de que el navegador mande la primera petición HTTP. Es el momento de negociación entre el cliente y el servidor, en donde se acuerdan los parámetros criptográficos, Este proceso autentica al servidor y permite que la conexión sea segura. Sin esta interacción, no existiría protección de datos y todo iría en texto plano aumentando el riesgo a un ataque.

## 2. ¿Cómo funciona paso a paso?
1. **Cliente:** Empieza enviando un paquete con la versión de TLS que usa, un número aleatorio que sirve como token y una lista de los métodos de cifrado que soporta.
2. **Servidor:** El servidor lee la lista y escoge el método más potente que compartan, responde diciendo que se va a usar. Además envía su propio número aleatorio junto con su certificado digital.
3. **Intercambio de llaves:** Usando la información de ese certificado, el cliente verifica la veracidad del certificado, genera un paquete de datos, lo encripta para que nadie en el medio lo pueda leer y se lo manda al servidor. Solo el servidor real tiene la capacidad de desencriptarlo.
4. **Conexión lista:** Usando los datos de esa interacción, junto con los números aleatorios, cada parte hace cálculos independientes hasta que ambos tengan la misma clave de sesión. Después, se vuelven a enviar datos con esta nueva clave, si logran descifrarla y leer el mensaje del otro, termina la negociación y empíeza el intercambio HTTP normal.

## 3. Certificados Digitales
El certificado juega el rol de identificación del servidor. Sirve para demostrar que es un servidor legítimo y no una página falsa. 
Si me conecto a un banco desde una red wifi pública, este certificado me ayuda a saber si la conexión es legítima y segura o es un atacante que imita la interfaz del banco para robar mis credenciales. 

## 4. ¿Por qué se usan dos tipos de cifrado a la vez?
En una misma conexión HTTPS usamos ambos tipos de cifrado (asimétrico y simétrico) porque cada uno tiene sus ventajas y desventajas, y juntos se complementan perfecto:

*   **Cifrado Asimétrico:** Es muy seguro para intercambiar datos, pero es un método muy lento y bastante complejo. Por eso, sólo la usamos al principio de la negociación, para poder pasar la clave secreta sin que nadie la intercepte. Si se usa para encriptar una página completa el proceso tardaría muchísimo.

*   **Cifrado Simétrico:** Es mucho más rápido y eficiente. El único defecto es que tanto el cliente como el servidor tienen que tener la misma clave de acceso la cual es muy importante y requiere muchísima seguridad. Como en el paso anterior ya nos pasamos la llave secreta de forma segura, ahora podemos usar este método más rápido para cifrar todo el resto del intercambio (las páginas, las contraseñas, las imágenes). Así logramos que la web cargue rápido sin perder seguridad.