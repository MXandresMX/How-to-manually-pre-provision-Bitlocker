# Instalar Windows 10 con bitlocker en 2 sistemas operativos

Vamos a borrar el disco donde se va a instalar el Windows 10. Si van a ser en 2 ssd entonces el otro lo dejamos "Unallocated" para que no aparezca como disco cuando inicie windows

Elegimos nuevo disco

![Install](/1.png)

Una vez creada la partición se elige formatear.

![Install](/2.png)

Una vez formateada la partición presionamos SHIFT + F10, se abre una ventana de comando y ejecutamos DISKPART y escribimos LIST VOLUME. Sólo nos interesa saber cual es la letra del unidad que se va a cifrar. Salimos con el comando EXIT

> diskpart
> list volume
