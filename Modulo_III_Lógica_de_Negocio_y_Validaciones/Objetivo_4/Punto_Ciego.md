# Objetivo 4: El Punto Ciego de los Escáneres Automatizados

## ¿Por qué la lógica de negocio es el "punto ciego" de la automatización?

Los escáneres automatizados (Nessus, Acunetix, Burp) son excelentes detectando fallas técnicas genéricas (XSS, inyecciones, configuraciones débiles) porque buscan patrones conocidos en las respuestas del servidor. 

Sin embargo, fallan ante las fallas de lógica de negocio porque estas son únicas para cada aplicación. Un escáner no comprende el contexto, no sabe que un precio no puede ser negativo, que un cupón es de un solo uso, o que no se debe aprobar una compra sin saldo. Si el servidor responde con un `200 OK`, la herramienta asume que todo está bien, ignorando la intención real del flujo.

## ¿Por qué la auditoría manual es indispensable?

La auditoría manual es la única capaz de detectar estas fallas porque el humano aporta el contexto y el razonamiento que la máquina no tiene:

1.  **Comprensión del flujo:** Entiende el viaje del usuario (cómo se compra, se registra o se solicita un reembolso).
2.  **Creatividad:** Piensa escenarios fuera de la caja (ej. "¿Qué pasa si envío una cantidad negativa? ¿Y si uso el cupón en dos pestañas a la vez?").
3.  **Análisis de impacto:** Entiende el valor real de los datos y las consecuencias financieras de una alteración.