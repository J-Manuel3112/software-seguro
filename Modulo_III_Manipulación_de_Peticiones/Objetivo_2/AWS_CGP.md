# Objetivo 2: SSRF y el Peligro en la Nube (AWS/GCP)

## ¿qué sucede si un atacante encuentra un SSRF en una aplicación alojada en Amazon Web Services (AWS) y fuerza al servidor a hacer una petición GET a la dirección mágica 169.254.169.254.?

Esa IP es el servicio de metadatos de la instancia (IMDS). Es una dirección interna que solo responde desde dentro de la instancia EC2 y proporciona información sobre su configuración. Si el atacante logra que el servidor haga esa petición (vía SSRF), el servidor responde con información interna crítica que nunca debería ser expuesta al exterior. O sea, básicamente el atacante presta el servidor para preguntarle a AWS cuáles son sus credenciales.

## ¿Qué información altamente sensible espera extraer el atacante?

El atacante busca las **credenciales temporales de IAM** de la instancia. Concretamente, extrae:

*   **`AccessKeyId`**: El identificador de la clave.
*   **`SecretAccessKey`**: La clave secreta.
*   **`Token`**: El token de sesión.

Con estas credenciales, el atacante puede autenticarse en la API de AWS y realizar acciones con los mismos permisos que la instancia, como leer archivos (S3), crear usuarios o modificar la infraestructura.