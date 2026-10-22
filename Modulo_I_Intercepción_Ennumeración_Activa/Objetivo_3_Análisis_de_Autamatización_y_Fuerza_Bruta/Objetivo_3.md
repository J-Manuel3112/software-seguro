# Objetivo 3: Análisis de Automatización y Fuerza Bruta

## 1. Diferencias entre Ataques de Automatización


* **Ataque de Fuerza Bruta Tradicional:** 
    Se basa en el intento de cada una de las combinaciones posibles una tras otra de manera sucesiva hasta dar con la indicada.

* **Ataque de Diccionario:** 
    Se basa en usar Listas predefinidas de combinaciones de letras usadas frecuentemente como contraseñas filtradas en internet.

* **Diferencia:** 
    El ataque de fuerza Bruta tradicional usa una forma mucho más primitiva para lograr el mismo objetivo, ya que al intentar cada combinación existente podría tardar años en descifrar una contraseña. Sin embargo, el ataque de diccionario, inicia los intentos con las combinaciones más usuales de contraseñas, haciendo más rápido e inteligente el proceso.

---

## 2. Mecanismos de Defensa 

* **CAPTCHA o reCAPTCHA:** 
    Desafíos visuales o lógicos que obligan al usuario a realizar para poder demostrar que realmente son humanos y no un bot de automatización.

* **Rate Limiting:** 
    Cada cierta cantidad de intentos fallidos en menos de un minuto, el servidor bloquea el acceso a más intentos temporalmente, en algunos casos va en escala, el tiempo aumenta más si se vuelven a realizar intentos fallidos.