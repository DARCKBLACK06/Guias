# Servidor de base de datos

Para configurar servidores de bases de datos **MariaDB** y **PostgreSQL** en AlmaLinux, aquí tienes los pasos detallados para cada uno.

---

## 1. Configuración de MariaDB

### Paso 1: Instala MariaDB
1. Actualiza los paquetes en tu sistema:

   ```bash
   sudo dnf update -y
   ```

2. Instala el servidor **MariaDB**:
> .[!NOTE].
> Es importante tener todos los paquetes actualizados.
   ```bash
   sudo dnf install -y mariadb-server
   ```

### Paso 2: Inicia y habilita el servicio MariaDB
1. Inicia el servicio:

   ```bash
   sudo systemctl start mariadb
   ```

2. Activa el servicio para que se inicie automáticamente al arrancar el sistema:

   ```bash
   sudo systemctl enable mariadb
   ```

### Paso 3: Configura la seguridad inicial de MariaDB
Ejecuta el siguiente comando para configurar la seguridad de MariaDB, lo que incluye la configuración de la contraseña de root y la eliminación de accesos anónimos:

```bash
sudo mysql_secure_installation
```

Sigue las indicaciones en pantalla para completar la configuración.

### Paso 4: Conéctate a MariaDB
Usa el siguiente comando para iniciar sesión en MariaDB como el usuario `root`:

```bash
sudo mysql -u root -p
```

Una vez dentro, puedes crear una base de datos, un usuario y otorgarle permisos:

```sql
CREATE DATABASE nombre_base_datos;
CREATE USER 'usuario'@'localhost' IDENTIFIED BY 'contraseña';
GRANT ALL PRIVILEGES ON nombre_base_datos.* TO 'usuario'@'localhost';
FLUSH PRIVILEGES;
EXIT;
```

### Paso 5: Configura el Firewall (si es necesario)
Si estás usando **firewalld**, permite el puerto 3306 para conexiones remotas de MariaDB (opcional):

```bash
sudo firewall-cmd --add-service=mysql --permanent
sudo firewall-cmd --reload
```

---

## 2. Configuración de PostgreSQL
Instalación de postgres


AGREGAR REPOSITORIO DE POSTGRES
```bash
sudo dnf install -y https://download.postgresql.org/pub/repos/yum/reporpms/EL-8-x86_64/pgdgredhat-repo-latest.noarch.rpm
```
DESACTIVAR EL MODULO DE POSTGRES PARA TENER LA VERSION ACTUAL
 ```bash
sudo dnf -qy module disable postgresql
 ```
ACTUALIZAMOS
 ```bash
sudo dnf update -y
 ```
INSTALAMOS EL CLIENTE Y SERVIDOR DE POSTGRES
 ```bash
sudo dnf install postgresql13 postgresql13-server
 ```
INICAREMOS LA BASE DE DATOS
 ```bash
systemctl enable postgresql-13 --now
systemctl restart postgresql-13
 ```
COMPROBAR EL STATUS 
 ```bash
systemctl status postgresql-13
 ```
CAMBIA LA CONTRASEÑA DE POSTGRES
 ```bash
passwd postgres
 ```
CAMBIAR USUARIO A POSTGRES
 ```bash
su - postgres
 ```
INICIAMOS POSTGRES
 ```sql
psql
 ```
CREAMOS U USUARIO Y CONTRASEÑA
 ```sql
CREATE USER usuario WITH PASSWORD 'contrasena';
 ```
CREAMOS LA BASE DE DATOS
 ```sql
CREATE DATABASE nombre_de_la_base;
 ```
AGREGAMOS EL USUARIO A LA BASE DE DATOS
 ```sql
ALTER DATABASE nombre_de_la_base OWNER TO usuario;
 ```
PARA SALIR
 ```bash
\q
 ```
CONFIGURACION DE LOS ARCHIVOS CONF

usar editor de texto de su preferencia

Ejemplo: nano /var/lib/pgsql/"version-instalada"/data/pg_hba.conf

 ```bash
vi /var/lib/pgsql/13/data/pg_hba.conf
 ```
modificar la liinea 86 si se desconoce su red poner en su lugar
0.0.0.0/0 md5
asi deberia de quedar
host all all 0.0.0.0/0 md5
 ```bash
vi /var/lib/pgsql/13/data/postgresql.conf
 ```
descomentar la linea 60 y remplazar localhost por *
listen_addresses = '*'
Descomentar la linea 64 y la 65 si esta es comentada
port = 5432
max_connections = 100

HABILITAR EN EL FIREWAL
 ```bash
firewall-cmd --add-port=5432/tcp --permanent
firewall-cmd --reload
 ```
REINICIAR POSTGRES PARA APLICAR LOS CAMBIOS
 ```bash
systemctl restart postgresql-13
 ```
