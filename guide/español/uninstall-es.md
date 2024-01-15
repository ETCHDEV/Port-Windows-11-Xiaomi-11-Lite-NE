<img align="right" src="https://github.com/ETCHDEV/Port-Windows-11-Xiaomi-11-Lite-NE/blob/main/lisa.png" width="250" alt=" Windows 11 ejecutándose en un Mi 11 Lite NE">


# Ejecutando Windows en el Mi 11 Lite NE/Mi 11 LE

## Desinstalación

### ¿Por qué es necesario esto?

- Si ha seguido la guía anterior, el orden de sus particiones será muy diferente y puede tener algunas consecuencias si no restaura su tabla de particiones original.
- Si desea desinstalar Windows, esto se utiliza en lugar de eliminar particiones manualmente para evitar errores humanos + escribir una guía completa dedicada solo a la desinstalación.
- Si desea volver a bloquear su gestor de arranque, necesitará que su tabla de particiones esté en stock.

### Requisitos previos
- [ADB y Fastboot] (https://developer.android.com/studio/releases/platform-tools)
- gpt_both0.bin (Obtenerlo de la ROM del dispositivo, ubicada en la carpeta de imágenes)

#### Restaurar GPT
> Reemplace ```<gpt_both0.bin>``` con la ruta al archivo gpt_both0.bin.
```cmd
fastboot flash partition:0 <gpt_both0.bin>
```

#### Borrar userdata para evitar el bootloop y restaurar el tamaño de FS
```cmd
fastboot -w
```

