<img align="right" src="https://github.com/ETCHDEV/Port-Windows-11-Xiaomi-11-Lite-NE/blob/main/lisa.png" width="250" alt="Windows 11 Running On a Mi 11 Lite NE">


# Uruchamianie systemu Windows na Xiaomi Mi 11 Lite NE/Mi 11 LE

## Instalacja systemu Windows

### Wymagania wstępne

- [Obraz systemu Windows na architekturę ARM (zalecany Windows 11)](https://uupdump.net/)
- [Najnowszy obraz UEFI skompilowany ze źródłowego kodu edk2-ms](https://github.com/edk2-porting/edk2-msm)
- [Obraz UEFI TYLKO do instalacji Windows!!](https://github.com/ETCHDEV/Port-Windows-11-Xiaomi-11-Lite-NE/releases/download/v0.0.1/boot-lisa-install.img)
- [DriverUpdater](https://github.com/WOA-Project/DriverUpdater/releases/latest)
- [Sterowniki](https://github.com/Icesito68/7xx-Drivers) (Kliknij na strzałkę "Get Code" i pobierz jako zip)

#### Rozruch do zmodyfikowanego obrazu UEFI przy użyciu następującego polecenia:
```cmd
fastboot boot boot-lisa-install.img
```
#### Po uruchomieniu do UEFI, wybierz opcję UEFI boot menu, a następnie wybierz ostatnią opcję (SCSI Disk option).

### Przypisywanie liter do dysków
#### Uruchom menedżer dysków Windows z poziomu CMD jako administrator,
> gdy Xiaomi Mi 11 Lite NE zostanie wykryty jako dysk SCSI.

```cmd
diskpart
```

### Przypisz litery do woluminów Windows i BOOT

#### Select the Windows volume of the phone
> Use `list volume` to find it, it's the one named "WINLISA". Note the vol no.
```diskpart
select volume <number>
assign letter=w
```
#### Now select the ESP volume of the phone
> Use `list volume` to find it, it's the one named "ESPLISA". Note the vol no.

```diskpart
select volume <number>
assign letter=s
```
#### Now exit diskpart
```diskpart
exit
```

### Install
> Replace `<path/to/install.wim>` with the actual path to install.wim, 
> `install.wim` is located in sources folder inside your ISO (it might also be named `install.esd`), 
> You can get it either by mounting or extracting the ISO.

> Since the touch on the device doesn't work, you have to use tools like NTlite to edit the ISO to make a local Administration account.

```cmd
dism /apply-image /ImageFile:<path/to/install.wim> /index:1 /ApplyDir:W:\
```

### Create Windows bootloader files

```cmd
bcdboot W:\Windows /s S: /f UEFI
```

### Install Drivers

> Replace `<lisadriversfolder>` with the actual location of the drivers folder

>Extract the zip file downloaded 
```cmd
.\driverupdater.exe -d <lisadriversfolder>\definitions\Desktop\ARM64\Internal\lisa.txt -r <lisadriversfolder> -p W:
```
  
## Allow unsigned drivers

> If you don't do this you'll get a BSOD

>  S: is the ESP partition
```cmd
cd S:\EFI\Microsoft\Boot
bcdedit /store BCD /set "{default}" testsigning on
bcdedit /store BCD /set "{default}" nointegritychecks on
bcdedit /store BCD /set "{default}" recoveryenabled no
bcdedit /store BCD /set "{default}" bootstatuspolicy IgnoreAllFailures
```

### Unassign disk letters
> So that they don't stay there after disconnecting the device

Open CMD as Administrator:
```cmd
diskpart
```

#### Unassigning Volumes of the phone:
> Use `list volume` to find it, it's the one named "WINLISA"

```diskpart
select volume <number>
remove letter w
```

#### Now select the ESP volume of the phone
> Use `list volume` to find it, it's the one named "ESPLISA"

```diskpart
select volume <number>
remove letter s
```

#### Exit diskpart
```diskpart
exit
```

## Boot into Windows
### Now boot into the lastest compiled UEFI img:
```cmd
fastboot boot boot-lisa.img
```

### Select the first Windows Option to boot

### If Windows rebooted after the setup to android, just go into fastboot the boot the img.
(I wouldn't recommend to flash the uefi img to boot because currently there is no support for majority of the device hardware and their drivers)

### You can launch programs by placing a batch script in startup folder.

## Finished!

