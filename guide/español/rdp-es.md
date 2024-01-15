<img align="right" src="https://github.com/ETCHDEV/Port-Windows-11-Xiaomi-11-Lite-NE/blob/main/lisa.png" width="250" alt="Windows 11 Running On a Mi 11 Lite NE">


# Ejecutando Windows en el Mi 11 Lite NE/Mi 11 LE

## Controlar el teléfono con RDP

### Requitos previos
- [unattend.xml](https://github.com/ETCHDEV/Port-Windows-11-Xiaomi-11-Lite-NE/releases/download/v0.0.1/unattend.xml) (For skipping OOBE and setting up RDP)
- [WinDbg](https://aka.ms/windbg/download) (For accessing the computer name to put in the RDP)
- [¡¡UEFI SOLO para montar las particiones!!](https://github.com/ETCHDEV/Port-Windows-11-Xiaomi-11-Lite-NE/releases/download/v0.0.1/boot-lisa-install.img)

> Debido a problemas con DriverUpdater, instale los controladores después de realizar la configuración de RDP y reiniciar después de ver el escritorio

> No utilices boot-lisa-host.img para esto

#### Inicie en la UEFI usando el siguiente comando:
```cmd
fastboot boot boot-lisa-install.img
```
#### Después de iniciar en UEFI, seleccione el menú de inicio de UEFI y luego seleccione la última opción (opción de disco SCSI)

### Asignar letras a los discos
#### Inicie el administrador de discos de Windows desde CMD como administrador
> Una vez que se detecta el Mi 11 Lite NE como un disco SCSI

```cmd
diskpart
```

### Asignación de letras de montaje a volúmenes de Windows y de ARRANQUE

#### Seleccione el volumen de Windows del teléfono
> Utilice `lista de volumen` para encontrarlo, es el que se llama "WINLISA". Tenga en cuenta el número del volumen.
```diskpart
select volume <number>
assign letter=w
```
#### Ahora seleccione el volumen ESP del teléfono
> Utilice `lista de volumen` para encontrarlo, es el que se llama "ESPLISA". Tenga en cuenta el vol no.

```diskpart
select volume <number>
assign letter=s
```
#### Now exit diskpart
```diskpart
exit
```

### Saltarse la OOBE y habilitar RDP
- Copy the unattend.xml inside ```W:\Windows\Panther``` folder. (You may have to create the ```Panther``` folder)

### Habilitación de KDNET y depuración
> Abrir CMD como administrador
```cmd
cd S:\EFI\Microsoft\Boot\
bcdedit /store BCD /dbgsettings NET BUSPARAMS:1 KEY:1.2.3.4 HOSTIP:169.254.255.255 PORT:50000 NODHCP
bcdedit /store BCD /set "{default}" debug on
```

### Desasignar letras de disco
> Para que no se queden ahí después de desconectar el dispositivo

Abra CMD como administrador:
```cmd
diskpart
```

#### Desasignación de volúmenes del teléfono:
> Use `list volume` to find it, it's the one named "WINLISA"

```diskpart
select volume <number>
remove letter w
```

#### Ahora seleccione el volumen ESP del teléfono
> Use `list volume` to find it, it's the one named "ESPLISA"

```diskpart
select volume <number>
remove letter s
```

#### Salir de diskpart
```diskpart
exit
```

#### Ahora, puede iniciar Windows utilizando la última imagen UEFI
- Después de iniciar en el escritorio, conecte el teléfono a la PC y abra WinDbg
- Ahora, haga clic en Adjuntar al kernel, en Red defina el puerto como 50000 y la clave como 1.2.3.4. Verificar interrupción en la conexión
- Después de la pausa inicial, escriba ```!process 0 1``` después de ```kd>``` y luego, después de 30 segundos, haga clic en pausa.
- Haga clic en uno de los ID de PEB y obtendrá un nombre de computadora como ```TELÉFONO-'caracteres aleatorios'```
- Después de eso, reinicie Windows presionando prolongadamente el volumen hacia abajo y el encendido y arrancando con la última imagen UEFI.
- Luego, nuevamente, cuando esté en el escritorio, conecte el teléfono y coloque el nombre de la computadora en su cliente RDP favorito.

## ¡Felicidades!, ahora puedes controlar Windows y depurar controladores.
