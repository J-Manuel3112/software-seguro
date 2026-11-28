# Objetivo 2: Análisis de Manipulación Transaccional (Cantidades Negativas)

## Explicación Técnica del Fallo

Esta vulnerabilidad ocurre por una falla en la lógica de negocio y una falta de validación de entrada (Input Validation) del lado del servidor.

El servidor confía ciegamente en los datos numéricos enviados por el cliente a través de la petición HTTP. Cuando el atacante intercepta la petición y cambia la cantidad de `1` a `-5`, el backend realiza el cálculo matemático de forma automática sin detenerse a verificar la coherencia del número. Al obtener un total negativo, la lógica del sistema interpreta ese número negativo como una devolución o un abono. En lugar de cobrarle al atacante, el sistema le acredita a su cuenta.

## ¿Cómo debería validar el desarrollador este campo?

El desarrollador debe entender que la validación debe ser estricta y realizarse siempre en el Backend, nunca solo en el Frontend, ya que este último es fácilmente evadible.

Las validaciones que debe implementar el servidor son:

1.  **Validación de tipo de dato:** Asegurar que el valor recibido sea un número entero y no un string o un decimal.
2.  **Validación de rango:** Establecer un límite mínimo y máximo. La cantidad debe ser `>= 1` (o el mínimo permitido por stock) y `<=` el stock disponible. Si el valor es `-5`, el servidor debe rechazar la petición inmediatamente con un error `400 Bad Request`.
3.  **Recálculo del precio en el servidor:** El backend nunca debe calcular el total basándose en un precio unitario que envíe el cliente. Debe buscar el precio del producto en su propia base de datos y multiplicarlo por la cantidad validada.
4.  **Verificación de stock:** Antes de procesar la compra, el servidor debe verificar si hay stock suficiente para la cantidad solicitada. Una cantidad negativa rompe esta lógica.

**En resumen:** El servidor debe implementar una lista blanca (whitelist) de valores permitidos o una validación estricta de rangos numéricos antes de ejecutar cualquier operación matemática o de base de datos.