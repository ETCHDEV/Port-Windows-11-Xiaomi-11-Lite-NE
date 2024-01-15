<img align="right" src="https://github.com/ETCHDEV/Port-Windows-11-Xiaomi-11-Lite-NE/blob/main/lisa.png" width="250" alt="Windows 11 Running On a Mi 11 Lite NE">


# Ejecutando Windows en el Mi 11 Lite NE/Mi 11 LE

## Instalar Windows Windows

### Requitos Previos

- [Windows ARM (Preferiblemente Windows 11)](https://uupdump.net/)
- [La última UEFI](https://github.com/ETCHDEV/Port-Windows-11-Xiaomi-11-Lite-NE/releases)
- [¡¡La UEFI SOLO para la instalación de Windows!!](https://github.com/ETCHDEV/Port-Windows-11-Xiaomi-11-Lite-NE/releases/download/v0.0.1/boot-lisa-install.img)
- [DriverUpdater](https://github.com/WOA-Project/DriverUpdater/releases/latest)
- [Drivers](https://github.com/Icesito68/7xx-Drivers) (Haga clic en la flecha Obtener código y descárguelo como zip.)

#### Arrancar en la UEFI modificada con el siguiente comando:
```cmd
fastboot boot boot-lisa-install.img
```
#### Después de iniciar la UEFI, Selecciona UEFI boot menu y después la última opción (SCSI Disk option)

### Asignar letras a los discos
#### Inicia Diskpart como administrador en el CMD
> Cuando el Mi 11 Lite NE sea detectado como disco SCSI

```cmd
diskpart
```

### Asigna un punto de montaje a los volúmenes

#### Selecciona el volumen de Windows del teleéfono
> Usa `list volume` para encontrarlo, es el que se llama "WINLISA". Apunta el número del volumen.
```diskpart
select volume <number>
assign letter=w
```
#### Ahora selecciona el volumen de ESP del teléfono
> Usa `list volume` para encontrarlo, es el que se llama "ESPLISA". Apunta el número del volumen.

```diskpart
select volume <number>
assign letter=s
```
#### Sal de diskpart
```diskpart
exit
```

### Instalar
> Reemplaza `<path/to/install.wim>` por el directorio de tu install.wim, 
> `install.wim` está localizado en la carpeta sources de la ISO (también puede que se llame `install.esd`), 
> Puedes obtenerlo descomprimiendo o montando el ISO.

```cmd
dism /apply-image /ImageFile:<path/to/install.wim> /index:2 /ApplyDir:W:\
```

### Creamos los archivos de arranque de Windows

```cmd
bcdboot W:\Windows /s S: /f UEFI
```

### Instalar los drivers

> Reemplaza `<lisadriversfolder>` por el directorio actual de la carpeta de drivers

>Si quieres RDP, Sigue este proceso en el primer arranque

>Extrae el arvhivo zip que descargaste
```cmd
.\driverupdater.exe -d <lisadriversfolder>\definitions\Desktop\ARM64\Internal\lisa.txt -r <lisadriversfolder> -p W:
```

>O si estás en la carpeta
```cmd
.\driverupdater.exe -d definitions\Desktop\ARM64\Internal\lisa.txt -r . -p W:
```
  
## Permitimos los drivers no firmados

> Si no haces esto obtendrás un BSOD

>  S: es la partición ESP
```cmd
cd S:\EFI\Microsoft\Boot
bcdedit /store BCD /set "{default}" testsigning on
bcdedit /store BCD /set "{default}" nointegritychecks on
bcdedit /store BCD /set "{default}" recoveryenabled no
bcdedit /store BCD /set "{default}" bootstatuspolicy IgnoreAllFailures
```

### Usamos el control RDP

> No uses esto si quieres el modo host del USB

Usa la siguiente guia: [RDP Method](https://github.com/ETCHDEV/Port-Windows-11-Xiaomi-11-Lite-NE/blob/main/guide/español/rdp-es.md)

### Desasignar las letras de los discos
> Para que no se queden ahí después de desconectar el dispositivo

Open CMD as Administrator:
```cmd
diskpart
```

#### Desasignar las letras de los volúmenes
> Usa `list volume` para encontrarlo, es el que se llama "WINLISA". Apunta el número del volumen.

```diskpart
select volume <number>
remove letter w
```

#### Ahora selecciona el volumen ESP 
> Usa `list volume` para encontrarlo, es el que se llama "ESPLISA". Apunta el número del volumen.

```diskpart
select volume <number>
remove letter s
```

#### Salimos de diskpart
```diskpart
exit
```

## Iniciamos Windows
### Ahora iniciaremos la última UEFI:

> Utilice boot-lisa-device.img para el modo de dispositivo (o necesita RDP y WinDbg)

> Utilice [boot-lisa-host.img](https://github.com/ETCHDEV/Port-Windows-11-Xiaomi-11-Lite-NE/releases/download/0.1/boot-lisa-host.img) para modo host (si desea conectar dispositivos USB como un mouse USB)

```cmd
fastboot boot boot-lisa.img
```

### Selecciona la primera opción de Windows para iniciarlo

### If Windows rebooted after the setup to android, just go into fastboot the boot the img.
(No recomendaría actualizar uefi img para arrancar porque actualmente no hay soporte para la mayoría del hardware del dispositivo y sus controladores)

### Utilice [boot-lisa-device.img](https://github.com/ETCHDEV/Port-Windows-11-Xiaomi-11-Lite-NE/releases/download/0.1/boot-lisa-device. img) para el modo de dispositivo (o necesita RDP y WinDbg)
### Utilice [boot-lisa-host.img](https://github.com/ETCHDEV/Port-Windows-11-Xiaomi-11-Lite-NE/releases/download/0.1/boot-lisa-host. img) para Modo Host (si desea conectar dispositivos USB)

### Puede iniciar programas colocando un script por lotes en la carpeta de inicio. (No es necesario si el host USB está habilitado)

### O use [Método RDP](https://github.com/ETCHDEV/Port-Windows-11-Xiaomi-11-Lite-NE/blob/main/guide/english/rdp-en.md)
(No utilice este método si se requiere un host USB)
## ¡Terminamos!