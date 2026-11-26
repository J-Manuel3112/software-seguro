# Objetivo 3: Peligros del "Directory Listing"

## ¿Qué es el Directory Listing?

El Directory Listing es una funcionalidad que tienen los servidores web que, al recibir una petición HTTP `GET` hacia un directorio, devuelven una lista HTML con todos los archivos y subdirectorios que contiene.

Normalmente, cuando navegamos a cualquier sitio web en la dirección "/carpetas", el servidor busca un archivo de índice (como `index.html`, `index.php` o `index.htm`) y lo devuelve. Pero si ese archivo no existe y la directiva de listado está habilitada, el servidor, en lugar de devolver un error `403 Forbidden` o `404 Not Found`, **genera automáticamente un html con el árbol de archivos**.

## ¿Qué ocurre cuando un atacante navega a un directorio sin archivo índice?

El atacante no necesita usar fuerza bruta probando nombres de URLs. Sólo escribe la ruta del directorio en el navegador y el servidor 
 . La respuesta HTTP tendrá un código `200 OK` y un `Content-Type: text/html` con una tabla que enumera:

-   Los nombres de todos los archivos alojados.
-   Los nombres de los subdirectorios.
-   Los tamaños de los archivos (a veces).
-   Las fechas de última modificación.

Esto le permite al atacante **mapear la estructura interna de la aplicación** sigilosamente y encontrar archivos privados de importancia.

## ¿Qué tipo de archivos sensibles podrían quedar expuestos?

La lista es muy amplia, pero los más críticos son:

1.  **Archivos de respaldo (Backups):** Si un administrador dejó un respaldo de la base de datos en el servidor, el listado de directorios lo expone. Por ejemplo: `.sql`, `.zip`, `.tar.gz`, `.bak`, `.old`
2.  **Archivos de configuración:** Suelen contener credenciales de bases de datos, claves secretas de la aplicación o tokens de API. Por ejemplo: `.env`, `config.php`, `wp-config.php`, `.htaccess`.
3.  **Código fuente:** Archivos que no deberían ser accesibles desde el exterior (aunque el servidor los ejecute, si están en carpetas de respaldo con otras extensiones, pueden mostrarse en texto plano). Por ejemplo:  `.php`, `.js`, `.py`
4.  **Archivos temporales o de caché:** Los logs pueden contener rutas internas, usuarios, contraseñas o errores detallados. Por ejemplo: `.log`, `.tmp`.
5.  **Archivos de usuarios:** PDFs, imágenes, documentos que contienen datos personales.
6.  **Directorios ocultos:** El listado de un `.git` expuesto permite descargar todo el historial del código fuente. Por ejemplo carpetas como `/.git/`, `/.svn/`, `/admin/`, `/backups/`

## ¿Cómo se mitiga?

La mitigación depende del servidor web que estemos utilizando. Lo recomendable es **deshabilitar el listado de directorios por defecto** y obligar a que siempre haya un archivo índice. Si no lo hay, el servidor debe devolver un `403 Forbidden`.

