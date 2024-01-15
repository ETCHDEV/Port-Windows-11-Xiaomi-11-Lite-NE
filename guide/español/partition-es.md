<img align="right" src="https://github.com/ETCHDEV/Port-Windows-11-Xiaomi-11-Lite-NE/blob/main/lisa.png" width="250" alt="Windows 11 Running On a Mi 11 Lite NE">


# Ejecutando Windows en el Mi 11 Lite NE/Mi 11 LE

## Installation

## Partitioning your device

### Prerequisites
- [ADB y Fastboot](https://developer.android.com/studio/releases/platform-tools)
- [parted para particionas el almacenamiento](https://www.mediafire.com/file/s9bjano4pezphou/parted/file)
- [Recovery](https://xdaforums.com/t/unofficial-recovery-new-orangefox-recovery-r13-14-support-android-13-14.4466559)

### Notas:
> **Advertencia** si alguna vez elimina alguna partición a través de diskpart, Windows enviará un comando UFS que borrará todo el almacenamiento UFS.
- ¡Todos tus datos serán borrados! Haga una copia de seguridad ahora si es necesario.
- Estos comandos no se han probado completamente (solo para el dispositivo variante de 256 GB)
- No ejecutar el mismo comando dos veces.
- NO REINICIA TU TELÉFONO si crees que cometiste un error, pide ayuda con el problema de Github.


#### ⚠️ ¡No ejecutes todos los comandos a la vez, ejecútalos en orden!

#### ⚠️ ¡¡¡NO COMETAS NINGÚN ERROR!!! ¡¡¡PUEDES ROMPER TU DISPOSITIVO CON LOS COMANDOS A CONTINUACIÓN SI LOS HACES MAL!!!

##### Arranque en TWRP/OF o cualquier recuperación personalizada que admita el cifrado del dispositivo/puede acceder a la partición de datos y tiene soporte para adb
##### Copie el parted al teléfono e ingrese a adb shell usando los siguientes comandos:
```cmd
adb push parted /cache/
adb shell "chmod 755 /cache/parted"
adb shell
```

##### Redimensionar la tabla de particiones
> Para que quepan las particiones de Windows
```sh
sgdisk --resize-table 64 /dev/block/sda
```

##### Iniciar parted
```sh
cd /cache
./parted /dev/block/sda
```

##### Borrar `userdata`
> You can make sure that 33 is the userdata partition number by running
>  `print all`
```sh
rm 33
```

##### Creando las particiones
> Si recibe algún mensaje de advertencia que le indica que ignore o cancele, simplemente escriba i y presione enter

<details>
<summary><b><strong>Para los modelos de 128GB</strong></b></summary>



- Crear la partición de datos de android
```sh
mkpart userdata ext4 12.2GB 70.0GB
```

- Cree la partición ESP (almacena datos del cargador de arranque de Windows y archivos EFI)
```sh
mkpart esp fat32 70.0GB 70.3GB
```

- Cree la partición principal donde se instalará Windows.
```sh
victoria mkpartntfs 70.3GB 125GB
```

  </summary>
</details>

<details>
<summary><b><strong>Para los modelos de 256GB</strong></b></summary>


- Cree la partición de datos de Android (busque dónde termina la última parte usando imprimir y comience la parte de datos del usuario desde allí)
```sh
datos de usuario mkpart ext4 12,2 GB 140,2 GB
```

- Cree la partición ESP (almacena datos del cargador de arranque de Windows y archivos EFI)
```sh
mkpart esp fat32 140,2GB 140,5GB
```

- Cree la partición principal donde se instalará Windows.
```sh
mkpart ganar ntfs 140,5 GB 254 GB
```
​
  </summary>
</details>

##### Haga que la partición ESP sea de arranque para que la imagen UEFI pueda detectarla y salir
```sh
set 34 esp on
quit
```
##### Reiniciar en el recovery
##### Inicie el shell nuevamente en su PC
```cmd
adb shell
```

##### Formatear particiones
- Formatear la partición ESP como FAT32
```sh
mkfs.fat -F32 -s1 /dev/block/by-name/esp -n ESPLISA
```

- Formatear la partición de Windows como NTFS
```sh
mkfs.ntfs -f /dev/block/by-name/win -L WINLISA
```
- Format data
Regrese al menú "Limpiar" y presione "Formatear datos",
luego escriba "sí

##### Simplemente reinicie el teléfono y vea si Android aún funciona (Sí, el teléfono está formateado)

### ¡¡La parte difícil ya está hecha!!, ahora ve a

## [Siguiente paso: instalar Windows](install-es.md)
