# Servidor DHCP

Para configurar un servidor DHCP en AlmaLinux, puedes utilizar el servicio **ISC DHCP Server** (Internet Systems Consortium DHCP), que es uno de los servidores DHCP más comunes y versátiles. Aquí te explico cómo instalarlo y configurarlo.

---

### Paso 1: Instala el servidor DHCP

1. Primero, actualiza los paquetes del sistema:

   ```bash
   sudo dnf update -y
   ```

2. Instala el servidor DHCP:

   ```bash
   sudo dnf install -y dhcp-server
   ```

### Paso 2: Configura el servidor DHCP

1. Edita el archivo de configuración del servidor DHCP, ubicado en `/etc/dhcp/dhcpd.conf`:

   ```bash
   sudo nano /etc/dhcp/dhcpd.conf
   ```

2. Configura los parámetros principales, como el rango de direcciones IP, la puerta de enlace, los servidores DNS, y otros ajustes según la red. A continuación, se muestra un ejemplo básico de configuración:

   ```ini
   # Configuración básica del servidor DHCP

   default-lease-time 600;
   max-lease-time 7200;

   subnet 192.168.1.0 netmask 255.255.255.0 {
       range 192.168.1.100 192.168.1.200;
       option routers 192.168.1.1;
       option domain-name-servers 8.8.8.8, 8.8.4.4;
       option domain-name "example.com";
   }
   ```

   - **default-lease-time** y **max-lease-time**: Tiempo de concesión de las direcciones IP (en segundos).
   - **subnet**: Define el segmento de red.
   - **range**: Define el rango de direcciones IP que el servidor asignará.
   - **option routers**: Define la puerta de enlace predeterminada.
   - **option domain-name-servers**: Especifica los servidores DNS.
   - **option domain-name**: Establece el nombre del dominio.

3. Guarda y cierra el archivo.

### Paso 3: Configura la interfaz de red

Especifica la interfaz de red que el servidor DHCP debe escuchar. Esto se define en el archivo `/etc/sysconfig/dhcpd`. Abre el archivo y edita la siguiente línea:

```bash
sudo nano /etc/sysconfig/dhcpd
```

Busca la línea `DHCPDARGS` y especifica el nombre de la interfaz de red (como `eth0` o `ens33`):

```ini
DHCPDARGS="eth0"
```

Guarda y cierra el archivo.

### Paso 4: Configura el Firewall (si es necesario)

Permite el tráfico DHCP en el firewall usando **firewalld**:

```bash
sudo firewall-cmd --add-service=dhcp --permanent
sudo firewall-cmd --reload
```

### Paso 5: Inicia y habilita el servicio DHCP

1. Inicia el servicio DHCP:

   ```bash
   sudo systemctl start dhcpd
   ```

2. Habilita el servicio para que se inicie automáticamente al arrancar el sistema:

   ```bash
   sudo systemctl enable dhcpd
   ```

### Paso 6: Verifica el estado del servidor DHCP

Para confirmar que el servidor DHCP está funcionando correctamente, usa el siguiente comando:

```bash
sudo systemctl status dhcpd
```

---

Con estos pasos, tu servidor DHCP en AlmaLinux debería estar listo y funcionando, asignando direcciones IP a los dispositivos de tu red.
