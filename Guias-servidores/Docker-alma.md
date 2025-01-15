# Docker Alma Linux
---

## **Instalación de Docker en AlmaLinux 9**

### **Paso 1: Actualizar el sistema**
Ejecuta los siguientes comandos para asegurarte de que tu sistema esté actualizado:
```bash
sudo dnf update -y
```

### **Paso 2: Instalar dependencias**
Docker necesita algunas herramientas adicionales para funcionar correctamente:
```bash
sudo dnf install -y yum-utils device-mapper-persistent-data lvm2
```

### **Paso 3: Configurar el repositorio oficial de Docker**
Agrega el repositorio oficial de Docker para CentOS (compatible con AlmaLinux):
```bash
sudo dnf config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
```

### **Paso 4: Instalar Docker**
Instala Docker Community Edition (CE) y los complementos necesarios:
```bash
sudo dnf install -y docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

### **Paso 5: Iniciar y habilitar Docker**
Inicia el servicio Docker y habilítalo para que arranque automáticamente al iniciar el sistema:
```bash
sudo systemctl start docker
sudo systemctl enable docker
```

### **Paso 6: Verificar la instalación**
Para confirmar que Docker se instaló correctamente, ejecuta:
```bash
docker --version
```
Deberías ver algo como:
```plaintext
Docker version 24.x.x, build xxxxxx
```

### **Paso 7: Configurar permisos de usuario**
Para evitar usar `sudo` en cada comando, agrega tu usuario al grupo `docker`:
```bash
sudo usermod -aG docker $USER
```
Cierra la sesión y vuelve a iniciarla para que los cambios surtan efecto.

---

## **Uso básico de Docker**

### **1. Probar Docker**
Ejecuta el contenedor de prueba oficial:
```bash
docker run hello-world
```
Esto descarga la imagen `hello-world`, crea un contenedor y muestra un mensaje de éxito.

### **2. Comandos básicos de Docker**

| **Acción**                 | **Comando**                                                |
|----------------------------|-----------------------------------------------------------|
| Listar imágenes            | `docker images`                                           |
| Descargar una imagen       | `docker pull <nombre_imagen>`                             |
| Crear y ejecutar un contenedor | `docker run -d -p 8080:80 <nombre_imagen>`              |
| Ver contenedores activos    | `docker ps`                                              |
| Ver todos los contenedores  | `docker ps -a`                                           |
| Detener un contenedor       | `docker stop <ID o nombre>`                              |
| Eliminar un contenedor      | `docker rm <ID o nombre>`                                |
| Eliminar una imagen         | `docker rmi <ID o nombre>`                               |
| Inspeccionar contenedor     | `docker inspect <ID o nombre>`                          |
| Ver logs del contenedor     | `docker logs <ID o nombre>`                             |

### **3. Ejemplo rápido**
Para ejecutar un servidor Nginx:
```bash
docker run -it --rm -d -p 8080:80 --name web nginx
docker stop web
```
Abre tu navegador en `http://localhost:8080`.

### **4. Añadiendo html customizado**
```bash
docker run --name web -d -p 8080:80 -v /home/najimi/Escritorio/docker/:/usr/share/nginx/html:ro nginx
docker stop web
```

---

## **Uso de Docker Compose**

Docker Compose permite definir y ejecutar aplicaciones compuestas por varios contenedores usando un archivo `docker-compose.yml`.

### **Sintaxis del archivo YAML**
Los archivos YAML tienen una estructura jerárquica basada en sangría con espacios (no tabuladores). Un archivo YAML para Docker Compose usa tres secciones principales:

1. **Version**: Define la versión del esquema de Docker Compose (recomendado: `3.8`).
2. **Services**: Describe los contenedores a ejecutar.
3. **Networks/Volumes** (opcional): Configura redes o volúmenes compartidos.

### **Ejemplo básico de un archivo `docker-compose.yml`**
```yaml
# docker-compose.yaml
services:
    database:
        image: 'mongo:4.4.6'
        container_name: 'pandoraxdn'
        restart: always
        environment:
          - MONGODB_INITDB_DATABASE=najimi
          - MONGODB_INITDB_ROOT_USERNAME_FILE=admin
          - MONGODB_INITDB_ROOT_PASSWORD_FILE=password
        volumes:
          - ./mongo-volume:/data/db
        ports:
            - '27017:27017'
```

---

### **Comandos comunes de Docker Compose**

| **Acción**                              | **Comando**                         |
|-----------------------------------------|--------------------------------------|
| Ejecutar todos los servicios definidos  | `docker-compose up`                 |
| Ejecutar en segundo plano               | `docker-compose up -d`              |
| Detener los servicios                   | `docker-compose down`               |
| Listar contenedores administrados       | `docker-compose ps`                 |
| Ver logs                                | `docker-compose logs`               |
| Reiniciar un servicio                   | `docker-compose restart <servicio>` |

### **Ejemplo: Levantar el archivo YAML**
Si tienes el archivo `docker-compose.yml` en tu directorio actual, ejecuta:
```bash
docker compose up -d
```

Esto descarga las imágenes necesarias, crea los contenedores y los ejecuta.

### **Eliminar los servicios**
Para detener y eliminar los contenedores creados por Docker Compose:
```bash
docker-compose down
```

---
