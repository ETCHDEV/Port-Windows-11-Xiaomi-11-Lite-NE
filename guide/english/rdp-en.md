<img align="right" src="https://github.com/ETCHDEV/Port-Windows-11-Xiaomi-11-Lite-NE/blob/main/lisa.png" width="250" alt="Windows 11 Running On a Mi 11 Lite NE">


# Running Windows on the Mi 11 Lite NE/Mi 11 LE

## Controlling phone over RDP

### Prerequisites
- [unattend.xml]()(For skipping OOBE and setting up RDP)
- [WinDbg]()(For accessing the computer name to put in the RDP)
- [UEFI image for ONLY mounting partition!!](https://github.com/ETCHDEV/Port-Windows-11-Xiaomi-11-Lite-NE/releases/download/v0.0.1/boot-lisa-install.img)

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

### Assigning Mount Letter to BOOT Volume
#### Select the BOOT volume of the phone
> Use `list volume` to find it, it's the one named "ESPLISA". Note the vol no.
```diskpart
select volume <number>
assign letter=s
```

#### Now exit diskpart
```diskpart
exit
```

### Enabling KDNET and debugging
```cmd
cd S:\EFI\Microsoft\Boot\
bcdedit /store BCD /dbgsettings NET BUSPARAMS:1 KEY:1.2.3.4 HOSTIP:169.254.255.255 PORT:50000 NODHCP
bcdedit /store BCD /set {default} debug on
```

### Unassign BOOT disk letter
> So that they don't stay there after disconnecting the device

Open CMD as Administrator:
```cmd
diskpart
```

#### Unassigning BOOT volume of the phone:
> Use `list volume` to find it, it's the one named "ESPLISA"

```diskpart
select volume <number>
remove letter s
exit
```

#### Now, you can boot into windows by using latest UEFI image
- After booting to desktop, connect the phone to the PC and open WinDbg
- Now, click on Attach to Kernel, Under Net define port as 50000 and Key as 1.2.3.4. Check Break on Connection
- After initial break, type ```!process 0 1``` after ```kd>``` and then after 30 sec click on break.
- Click one of the PEB id and you will get a Computer Name Like ```PHONE-random character```
- After that reboot windows by long pressing both vol down and power and booting off the latest UEFI image.
- After againg when on desktop, connect the phone and put the computer name in your faviourite RDP client.

## Congrats!, now you can control windows and debug drivers.

