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

## Información sobre Apache2

### Ficheros de configuración

Los ficheros de configuración principal de Apache2 se encuentran en el directorio /etc/apache2/. Algunos de los ficheros clave incluyen:

- apache2.conf: Este archivo sirve como el principal punto de configuración de Apache2. Contiene ajustes globales para el servidor web, como el puerto de escucha, los directorios raíz y las configuraciones de los módulos.

El archivo apache2.conf contiene varias secciones que definen la configuración global del servidor. Algunas secciones principales incluyen:

- Configuración Global: Aquí se encuentran configuraciones generales que afectan a todo el servidor, como el puerto, el usuario y grupo bajo los cuales se ejecuta Apache, y la configuración del servidor DNS.

- Directivas de Configuración: Este es el lugar donde puedes agregar directivas específicas del servidor, como configuraciones para módulos adicionales o ajustes de seguridad específicos para el servidor.

Directorios sites-available y sites-enabled:
- sites-available: Este directorio contiene archivos de configuración de los sitios web disponibles. Cada archivo representa un sitio web específico que Apache puede servir. Es importante tener en cuenta que estos sitios no están activos por defecto.

- sites-enabled: En este directorio, se encuentran enlaces simbólicos a los archivos de configuración de los sitios web que están activos. Apache2 solo lee los archivos de configuración de los sitios web que están enlazados en este directorio. Esto permite habilitar o deshabilitar sitios web sin necesidad de eliminar o mover los archivos de configuración.

Directorios mods-available y mods-enabled:
- mods-available: En este directorio se encuentran los archivos de configuración de los módulos de Apache que están disponibles para ser cargados. Cada archivo representa un módulo específico y contiene configuraciones relacionadas con ese módulo.

- mods-enabled: Similar a sites-enabled, este directorio contiene enlaces simbólicos a los archivos de configuración de los módulos que están activos. Los módulos que se cargan en el servidor Apache se definen mediante estos enlaces simbólicos. Habilitar o deshabilitar módulos es tan simple como crear o eliminar enlaces en este directorio, respectivamente.

