# Objetivo 1: El Arte de la Inyección Ciega (Blind SQLi)

## Diferencia entre Boolean-Based Blind y Time-Based Blind

Son dos técnicas que permiten al atacante extraer información sin verla, la diferencia es que usan canales distintos para lograrlo:

**Boolean-Based Blind:** Responde según la condición inyectada. Si la condición es verdadera puede mostrar o no los resultados, Si la condición es falsa directamente no los muestra.

**Time-Based Blind:** Responde igual, lo que varía es el tiempo de respuesta. Si la condición es verdadera, se inyecta un retraso de 5 segundos, pero si es falsa, responde automaticamente.

---

## ¿Cómo logra un atacante extraer el nombre de una base de datos letra por letra?

Aunque la pantalla solo diga "Cargando...", el atacante empieza a adivinar por preguntas:

1.  **Longitud:** Pregunta si el nombre tiene X letras (ej. "¿es mayor a 5?").
2.  **Primera letra:** Pregunta si la primera letra es 'a', 'b', 'c'... hasta acertar.
3.  **Optimización:** Usa comparaciones mayores/menores (ej. "¿es mayor a 'm'?") para reducir intentos.
4.  **Repetición:** Repite el proceso para cada letra hasta reconstruir el nombre completo.
