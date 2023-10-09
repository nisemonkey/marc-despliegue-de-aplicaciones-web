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

### Crear carpetas públicas y privadas

Como puede observarse en estas dos imágenes, he creado dos carpetas en mi directorio raíz. Una me prohíbe el acceso mientras que la otra me deja entrar.

![image](https://github.com/nisemonkey/marc-despliegue-de-aplicaciones-web/assets/144774706/7b46c2e0-ab77-43b4-9546-9857b18f5cad)

![image](https://github.com/nisemonkey/marc-despliegue-de-aplicaciones-web/assets/144774706/a30888b5-d3ce-4efc-b8c5-633c4141437b)

Para ello, he editado el fichero "apache2.conf" y he agregado esta configuración para cada uno de los directorios en cuestión:

![image](https://github.com/nisemonkey/marc-despliegue-de-aplicaciones-web/assets/144774706/98c5724c-f351-4364-8551-1c0e28c60d12)

Mi conclusión es que con todo radica en lo que escribas dentro del campo "Require". Si es "all granted", habrá acceso; si no, no se podrá ver nada

### Añadir www con mod_rewrite y .htaccess

Esto por ahora mama bastante porque no me va

### Personalizar tema de los directorios

Para ello habrá que instalar git ``sudo apt install git`` y luego clonar el repositorio que queremos:

- ``git clone https://github.com/ramlmn/Apache-Directory-Listing.git /var/www/html/cfg``

Una vez quede guardado ahí, habrá que coger la carpeta "directory-listing" y "htaccess.txt". Deberemos renombrar la segunda a ".htaccess" y cambiar unos cuantos parámetros tal y como se comenta en el readme del repositorio. Este es el resultado final en mi caso (utilicé grid-darkmode.css).

![image](https://github.com/nisemonkey/marc-despliegue-de-aplicaciones-web/assets/144774706/9ea8e6b8-9e4b-43bd-a03f-bbc71a98f331)





