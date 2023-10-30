# Administración de servidores web

## Servidores virtuales de Apache

En esta ocasión habrá que crear dos servidores virtuales que, en mi caso, alojaré en una máquina virtual Ubuntu Server. El proceso será realizado en local, haciendo uso del adaptador puente y contando con el dhcp4 que provee el recinto escolar.

### Preparaciones

Empezaré creando un Ubuntu Server totalmente nuevo, ya que el que había empleado en otras ocasiones ya estaba bastante trasteado y no me hacía especial ilusión manipularlo más.
- `sudo apt update`
- `sudo apt install apache2 php libapache2-mod-php -y`
- `sudo a2enmod rewrite`
- `sudo systemctl restart apache2`

Para crear los servidores virtuales, primero crearé la raíz de cada uno y posteriormente copiaré el fichero de configuración que nos da apache2 para poder darle la dirección que yo quiera.
- `sudo mkdir -p /var/www/daw`
- `sudo mkdir -p /var/www/mlopezg`
- `sudo cp /etc/apache2/sites-available/000-default.conf /etc/apache2/sites-available/daw.conf`
- `sudo cp /etc/apache2/sites-available/000-default.conf /etc/apache2/sites-available/mlopezg.conf`

Dentro de las raíces /var/www/daw y /var/www/mlopezg he creado un index.html y una hoja de estilo para que la página se vea "bonita". El resultado no decepcionará.

### Configurar los archivos

Ahora que ya he copiado los fichero de configuración, tocará modificarlos. Concretamente quiero tocar el server name y su raíz. Este es el resultado:

![image](https://github.com/nisemonkey/marc-despliegue-de-aplicaciones-web/assets/144774706/758c1f67-36d1-4c3d-8b5a-83a7ec2af834)

![image](https://github.com/nisemonkey/marc-despliegue-de-aplicaciones-web/assets/144774706/d2472d85-a63f-49cd-9a71-0f56a5da08d5)

Con todo eso listo, podemos proceder al último paso necesario dentro de esta máquina.

- `sudo a2ensite daw.conf`
- `sudo a2ensite mlopezg.conf`
- `sudo systemctl restart apache2`
- Adicionalmente, es aconsejable asegurarse de que en la carpeta /etc/hosts tengamos nuestra dirección IP junto al server name de ambos servidores virtuales.

### Configurar "cliente"

Simplemente hay que crear una máquina virtual de ubuntu desktop, aunque podría ser windows, y configurar su dirección IP. Cabe recordar que hay que poner ambas máquinas con el mismo adaptador red (adaptador puente). Lo único que hay que tocar en cuanto a la IP es desactivar la distribución automática de DNS y allí poner la IP que apunta a los servidores virtuales.

![image](https://github.com/nisemonkey/marc-despliegue-de-aplicaciones-web/assets/144774706/9ec1fb79-2b35-4e22-94d8-5b6ea6cd4b4c)

Finalmente, entramos al fichero de hosts y añadimos lo mismo que antes:

![image](https://github.com/nisemonkey/marc-despliegue-de-aplicaciones-web/assets/144774706/e1f38cdd-47a4-4c27-9bd3-e43ac97f7727)

### Comprobación

Sas llegado aquí, ¡felicidades! El último paso es abrir el navegador e introducir la dirección de cualquiera de los dos servers virtuales y ver el espectacular resultado.

![image](https://github.com/nisemonkey/marc-despliegue-de-aplicaciones-web/assets/144774706/49b2c79a-8862-4c3c-934d-b16f804ad37a)

![image](https://github.com/nisemonkey/marc-despliegue-de-aplicaciones-web/assets/144774706/1e17e8bd-7959-4603-bb99-1616bd94b986)


