# Servidor SSH

Para configurar un servidor SSH en AlmaLinux, puedes seguir estos pasos para instalar y asegurar el servicio SSH.

---

### Paso 1: Instala el servidor SSH

1. Actualiza los paquetes del sistema:

   ```bash
   sudo dnf update -y
   ```

2. Instala el paquete **OpenSSH** si aún no está instalado:

   ```bash
   sudo dnf install -y openssh-server
   ```

### Paso 2: Inicia y habilita el servicio SSH

1. Inicia el servicio **sshd**:

   ```bash
   sudo systemctl start sshd
   ```

2. Habilita el servicio para que se inicie automáticamente al arrancar el sistema:

   ```bash
   sudo systemctl enable sshd
   ```

### Paso 3: Configura el Firewall (si es necesario)

Si tienes **firewalld** activado, debes permitir el tráfico SSH:

```bash
sudo firewall-cmd --add-service=ssh --permanent
sudo firewall-cmd --reload
```

### Paso 4: Configura el servidor SSH (opcional para mayor seguridad)

1. Edita el archivo de configuración SSH para ajustar algunas opciones de seguridad:

   ```bash
   sudo nano /etc/ssh/sshd_config
   ```

2. Realiza algunos ajustes recomendados:

   - Cambia el puerto SSH (opcional, pero recomendado por seguridad):

     ```ini
     Port 2222
     ```

   - Desactiva el inicio de sesión de root (para evitar que el usuario root se conecte directamente):

     ```ini
     PermitRootLogin no
     ```

   - Limita el acceso SSH a usuarios específicos:

     ```ini
     AllowUsers usuario1 usuario2
     ```

   - Activa el uso de autenticación con claves (opcional):

     ```ini
     PubkeyAuthentication yes
     ```

3. Guarda y cierra el archivo de configuración.

4. Reinicia el servicio SSH para aplicar los cambios:

   ```bash
   sudo systemctl restart sshd
   ```

### Paso 5: Configura la autenticación con clave SSH (opcional, para mayor seguridad)

1. En la máquina cliente, genera un par de claves SSH si aún no tienes uno:

   ```bash
   ssh-keygen -t rsa -b 4096
   ```

2. Copia la clave pública al servidor:

   ```bash
   ssh-copy-id usuario@ip_del_servidor
   ```

3. Una vez copiada la clave, puedes iniciar sesión sin contraseña:

   ```bash
   ssh usuario@ip_del_servidor -p 2222
   ```

**Nota**: Si cambiaste el puerto en la configuración, asegúrate de especificarlo con `-p <nuevo_puerto>` en cada conexión.

---

Con estos pasos, ya tendrás un servidor SSH en AlmaLinux configurado y seguro para conexiones remotas.
