<img align="right" src="https://github.com/ETCHDEV/Port-Windows-11-Xiaomi-11-Lite-NE/blob/main/lisa.png" width="250" alt="Windows 11 Running On a Mi 11 Lite NE">


# Ejecutando Windows en el Mi 11 Lite NE/Mi 11 LE

## Problemas comunes


## El dispositivo puede iniciarse en Android pero no en el gestor de arranque

### Requisios previos:

- [ADB & Fastboot](https://developer.android.com/studio/releases/platform-tools)
- [parted for patitioning the storage](https://www.mediafire.com/file/s9bjano4pezphou/parted/file)

Esto se debe a particiones con nombres de volúmenes que el gestor de arranque no puede manejar. Para solucionarlo:

- Arranque en TWRP/OF o cualquier recuperación personalizada que admita el cifrado del dispositivo/puede acceder a la partición de datos y tiene soporte para adb

- Copie el parted al teléfono e ingrese adb shell usando los siguientes comandos:
```cmd
adb push parted /cache/
adb shell "chmod 755 /cache/parted"
adb shell
```

- Iniciar parted
```sh
cd /cache
./parted /dev/block/sda
```

- Ejecute ```print``` para listar todas las particiones

- Busque particiones que tengan espacios en los nombres, por ejemplo, "Partición de datos básicos" y anote su número de volumen.

- Ahora ejecute ```rm <número de vol>```, por ejemplo, ```rm 35``` (Asegúrese de haber eliminado ambas particiones de Windows creadas)

- Si eliminó las particiones 34 y 35, siga la [Guía de instalación] (./partition-en.md) desde el paso Crear la partición ESP (dependiendo de si su dispositivo tiene 128 GB o 256 GB)