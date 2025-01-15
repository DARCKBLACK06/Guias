# Servidor DNS

Para configurar un servidor DNS en AlmaLinux, puedes usar **BIND (Berkeley Internet Name Domain)**, que es uno de los servidores DNS más populares. A continuación, tienes los pasos detallados para instalar y configurar un servidor DNS con BIND.

---

### Paso 1: Instala BIND

1. Actualiza los paquetes en tu sistema:

   ```bash
   sudo dnf update -y
   ```

2. Instala BIND:

   ```bash
   sudo dnf install -y bind bind-utils
   ```

### Paso 2: Configura BIND

1. Abre el archivo de configuración principal de BIND, ubicado en `/etc/named.conf`:

   ```bash
   sudo nano /etc/named.conf
   ```

2. Edita las siguientes opciones según sea necesario:

   - Cambia la lista de direcciones IP en las que BIND escuchará solicitudes DNS. Por ejemplo:

     ```ini
     listen-on port 53 { 127.0.0.1; any; };
     ```

     Cambia `any` a la dirección IP de tu servidor si solo quieres que escuche en una interfaz específica.

   - Permite las consultas desde tu red local cambiando el parámetro `allow-query`:

     ```ini
     allow-query { localhost; 192.168.1.0/24; };
     ```

   - Si quieres habilitar la resolución recursiva para la red local, asegúrate de que `recursion` esté en `yes`:

     ```ini
     recursion yes;
     ```

3. Guarda y cierra el archivo.

### Paso 3: Configura una Zona de Resolución Directa (Forward Lookup Zone)

1. Crea un archivo de zona para el dominio que deseas administrar. Por ejemplo, si quieres crear una zona para el dominio `example.com`, agrega la zona en el archivo `/etc/named.conf`:

   ```ini
   zone "example.com" IN {
       type master;
       file "/var/named/example.com.db";
       allow-update { none; };
   };
   ```

2. Crea el archivo de zona:

   ```bash
   sudo nano /var/named/example.com.db
   ```

3. Agrega el contenido de la zona (ajusta las direcciones IP y registros según sea necesario):

   ```ini
   $TTL 86400
   @   IN  SOA     ns1.example.com. admin.example.com. (
                   2023102701 ; Serial
                   3600       ; Refresh
                   1800       ; Retry
                   1209600    ; Expire
                   86400 )    ; Minimum TTL

   ; Servidores de nombres
   @       IN  NS      ns1.example.com.

   ; Registros A
   ns1     IN  A       192.168.1.10
   @       IN  A       192.168.1.10
   www     IN  A       192.168.1.20
   ```

   - **Serial**: Incrementa este número cada vez que realices cambios en el archivo de zona.
   - **ns1** y **www** son registros A con direcciones IP dentro de tu red.

4. Guarda y cierra el archivo.

### Paso 4: Configura una Zona de Resolución Inversa (Reverse Lookup Zone)

1. Agrega una zona inversa en el archivo `/etc/named.conf`:

   ```ini
   zone "1.168.192.in-addr.arpa" IN {
       type master;
       file "/var/named/192.168.1.db";
       allow-update { none; };
   };
   ```

2. Crea el archivo de zona inversa:

   ```bash
   sudo nano /var/named/192.168.1.db
   ```

3. Agrega el contenido de la zona inversa:

   ```ini
   $TTL 86400
   @   IN  SOA     ns1.example.com. admin.example.com. (
                   2023102701 ; Serial
                   3600       ; Refresh
                   1800       ; Retry
                   1209600    ; Expire
                   86400 )    ; Minimum TTL

   ; Servidores de nombres
   @       IN  NS      ns1.example.com.

   ; Registros PTR
   10      IN  PTR     ns1.example.com.
   20      IN  PTR     www.example.com.
   ```

4. Guarda y cierra el archivo.

### Paso 5: Configura el Firewall (si es necesario)

Permite el tráfico DNS (puerto 53) en el firewall usando **firewalld**:

```bash
sudo firewall-cmd --add-service=dns --permanent
sudo firewall-cmd --reload
```

### Paso 6: Inicia y habilita el servicio BIND

1. Inicia el servicio BIND:

   ```bash
   sudo systemctl start named
   ```

2. Activa el servicio para que se inicie automáticamente al arrancar el sistema:

   ```bash
   sudo systemctl enable named
   ```

### Paso 7: Verifica la configuración de BIND

1. Usa el comando `named-checkconf` para verificar que la configuración de `named.conf` no tenga errores:

   ```bash
   sudo named-checkconf
   ```

2. Verifica los archivos de zona para asegurarte de que no contienen errores de sintaxis:

   ```bash
   sudo named-checkzone example.com /var/named/example.com.db
   sudo named-checkzone 1.168.192.in-addr.arpa /var/named/192.168.1.db
   ```

### Paso 8: Prueba el servidor DNS

Desde una máquina cliente, puedes probar la resolución de nombres usando el comando `dig`:

```bash
dig @ip_del_servidor_dns www.example.com
```

Si todo está configurado correctamente, el servidor DNS debería responder con la dirección IP correspondiente para `www.example.com`.

--- 

Con estos pasos, habrás configurado un servidor DNS básico en AlmaLinux usando BIND, listo para resolver nombres en tu red.