![image](https://github.com/nisemonkey/marc-despliegue-de-aplicaciones-web/assets/144774706/aced311a-c6ad-4a65-920e-d8bb5f5f2280)

### Ficheros de ejecución

El binario principal de Apache2 se encuentra en /usr/sbin/apache2. Este binario se utiliza para iniciar, detener, recargar y reiniciar el servidor Apache2, así como para realizar otras operaciones relacionadas con el servicio.

![image](https://github.com/nisemonkey/marc-despliegue-de-aplicaciones-web/assets/144774706/b3796ab3-c4d1-4a43-b767-8d8833b27264)

Instrucciones:

- Iniciar: simplemente inicia el servicio de apache. ``sudo service apache2 start``
- Detener: detiene el servicio de apache. ``sudo service apache2 stop``
- Recargar: recarga la configuración del servidor Apache2 sin detener el servicio por completo. ``sudo service apache2 reload``
- Reiniciar: lo detiene y luego lo vuelve a iniciar. ``sudo service apache2 restart``

Para verificar la sintaxis de la configuración se utiliza la instrucción ``sudo apache2ctl configtest``

![image](https://github.com/nisemonkey/marc-despliegue-de-aplicaciones-web/assets/144774706/32cb8a62-7233-4178-885d-0e43d9cb9080)

### Ficheros de monitorización

La ubicación de estos ficheros, también conocidos como logs, se encuentra en "/var/log/apache2/"

![image](https://github.com/nisemonkey/marc-despliegue-de-aplicaciones-web/assets/144774706/e170a7eb-638a-4d6d-8a54-bb0cd0511a3a)

- error.log: Este archivo registra los mensajes de error generados por el servidor Apache2. Contiene información detallada sobre errores, advertencias y problemas que ocurren durante la operación del servidor.

- access.log: Este archivo registra detalles sobre cada solicitud que el servidor Apache2 recibe. Incluye información sobre las direcciones IP de los clientes, las páginas solicitadas, los códigos de respuesta del servidor y otros detalles relacionados con las solicitudes HTTP.

La rotación de logs sirve para que no haya demasiados archivos de registro y el disco explote. Se utiliza el sistema de rotación del sistema operativo, y su configuración se encuentra en "/etc/logrotate.d/apache2".

La rotación de logs se configura para comprimir y archivar los logs antiguos, manteniendo solo una cantidad específica de archivos de registro recientes para su análisis.

![image](https://github.com/nisemonkey/marc-despliegue-de-aplicaciones-web/assets/144774706/205acfcf-d098-4275-842f-2852eb401d8a)

Para monitorear en tiempo real, hice un ``tail -f`` de los dos ficheros de log.

![image](https://github.com/nisemonkey/marc-despliegue-de-aplicaciones-web/assets/144774706/7afd7925-1f21-4524-aa4b-92c14d3d0f82)

Para obtener un informe detallado, realizaré la siguiente instalación. Usaré una herramienta llamada "goaccess" siguiendo los siguientes pasos:

- ``sudo apt-get update``
- ``sudo apt-get install goaccess``
- ``goaccess /var/log/apache2/access.log``

Este es el resultado:

![image](https://github.com/nisemonkey/marc-despliegue-de-aplicaciones-web/assets/144774706/c0add930-9426-45f1-b506-0ab30b463477)

## Entendiendo conceptos

### Firewall

Un firewall es esencial porque:

- Protege contra Amenazas: Nos ayuda a proteger el dispositivo de malware, hackers y DoS.
- Controla el Tráfico: Sirve para poder monitorear y restringir el tráfico de red a gusto.
- Seguridad en Capas: Un firewall no deja de ser una capa de seguridad y, por eso mismo, es tan importante, ya que al combinarse con otros medios de seguridad se vuelve poderosa.

Para la instalación y configuración, seguí los siguientes pasos (en mi caso ya lo tuve instalado, pero igualmente conviene actualizar de vez en cuando):
- ``sudo apt-get update``
- ``sudo apt-get install ufw``
- ``sudo ufw allow http``
- ``sudo ufw allow https``
- ``sudo ufw deny from any``
De esta forma le estoy diciendo al firewall que solamente permita tráfico desde el puerto http y https, bloqueando el resto.

![image](https://github.com/nisemonkey/marc-despliegue-de-aplicaciones-web/assets/144774706/02834fe9-f793-4645-bfdc-ee330f401029)

### URL

Una URL es una dirección que se utiliza para acceder a un recurso web. Puede ser una página web, una imagen, un vídeo o cualquier tipo de archivo. Tiene varias partes:

- Protocolo: Define el método de acceso. Hasta donde he visto, he trabajado con "http", un protocolo no seguro, y "https" que es un protocolo cifrado con SSL/TLS y por ende es seguro. También existen otros protocolos como "ftp" (file transfer protocol) para transferir archivos y "mailto" para correo electrónico.

- Dominio: El dominio es la dirección del sitio web. Es básicamente lo que vemos después del "https://"; "www.ejemplo.com" sería un dominio, donde "ejemplo" es el nombre y ".com" (puede ser .org, .net) es el dominio de nivel superior.

- Subdominio: Algunas URLs pueden tener un subdominio que precede al dominio principal. Por ejemplo, en "https://blog.ejemplo.com", "blog" es el subdominio.

- Ruta: La ruta especifica la ubicación específica del recurso en el servidor web. En "https://www.ejemplo.com/blog/articulo1", "/blog/articulo1" es la ruta. La ruta a menudo corresponde a una estructura de carpetas en el servidor web. La ruta en concreto quizá hace referencia a una página nueva o a algún tipo de recurso.

- Parámetros: Los parámetros permiten pasar información adicional a la página web. Por ejemplo, en "https://www.ejemplo.com/buscar?q=ejemplo", "?q=ejemplo" son los parámetros. Los parámetros están separados del resto de la URL por un signo de interrogación (?).

- Fragmento: El fragmento permite enlazar a una sección específica de una página web. Por ejemplo, en "https://www.ejemplo.com/pagina#seccion", "#seccion" es el fragmento. Los fragmentos están separados del resto de la URL por un signo de numeral (#).

### Funcionamiento del protocolo HTTP

El protocolo HTTP es el protocolo de comunicación estándar de la triple W. Su funcionamiento es más o menos el siguiente:

- Solicitud: aquí es cuando el usuario entra en la dirección web. El navegador crea una solicitud HTTP que contiene el tipo de solicitud (GET o POST), la URL y otros detalles.
- Envío de la solicitud: aquí simplemente se realiza el envío de la solicitud hasta alcanzar el servidor.
- Procesamiento dle servidor: Dependiendo del tipo de solicitud, el servidor puede buscar archivos, ejecutar scripts, o realizar otras tareas. Además, mandará el clásico código de estado en función del resultado de dicho procesamiento, que podría ser perfectamente el clásico 404 y también enviará los datos solicitados (si no da errores vaya).
- Envío de la respuesta: aquí el servidor le devuelve al usuario lo que estaba solicitando. Se terminará cuando alcance al usuario.

A partir de aquí, el navegador del cliente se encarga de interpretar la información que acaba de recibir. Si es una página, tendrá que interpretar todo su código html y todos sus recursos adicionales, los cuales se consiguen haciendo peticiones a parte al servidor.

### El archivo .htaccess

Es un archivo de configuración para servidores web basados en apache. Garantiza control sobre varias configuraciones y características del servidor para directorios específicos sin toquetear la configuración del servidor principal. Son especialmente útiles para configuraciones de seguridad, reescritura de URL, redirecciones y restricciones de acceso.

Para reescribir una URL, aquí un ejemplo. Quiero convertir https://www.ejemplo.com/producto.php?id=123 en https://www.ejemplo.com/producto/123:

RewriteEngine On -> Activa el motor de reescritura.
RewriteRule ^producto/([0-9]+)$ producto.php?id=$1 [L] -> Especifica una regla que cambiará el URL a lo que quiero.

Para restringir acceso, aquí un ejemplo:

Order Deny,Allow
Deny from all
Allow from xxx.xxx.xxx.xxx

Aquí básicamente estoy dándole la instrucción de que quieor que solamente se pueda acceder a una página desde la IP que indico en el "Allow".







