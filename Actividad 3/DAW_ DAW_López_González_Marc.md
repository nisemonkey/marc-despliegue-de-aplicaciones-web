# Administración de servidores web

## Módulos de Apache 

Empezaré insertando el módulo "mod_info" a mi servidor de apache. Se encarga de proporcionar información detallada sobre el estado del server y su configuración. Básicamente, monitorea todo el rendimiento del sistema e intenta solucionar problemas si aparecen.

![image](https://github.com/nisemonkey/marc-despliegue-de-aplicaciones-web/assets/144774706/f80860f2-f279-4eff-a708-8089ad14825e)

Tendré que hacer un pas extra para poder visualizar esta información desde el navegador de mi PC (si tuviera Ubuntu Desktop podría hacerlo localmente, pero la gracia es qu pueda verlo). Para ello, me di permiso a mi dirección IP para poder acceder a esta información en la ruta "/etc/apache2/mods-enabled/info.conf

![image](https://github.com/nisemonkey/marc-despliegue-de-aplicaciones-web/assets/144774706/2225ffec-da0e-4028-b6e3-a1e2765adaf7)

Ahora ocultaremos la versión de ubuntu y apache para que no suponga una vulnerabilidad. Para ello hay que cambiar/agregar los siguientes parámetros:

![image](https://github.com/nisemonkey/marc-despliegue-de-aplicaciones-web/assets/144774706/3760f005-5a90-4fed-b843-9def5eef9dda)

De esta forma, cuando visitemos una carpeta en vez de un recurso, aparecerá esto:

![image](https://github.com/nisemonkey/marc-despliegue-de-aplicaciones-web/assets/144774706/907dc80c-f35e-4860-a228-f938e7d44881)

### Añadir www con mod_rewrite y .htaccess

Empezaré creando el fichero .htaccess en "/var/www/html/" y pondré los siguientes parámetros:
RewriteEngine On
RewriteCond %{HTTP_HOST} !^www\. [NC]
RewriteRule ^(.*)$ http://www.%{HTTP_HOST}/$1 [R=301,L]




