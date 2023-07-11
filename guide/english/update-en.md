<img align="right" src="https://github.com/ETCHDEV/Port-Windows-11-Xiaomi-11-Lite-NE/blob/main/lisa.png" width="250" alt="Windows 11 Running On a Mi 11 Lite NE">


# Running Windows on the Mi 11 Lite NE/Mi 11 LE

## Driver updating

### Prerequisites

- [UEFI image for ONLY installing Windows!!](https://github.com/ETCHDEV/Port-Windows-11-Xiaomi-11-Lite-NE/releases/download/v0.0.1/boot-lisa-install.img)
- [DriverUpdater](https://github.com/WOA-Project/DriverUpdater/releases/latest)
- [Updated Drivers](https://github.com/Icesito68/7xx-Drivers) (Click on the Get Code arrow and Download as zip)

#### Boot into modified UEFI img using the following command:
```cmd
fastboot boot boot-lisa-install.img
```
#### After booting into UEFI, select the UEFI boot menu and then in it select the last option (SCSI Disk option)

### Assigning letters to disks
#### Start the Windows disk manager from CMD as admin
> Once the Mi 11 Lite NE is detected as a SCSI Disk

```cmd
diskpart
```

### Assigning Mount Letter to Windows and BOOT Volumes
#### Select the Windows volume of the phone
> Use `list volume` to find it, it's the one named "WINLISA". Note the vol no.
```diskpart
select volume <number>
assign letter=w
```
#### Now exit diskpart
```diskpart
exit
```

### Install Drivers

> Replace `<lisadriversfolder>` with the actual location of the updated drivers folder

>Extract the zip file downloaded 
```cmd
.\driverupdater.exe -d <lisadriversfolder>\components\QC7325\Platform -r <lisadriversfolder> -p W:
```


### Now you can Boot to Windows, first startup after installing drivers may take some time (like getting the Getting Ready message)


## Finished!!
