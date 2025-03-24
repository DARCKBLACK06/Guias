# Pasos para eliminar otro sistema operativo en Windows

## 1. Abrir la terminal como administrador
Ejecutar **CMD** en modo administrador.

## 2. Entrar a DiskPart y asignar una letra a la partición EFI
Ejecutar los siguientes comandos:
```sh
 diskpart
 sel disk 0
 sel part 1
 assign letter=z:
 exit
```

## 3. Restaurar el gestor de arranque de Windows
Ejecutar el siguiente comando en la terminal con privilegios de administrador:
```sh
 bcdboot C:\Windows /s z: /f all
```

## 4. Listar las entradas de arranque disponibles
Ejecutar:
```sh
 bcdedit.exe /enum firmware
```
Buscar la entrada del sistema operativo que deseas eliminar. Debería verse algo similar a:
```
 identifier              {39c148f6-9aae-11ef-8c83-806e6f6e6963}
 device                  partition=\Device\HarddiskVolume8
 path                    \EFI\almalinux\shimx64.efi
 description             AlmaLinux
```

## 5. Eliminar la entrada de arranque no deseada
Ejecutar:
```sh
 bcdedit.exe /delete {39c148f6-9aae-11ef-8c83-806e6f6e6963}
```
Esto eliminará la entrada del otro sistema operativo y el GRUB.

---

# Pasos para comprobar la integridad y archivos dañados del disco
(Próxima sección por completar)

