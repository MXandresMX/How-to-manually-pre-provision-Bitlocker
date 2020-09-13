# Instalar Windows 10 con bitlocker en 2 sistemas operativos

Vamos a borrar el disco donde se va a instalar el Windows 10. Si van a ser en 2 ssd entonces el otro lo dejamos "Unallocated" para que no aparezca como disco cuando inicie windows

Elegimos nuevo disco

![Install](/1.png)

Una vez creada la partición se elige formatear.

![Install](/2.png)

Una vez formateada la partición presionamos SHIFT + F10, se abre una ventana de comando y ejecutamos `DISKPART` y escribimos `LIST VOLUME`. Sólo nos interesa saber cual es la letra del unidad que se va a cifrar. Salimos con el comando `EXIT`
```
> diskpart
> list volume
> exit
```
![Install](/3.png)

Ahora vamos a cifrar la unidad en este caso es F:, si se quiere cifrar solamente el espacio usado se usa `-used`
```
> manage-bde -on F:
```
![Install](/4.png)

Para ver el estado de encriptación hay que ejecutar el siguiente comando
```
> manage-bde -on F:
```
![Install](/5.png)

Una vez que el proceso a llegado al 100%, entonces cerramos la ventana e instalamos windows normalmente.

```diff
- Importante! Cuando el sistema operativo haya iniciado sesión,
- necesitamos remover la USB de instalación. 
- Abrir el administrador de discos y seleccionar los discos que no usaremos
- y le daremos OFFLINE
```

Ejecutamos en un cmd `gpedit.msc`

Vaya a Configuración del equipo, Plantillas administrativas, Componentes de Windows, Cifrado de unidad BitLocker, Unidades del sistema operativo.

1. “Requerir autenticación adicional al iniciar”. Asegúrese de que la opción “Habilitado” esté seleccionada, de modo que todas las otras opciones estén activas.
2. Desmarque la casilla para "Permitir BitLocker sin un TPM compatible".
3. Para la opción de “Configurar inicio del TPM”, seleccione “Permitir TPM”.
4. Para la opción de "Configurar PIN de inicio del TPM:", seleccione "Requerir PIN de inicio con TPM".
5. Para la opción de "Configurar clave de inicio del TPM:", seleccione "Permitir clave de inicio con TPM".
6. Para la opción de "Configurar la clave de inicio y el PIN del TPM:", seleccione "Permitir clave y PIN de inicio con TPM".
7. Haga clic en el botón "Aplicar" y, luego, en el botón "Aceptar" para guardar los cambios en el Editor de directivas de grupo local.

Ahora vamos a agregar el PIN y la encriptación. Sigue las instrucciones en la pantalla e ingresa el pin.
```
> manage-bde -protectors -add c: -rp -tpmandpin
```
Ahora vamos a exportar el RecoveryKey.
```
> manage-bde -protectors -get c:>%userprofile%\desktop\Recoverykey.txt
```
Habilitamos la encriptación.
```
> manage-bde -protectors -enable c:
```
Agregamos un identificador a la instancia de windows. (Opcional)
```
> bcdedit /set {current} description "Joe's Windows 10"
```

```diff
- Nota: Si vas a realizar la segunda instancia entonces recomendamos guardar la primera key de
- la primera instancia. Porque cuando instalamos la segunda instancia entonces su letra aparece
- en el administrador de archivos de la primera instancia. Y hay que meter la key de la primera
- instancia si nos la pide, pero sera solo 1 vez.
```

