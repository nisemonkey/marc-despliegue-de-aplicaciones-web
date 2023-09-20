# Ejercicio 3

### Analiza los headers de las peticiones cuando inicias sesión en el Moodle y comprende 
cómo se obtiene el token. Para ello, necesitamos saber de dónde salen TODOS los 
datos sensibles que se envían.

Para ello hay que utilizar la herramienta de **inspeccionar** elementos que proporciona el navegador web. En cuanto se realiza el login me di cuenta de que se generan tres acciones en cascada.

![image](https://github.com/nisemonkey/marc-nombre-despliegue-de-aplicaciones-web/assets/144774706/61f67f10-db6f-45f8-83d9-1d9dab41bdad)

Aquí se pueden observar dos index y luego algo llamado "my". Lo más interesante es el primer index, ya que es el que enviará mis datos sensibles a la base de datos para realizar la comprobación empleando el método POST. 

![image](https://github.com/nisemonkey/marc-nombre-despliegue-de-aplicaciones-web/assets/144774706/51dac774-9875-44e7-b4ee-91a53290453c)

Por último, cuando se validan nuestros datos en el segundo index pasamos a la página principal del moodle, o "my/", donde ya se mostrarán todos los datos que van linkeados al usuario con el que inicié la sesión.

El token es, en este caso, un conjunto de datos encriptados que se envían posteriormente junto a la petición. En mi caso, obtuve este token:

![image](https://github.com/nisemonkey/marc-nombre-despliegue-de-aplicaciones-web/assets/144774706/3fb20e9d-dd96-4eae-95bd-82a14d4980c0)

Esto probablemente es un token de inicio de sesión que se utiliza para identificar y autenticar al usuario en la aplicación web. Su propósito principal es mejorar la seguridad y la experiencia del usuario al evitar que las credenciales de inicio de sesión se envíen en cada solicitud.


# Ejercicio 4

### ¿A qué puerto se reciben normalmente las peticiones del protocolo HTTP? ¿A qué 
capa del modelo TCP/IP se encuentra el protocolo HTTP? ¿Y los protocolos TCP, 
UDP, e IP?

- **HTTP**: recibe peticiones a través del puerto 80. Se encuentra en la cuarta capa del modelo TCP/IP, la capa de aplicación.
- **TCP**: trabaja en el tercer nivel, también conocido como la capa de transporte.
- **UDP**: trabaja en el tercer nivel, también conocido como la capa de transporte.
- **IP**: trabaja en el segundo nivel, también conocido como la capa de red.

# Ejercicio 5

### ¿Cuál es el significado de la siguiente respuesta de un servidor?
HTTP/1.1 302 Found 
Location: http://www.example.com/test/index2.php

El código de error 302 indica que el recurso solicitado ha sido desplazado temporalmente a otro URL, que es indicado justo debajo en "Location", donde puede verse la nueva ubicación del recurso.

