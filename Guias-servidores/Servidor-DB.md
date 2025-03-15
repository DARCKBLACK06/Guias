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
sudo dnf install
https://download.postgresql.org/pub/repos/yum/reporpms/EL-8-x86_64/pgdgredhat-repo-latest.noarch.rpm
```
