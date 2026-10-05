# Objetivo 1: Puertos y Servicios Esenciales

## Estos son los puertos fundamentales, con sus respectivos servicios por defecto:

### - Puerto 20: 
        Es usado para la transferencia de los archivos y directorios que son solicitados en el puerto 21.

### - Puerto 21:
        Se encarga de establecer la conexión con el servidor, así como también administrar la autenticación de las páginas.

### - Puerto 22:
        Permite el acceso y controlar servidores de forma remota a través de una terminal de comandos fuertemente cifrada y segura.

### - Puerto 23: 
        Lo mismo que el puerto 22, con la gran diferencia de que no encripta la información enviada, es muy peligrosa ya que si el tráfico de esa red es analizado, se puede ver la información enviada tal cuál.

### - Puerto 25:
        Es el protocolo estándar que se usa para el envío y enrutamiento de correos electrónicos entre servidores de mail a través de internet.

### - Puerto 53:
        Se encarga de la traducción de nombres de dominios a sus respectivas direcciones IP numéricas. Recibe el nombre del dominio ingresado por el usuario y devuelve la dirección IP que le corresponde.

### - Puerto 80:
        Se encarga de transmitir la información y recursos web en texto plano(Imágenes, textos y código de sitios) sin ningún tipo de cifrado, si se llega a interceptar se podrá ver la información exacta.

### - Puerto 110:
        Es el protocolo utilizado por los clientes de correo electrónico para conectarse a un servidor remoto, recibir y descargar los mensajes. Su comportamiento estándar es almacenar los correos en el dispositivo local y eliminarlos del servidor original.

### - Puerto 443:
        Es la versión segura del puerto 80, la información viaja cifrada y segura gracias a los certificados TLS.

### - Puerto 3306:
        Es el puerto encargado de la comunicación entre el backend y la Base de Datos, se encarga de recibir las conexiones y ejecutar las consultas que se envían desde el código.