# Servidor Web

Nota: por defecto firewalld está instalado por defecto en Alma Linux

Firewalld es un servicio dinámico de muro cortafuegos que provee una administración dinámica con soporte para zonas y asignación de niveles de confianza a éstas últimas. Tiene soporte para IPv4 e IPv6 así como también dispositivos puente tipo Ethernet.

## Ver la lista de zonas de firewalld
```
firewall-cmd --get-zones
```

## Verificar cuál es la zona utilizada por firewalld:
```
firewall-cmd --get-default-zone
```

## Cambiar la zona predeterminada
```
firewall-cmd --set-default-zone=public
```

## Añadir puerto a firewall
```
firewall-cmd --zone=public --add-port=10000/tcp
firewall-cmd --permanent --zone=public --add-service=http
```

## Lista de servicios habilitados en la zona actual
```
firewall-cmd --list-services
```

## Aplicar el cambio es necesario volver a cargar la configuración
```
firewall-cmd --reload
```

# Instalación nginx
```
dnf update && dnf install nginx
```

## ¿Qué es systemd?
Systemd es un paquete de software que actúa como administrador de sistemas y servicios para Linux. Es el primer proceso que se inicia al encender el sistema y el último en cerrarlo. 

### Systemd tiene las siguientes características: 
- Inicia el resto del sistema 
- Mantiene puntos de montaje y montaje automático 
- Es compatible con instantáneas 
- Realiza un seguimiento de los procesos mediante grupos de control de Linux 
- Utiliza activación por socket y D-Bus para iniciar servicios 
- Ofrece inicio de daemons a pedido 
- Unifica la configuración y el comportamiento de los servicios en todas las distribuciones de Linux 
- Systemd es el sistema de inicio predeterminado en muchas distribuciones de Linux, como Ubuntu, Red Hat Enterprise Linux, CentOS y Fedora.

## ¿Qué es systemctl?
Systemctl es una herramienta de línea de comandos que permite administrar el sistema systemd y los servicios en Linux. Con ella, es posible controlar y administrar las unidades y servicios del sistema, mediante comandos para: Iniciar, Detener, Reiniciar, Verificar el estado. 

```
systemctl start nginx
systemctl enable nginx
systemctl status nginx
```

### Verificar versión de nginx
```
nginx -v
```

### Realizar copia de seguridad de nginx.conf
```
cp nginx.conf nginx.conf.back.date.user
```

### Crear directorios para administrar los bloques del servidor.
```
mkdir /etc/nginx/sites-available
mkdir /etc/nginx/sites-enabled
```

### Dentro del bloque HTTP, agregue las dos líneas siguientes:
```
include /etc/nginx/sites-enabled/*.conf;
server_names_hash_bucket_size 64;
```

### Crear virtual host
```
server {
    listen 8080;
    server_name _;
    root /var/www/najimi.com;

    index index.html index.htm index.php;

    location / {
        try_files $uri $uri/ =404;
    }

    location ~ /\.ht {
        deny all;
    }

}
```

## SElinux

SELinux define los controles de acceso para las aplicaciones, los procesos y los archivos de un sistema. Utiliza políticas de seguridad, que son un conjunto de reglas que le indican a SELinux los recursos a los que puede acceder y a los que no.

### Permisos a carpeta de directorio
```
chcon -R -t httpd_sys_content_t /var/www/pandora.com
```

## Instalar repositorio de php
```
dnf config-manager --set-enabled crb
dnf install https://dl.fedoraproject.org/pub/epel/epel{,-next}-release-latest-9.noarch.rpm
dnf install https://rpms.remirepo.net/enterprise/remi-release-9.rpm
```

## Reiniciar configuración de php e instalar la versión 8.2
```
dnf module reset php
dnf module enable php:remi-8.2
dnf install php php-cli php-fpm php-mysqlnd
```

## Instalar versión de php 7.4
```
dnf install php74 php74-php-cli php74-php-fpm php74-php-mysqlnd
```

## Habilitar servicios de php-fpm

PHP-FPM (FastCGI Process Manager) es una herramienta que permite a PHP procesar peticiones web de manera más rápida y eficiente. Es una interfaz de programación de aplicaciones de servidor (SAPI) de código abierto que se encarga de interpretar el código PHP. 

PHP-FPM funciona de la siguiente manera:
- Recibe las solicitudes de la aplicación PHP
- Las distribuye a procesos PHP individuales, llamados trabajadores
- Esto permite responder a las solicitudes de manera "en paralelo"

```
systemctl enable php-fpm --now
systemctl enable php74-php-fpm --now
```
## Configuración de archivos socket nginx
Configurar los sockets para cada versión de PHP-FPM: Edita los archivos de configuración para que cada versión de PHP-FPM use un socket diferente. Aquí te muestro un ejemplo de cómo editar el archivo de configuración para PHP 8.2

### Php 82
```
nvim /etc/php-fpm.d/www.conf

listen = /run/php-fpm/php82-fpm.sock
listen.owner = nginx
listen.group = nginx
listen.mode = 0660

systemctl restart php-fpm.service
chown nginx:nginx /run/php-fpm/php82-fpm.sock
```

### Php 74
```
nvim /etc/opt/remi/php74/php-fpm.d/www.conf

listen = /run/php-fpm/php74-fpm.sock
listen.owner = nginx
listen.group = nginx
listen.mode = 0660

systemctl restart php74-php-fpm.service
chown nginx:nginx /run/php-fpm/php74-fpm.sock
```

## Integrar php-fpm con nginx
```
server {
    listen 8080;
    server_name _;
    root /var/www/najimi.com;

    index index.html index.htm index.php;

    location / {
        try_files $uri $uri/ =404;
    }

    location ~ \.php$ {
        include fastcgi_params;
        fastcgi_pass unix:/run/php-fpm/php74-fpm.sock;
        fastcgi_index index.php;
        fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
    }

    location ~ /\.ht {
        deny all;
    }

}
```
