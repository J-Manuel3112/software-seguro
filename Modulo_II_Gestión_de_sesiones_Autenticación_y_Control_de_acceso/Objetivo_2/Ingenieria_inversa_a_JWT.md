# Objetivo 2: Ingeniería Inversa a JSON Web Tokens (JWT)

## 1. Origen y captura del token
- **Endpoint de prueba:** DummyJSON Authentication API (`POST https://dummyjson.com/auth/login`)
- **Método de interceptación:** se usó la Consola del navegador, realizando una petición `POST` de autenticación con un usuario de prueba propio de la web y decodificando los segmentos del JWT en el cliente.

    Nombre de prueba: "emilys"
    Contraseña de prueba: "emilyspass"

## 2. Estructura técnica del JWT

Un JWT estándar se compone de tres partes separadas por puntos (`.`): 
    **Header**: Define los metadatos del token, principalmente el algoritmo criptográfico usado para firmar (alg) y el tipo de token (typ).
    **Payload**: Contiene las claims (declaraciones o atributos sobre el sujeto/usuario y metadatos del ciclo de vida del token).
    **Signature**: firma criptográfica para verificar la integridad del token y comprobar que no fue alterado ni emitido sin la clave correspondiente.

JWT: eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpZCI6MSwidXNlcm5hbWUiOiJlbWlseXMiLCJlbWFpbCI6ImVtaWx5LmpvaG5zb25AeC5kdW1teWpzb24uY29tIiwiZmlyc3ROYW1lIjoiRW1pbHkiLCJsYXN0TmFtZSI6IkpvaG5zb24iLCJnZW5kZXIiOiJmZW1hbGUiLCJpbWFnZSI6Imh0dHBzOi8vZHVtbXlqc29uLmNvbS9pY29uL2VtaWx5cy8xMjgiLCJpYXQiOjE3OTU3MzEwMzcsImV4cCI6MTc5NTczNDYzN30.zs58_xt6l7K2DNKx--a84BHJo5--Cb_qEgNDbG6Nq9o

### Header (Encabezado)

- "alg": "HS256"
- "typ": "JWT"

### Payload Decodificado

- "id": 1,
- "username": "emilys",
- "email": "emily.johnson@x.dummyjson.com",
- "firstName": "Emily",
- "lastName": "Johnson",
- "gender": "female",
- "image": "[https://dummyjson.com/icon/emilys/128](https://dummyjson.com/icon/emilys/128)",
- "iat": 1795731037,
- "exp": 1795734637

## 3. ¿Qué algoritmo de firma se está utilizando en el Header?

Se utiliza HS256 (HMAC con SHA-256). Es un mecanismo de firma simétrica basado en un código de autenticación de mensajes con hash (HMAC) y SHA-256, utilizando una clave secreta compartida del lado del servidor para generar y validar la firma. Quien posea la clave secreta puede firmar tokens válidos, si la clave secreta es débil o se filtra, un atacante podría forjar tokens alterados.

## 4. ¿Qué datos sensibles o roles se están enviando en el Payload en texto plano?

- Codificación vs. cifrado: El Payload no está cifrado, está codificado en Base64URL. Cualquier persona que intercepte el token puede decodificarlo de inmediato y leer su contenido sin necesidad de conocer la clave secreta.

- Roles o permisos: En este payload en particular no se observan campos explícitos de rol o autorización (como role, roles, scope o permissions).

- Datos visibles / sensibles (PII e identificadores): Sí se exponen en texto plano:

    * id: identificador numérico de usuario.

    * username (emilys): nombre de usuario.

    * email (emily.johnson@x.dummyjson.com): correo electrónico.

    * firstName y lastName (Emily, Johnson): nombre y apellido.

    * gender (female)

    - iat (1795731037) y exp (1795734637): marcas de tiempo Unix de emisión (Issued At) y expiración (Expiration Time).