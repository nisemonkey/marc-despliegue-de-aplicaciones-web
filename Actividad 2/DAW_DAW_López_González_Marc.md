#  Administración de servidores web

## Creación de una máquina Ubuntu e instalación de Apache

Empezamos con la instalación de un Ubuntu Server.

![image](https://github.com/nisemonkey/marc-despliegue-de-aplicaciones-web/assets/144774706/9fb29491-ab21-4dae-83be-59589282cbb7)

Hecho esto, pasé al paso número dos. Instalar apache y todos sus recursos desde la terminal que proporciona la propia interfaz.

![image](https://github.com/nisemonkey/marc-despliegue-de-aplicaciones-web/assets/144774706/4d7fac5e-27d5-49cf-b25c-f02952fa2592)

Por último, le daré acceso a los puertos por defecto con un pequeño ajuste dentro del firewall.

![image](https://github.com/nisemonkey/marc-despliegue-de-aplicaciones-web/assets/144774706/f9b7c95e-a1bc-449d-a49c-6117e4815f76)

## Entendiendo algunos conceptos

- GET: aquí estás haciendo una solicitud HTTP para solicitar datos de un recurso específico. Es decir, le pides al servidor que te dé cierta información. Por ejemplo, poner una URL en el navegador y darle a "enter" es una solicitud GET.
- POST: este es otra solicitud HTTP pero que se utiliza para enviar datos y crear un recurso. Es decir, sería cuando introduces tus credenciales en un formulario o algo así. Total, que sirve para datos sensibles.
- PUT: este método de solicitud HTTP se utiliza para actualizar o crear un recurso nuevo o existente. A simple vista parece que PUT y POST hacen lo mismo, pero PUT se centra en actualizar, mientras que POST en crear.
- DELETE: otro método de solicitud HTTP. Le pides al server que elimine un recurso en específico que se identifica en la URL. Al acabar la solicitud, ese recurso ya no existirá en el servidor.

Al final, cada uno de estos métodos tiene un propósito específico y sus diferencias son claras.

## Despliegue y configuración de apache2

Antes de empezar a toquetear apache2, vamos a comprobar si está activo o qué.

![image](https://github.com/nisemonkey/marc-despliegue-de-aplicaciones-web/assets/144774706/3ec3b708-6f79-49ba-b1f1-a15ef88ee3d3)

Ahora que sabemos que exite, hacemos un hostname -I para saber cómo acceder y escribimos el resultado en el navegador. Si está la máquina bien configurada (el adaptador de red está bien colocado), este será el resultado:

![image](https://github.com/nisemonkey/marc-despliegue-de-aplicaciones-web/assets/144774706/c7f3cea4-075f-4325-871a-b1540177c848)

Ahora haré un ajuste para cambiar el puerto empleado del 80 al 4444. Para ello, hay que acceder a la ruta "/etc/apache2/ports.conf", editar el puerto deseado, acceder a "/etc/apache2/sites-enabled/000-default.conf" e insertar nuestro puerto y reiniciar el servidor apache con la instrucción ``sudo service apache2 restart``.

![image](https://github.com/nisemonkey/marc-despliegue-de-aplicaciones-web/assets/144774706/4283b2bc-45b2-4376-9d93-86f41ac46a3f)

![image](https://github.com/nisemonkey/marc-despliegue-de-aplicaciones-web/assets/144774706/f6e8f7de-d3f9-4c31-83f8-bd310d8cf973)

Si todo funciona, desde la propia terminal podemos hacer una comprobación:

![image](https://github.com/nisemonkey/marc-despliegue-de-aplicaciones-web/assets/144774706/54a21022-e6eb-4dfa-b6b3-af58821d7d4c)

Si no, ingresamos a la web con http://x.x.x.x:4444" y nos debería dejar.

![image](https://github.com/nisemonkey/marc-despliegue-de-aplicaciones-web/assets/144774706/993b72e6-34e4-49c3-afd8-373cfc8d3096)

## Creación e integración de SSL

Para empezar, primero hay que comprobar/activar el soporte SSL con la instrucción ``sudo a2enmod ssl`` y hacer un restart del servicio para que se habilite. Ahora procederé a crearme la llave y el certificado con la instrucción ``sudo openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout /etc/ssl/private/apache-selfsigned.key -out /etc/ssl/certs/apache-selfsigned.crt``. Una vez esté todo esto creado, hay que ir al fichero de conf donde pusimos previamente nuestro puerto preferido (4444) y añadir lo siguiente:

![image](https://github.com/nisemonkey/marc-despliegue-de-aplicaciones-web/assets/144774706/4520e8d6-184b-4bd3-84b8-61e72730ca9f)

Si ahora recargo la página del server (recordar que estaba usando la URL "**http**://x.x.x.x:4444") me aparecerá un error indicando que la solicitud enviada no puede ser entendida porque se está utilizando un puerto SSL.

![image](https://github.com/nisemonkey/marc-despliegue-de-aplicaciones-web/assets/144774706/ad101a3a-6337-497e-b50f-c9067ca8c2ec)

A mi navegador la de miedo la página pero funciona correctamente:

![image](https://github.com/nisemonkey/marc-despliegue-de-aplicaciones-web/assets/144774706/0e2a2964-64b1-4ccf-be06-9e8d966a1a92)

