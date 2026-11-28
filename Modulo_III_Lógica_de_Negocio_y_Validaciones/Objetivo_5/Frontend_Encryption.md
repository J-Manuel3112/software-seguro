# Objetivo 5: El Espejismo del Cifrado en el Cliente (Frontend Encryption)

## ¿Por qué es una falsa sensación de seguridad?

Se trata de una falsa sensación de seguridad porque confunde **ofuscación** con **seguridad real**.

El cifrado en el frontend solo hace que el tráfico sea ilegible a simple vista para alguien que mire el tráfico de red, no impide que un atacante modifique los datos. El atacante puede interceptar ese mensaje cifrado, espiar y robar información, volver a cifrarlo y hacer que continúe su camino. 

La seguridad real no depende de que el atacante no pueda leer el tráfico, sino de que el servidor valide correctamente lo que recibe, independientemente de cómo venga.

## El Fallo Fundamental de Diseño

El fallo está en confiar en el Frontend para implementar controles de seguridad. Cualquier lógica de seguridad (cifrado, validación, autenticación) que se ejecute en el navegador puede ser fácil o difícilmente descifrada, ya que el atacante tiene control total sobre el código que se ejecuta en su propia máquina. El cifrado en el cliente solo protege contra la curiosidad casual.

## Pasos Tácticos del Atacante

Un atacante puede evadir esta protección en tres fases:

### Fase 1: Ingeniería Inversa del Cifrado
1.  **Intercepción inicial:** El atacante usa Burp Suite para capturar una petición y observa que el body es un texto cifrado ilegible.
2.  **Análisis del código:** Usando la pestaña *Sources* de las Herramientas de Desarrollador (F12), localiza el archivo `.js` que contiene la función de cifrado.
3.  **Replicación:** Copia esa función de cifrado a la consola del navegador o a un script local para poder cifrar datos manualmente.

### Fase 2: Manipulación y Cifrado
1.  **Modificación en texto plano:** El atacante modifica los valores deseados en el objeto original.
2.  **Cifrado del payload:** Usando la función replicada, cifra el nuevo body manipulado.
3.  **Inyección en Burp Suite:** Pega el resultado cifrado en el campo correspondiente de la petición interceptada.

### Fase 3: Explotación
1.  **Envío:** El atacante envía la petición al servidor con el payload cifrado y manipulado.
2.  **Bypass:** El servidor recibe el payload, lo descifra con su clave privada y confía ciegamente en los datos porque vienen cifrados. Como el servidor no valida la integridad ni la autorización del dato descifrado, procesa la manipulación (ej. acredita los $10,000 al atacante).