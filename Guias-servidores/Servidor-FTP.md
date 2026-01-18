# Servidor Ftp

Para configurar un servidor FTP en AlmaLinux, puedes usar **vsftpd** (Very Secure FTP Daemon), que es un servidor FTP seguro, rápido y bastante fácil de configurar. Aquí tienes los pasos detallados para instalarlo y configurarlo:

### Paso 1: Instala vsftpd
1. Primero, actualiza los paquetes de tu sistema:

   ```bash
   sudo dnf update -y
   ```

2. Luego, instala el servidor FTP **vsftpd**:

   ```bash
   sudo dnf install -y vsftpd
   ```

### Paso 2: Configura vsftpd
1. Abre el archivo de configuración de vsftpd:

   ```bash
   sudo nano /etc/vsftpd/vsftpd.conf
   ```

2. A continuación, realiza los siguientes cambios en el archivo para configurar un servidor FTP básico:

   - Permite que los usuarios locales se conecten al servidor FTP:
     
     ```ini
     local_enable=YES
     ```

   - Permite subir archivos al servidor FTP:
     
     ```ini
     write_enable=YES
     ```

   - Restringe a los usuarios locales a sus directorios personales:

     ```ini
     chroot_local_user=YES
     ```

   - Opcionalmente, puedes habilitar conexiones anónimas (solo lectura) para el acceso público:

     ```ini
     anonymous_enable=NO
     ```

3. Guarda y cierra el archivo.

### Paso 3: Configura Firewall (si es necesario)
Si tienes **firewalld** habilitado, debes permitir el servicio FTP:

```bash
sudo firewall-cmd --add-service=ftp --permanent
sudo firewall-cmd --reload
```

### Paso 4: Inicia y habilita el servicio vsftpd
1. Activa el servicio para que se inicie automáticamente al arrancar el sistema:

   ```bash
   sudo systemctl enable --now vsftpd
   ```
2. Inicia el servicio **vsftpd**:

   ```bash
   sudo systemctl start vsftpd
   ```

### Paso 5: Prueba el servidor FTP
1. Crea o verifica que exista un usuario local en tu sistema (por ejemplo, `ftpuser`):

   ```bash
   sudo adduser ftpuser
   sudo passwd ftpuser
   ```

2. Accede al servidor FTP usando un cliente FTP desde otro dispositivo o desde la misma máquina:

   ```bash
   ftp <ip_del_servidor>
   ```

   Luego, inicia sesión con el nombre de usuario y contraseña que configuraste.

### Paso 6: Opcional - Configura acceso seguro con FTPS
Para mejorar la seguridad, puedes configurar FTP con SSL/TLS (FTPS):

1. Crea certificados SSL para FTPS:

   ```bash
   sudo openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout /etc/ssl/private/vsftpd.key -out /etc/ssl/certs/vsftpd.crt
   ```

2. Agrega estas opciones en el archivo `/etc/vsftpd/vsftpd.conf`:

   ```ini
   ssl_enable=YES
   allow_anon_ssl=NO
   force_local_data_ssl=YES
   force_local_logins_ssl=YES
   ssl_tlsv1=YES
   ssl_sslv2=NO
   ssl_sslv3=NO
   rsa_cert_file=/etc/ssl/certs/vsftpd.crt
   rsa_private_key_file=/etc/ssl/private/vsftpd.key
   ```

3. Guarda el archivo y reinicia **vsftpd**:

   ```bash
   sudo systemctl restart vsftpd
   ```

Ahora tienes configurado un servidor FTP en AlmaLinux y, opcionalmente, un servidor FTPS para conexiones seguras.

