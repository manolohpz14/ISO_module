# 1. Teoría Primera Práctica — Descargar Ubuntu en una VM Windows, copiar la ISO a un USB y usarla en el host para instalar una nueva VM (No se entrega, echa en clase)



## Objetivo
- Descargar en la **VM Windows** la ISO de la última versión de Ubuntu Desktop.  
- Pasar la ISO al **host Linux** mediante un **pendrive USB**/ mediante descagar via ftp/ mediante el montado del disco virtual en el que arrancará la máquina cliente en windows/ mediante descarga directa del recurso con wget.  
- Crear una nueva máquina virtual en el host con esa ISO usando **virt-manager**.  


## 1. Descargar la ISO en la VM Windows

1. Inicia la **VM Windows** en virt-manager.  
2. Dentro de Windows, abre un navegador (Edge/Chrome).  
3. Ve a la página oficial: [https://ubuntu.com/download/desktop](https://ubuntu.com/download/desktop)  
4. Descarga la ISO (ejemplo: `ubuntu-25.04-desktop-amd64.iso`).  
5. Guarda la ISO en `C:\Users\<TuUsuario>\Downloads`.



## 2. Preparar un pendrive en el host

1. Inserta el pendrive en el **host físico**.  
2. En el host, identifica el dispositivo:  
   ```bash
   lsblk
   ```

    ```text
    sda    100G
    ├─sda1  96G /
    └─sda2   4G swap
    sdb    16G   ← este es tu pendrive
    └─sdb1 16G
    ```
   

Con el administrados de discos de gnome, prueba a darle formato FAT (el soportado por Windows y distribuciones linux)
Mediante consola de comandos, sería de la siguiente forma, pero NO LO HAREMOS ya que necesitamos verlo previamente...

   ```bash
   sudo apt install -y exfatprogs   # si no está instalado
   sudo umount /dev/sdb   # el dispositivo no puede estar montado en el sistema de archivos
   sudo mkfs.exfat /dev/sdb
   ```



## 3. Pasar el pendrive a la VM Windows (USB passthrough)

1. Abre virt-manager, selecciona tu VM Windows y entra en Detalles de hardware.
2. Con la VM encendida, en el panel izquierdo pulsa Add Hardware → USB Host Device.
3. Elige tu pendrive de la lista y pulsa Finish.
4. Dentro de Windows debería aparecer el pendrive como una nueva unidad (E:\, por ejemplo).



## 4. Copiar la ISO al pendrive en Windows

1. En Windows, abre el Explorador de Archivos.
2. Copia la ISO (ubuntu-25.04-desktop-amd64.iso) desde Downloads al pendrive (E:\).
3. Espera a que termine la copia.
4. Usa Quitar hardware con seguridad en Windows para desmontar el USB.



## 5. Volver a montar el pendrive en el host

1. En virt-manager, quita el USB de la VM (o apaga la VM).
2. El host detectará de nuevo el pendrive.
3. Monta el pendrive: para ello, vete a gnome disk y dale al boton de "play". Dejo también el comando, aunque NO debes USARLO.
  ```bash   
  lsblk              # identifica el dispositivo (ej. /dev/sdb1)
  sudo mount /dev/sdb1 /mnt
  ```
4. Copia la ISO al host: esto se hace manejando archivos de forma normal, no hace falta que uses el comando CP.
  ```bash
    mkdir -p ~/isos
    cp /mnt/ubuntu-25.04-desktop-amd64.iso ~/isos/
    sudo umount /mnt
  ```




## 6. Crear una nueva VM Ubuntu en el host con virt-manager
En caso de que el dispositivo USB no funcione, la ISO está compartida tanto por FTP como SMB y así no tienes que hacer caso a los 5 primeros puntos anteriores.
FTP es un protocolo de red para transferir archivos entre un cliente y un servidor. Funciona sobre TCP, típicamente en los puertos 21 (control) y 20 (datos).
Por otro lado, SMB es un protocolo de red para compartir archivos, impresoras y recursos entre computadoras, principalmente en entornos Windows.

### Windows

- `\\<IP-del-servidor>\Publica (smb poner usuario Manolo contraseña 123456)`

- `ftp://alumnosiso:Iso15092025@IP-servidor`


### Ubuntu

- `ftp://alumnosiso:Iso15092025@IP-servidor`

- `smb://<IP-servidor>/Publica`


## 7. Crear una nueva VM Ubuntu en el host con virt-manager
Si todo lo anterior no funciona lo más limpio es decargar la distribución mediante el comando wget, que permitirá la descarga desde terminal. Otra opción comentada al principio es la de
montar directamente el disco windows donde se ha descargado la iso de ubuntu desktop en tu host. Esto puede ser también un tanto catastrófico ya que requiere de que tengáis permisos de administrador, aunque
el proceso en sí, es simple. Para ello, primero, hay que entender que ubuntu no funciona igual. P

### Primero tenemos que entender que es el directorio /mnt en linux.

- /mnt es una carpeta del sistema reservada como punto de montaje.

- Montar = decirle al sistema operativo:

- “Muestra el contenido de este disco/dispositivo dentro de esta carpeta”.

- Si no hay nada montado, /mnt es una carpeta vacía normal en el host.

### Uso en Discos físicos (ejemplo: un pendrive)

Un pendrive aparece como dispositivo, por ejemplo /dev/sdb1. Para montarlo manualmente:

  ```bash
sudo mount /dev/sdb1 /mnt

  ```
Ahora en /mnt ves los archivos del pendrive.

Si creas algo dentro de /mnt, se escribe en el pendrive.

Cuando desmontas (sudo umount /mnt), el contenido del host en /mnt reaparece (si había algo).



### Ahora bien, aunque similar, para montar discos virtual es un poco distinto. Lo primero es instalar mediante apt una libreria:


  ```bash
sudo guestmount -a /var/lib/libvirt/images/mi_vm.qcow2 -i --ro /mnt
  ```
    -a → ruta al disco virtual.
    
    -i → auto-detección de particiones y sistemas de archivos.
    
    --ro → modo solo lectura (muy recomendado si no quieres estropear la VM).
    
    /mnt → punto de montaje en tu host.


### Por ultimo vemos como pasar la informacion de la VM al host

- Navega hasta: 
```bash
/mnt/Users/manolo/Downloads/
```

- Copiar el archivo windows.iso de esa carpeta al Downloads del usuario manolo del host Ubuntu:

```bash
cp "/mnt/Users/manolo/Downloads/windows.iso" "/home/manolo/Downloads/"
```

- Desmontar el disco virtual (es igual que el umount visto en el punto 5.4 de este punto teórico:

```bash
sudo guestunmount /mnt
```



## 8. Crear una nueva VM Ubuntu en el host con virt-manager

Una vez tienes el ISO mediante alguno de los procedimientos anteriores (pen, ftp, montando disco virtual, wget) , se pide que:

1. Abre virt-manager.

2. Pulsa New (Nuevo).

3. Elige Local install media (ISO image or CDROM).

4. Pulsa Browse → Browse Local, selecciona la ISO en ~/isos/.

5. Virt-manager detectará automáticamente que es Ubuntu. Pulsa Forward.

6. Asigna recursos (ejemplo: 4 GB RAM, 2 CPUs, 40 GB disco).

7. 8. Finaliza: la VM arrancará desde la ISO y verás el instalador de Ubuntu.

8. Sigue los pasos de clase para instalar Ubuntu. Recuerda cambiar el idioma del teclado y descargar el paquete gráfico.

9. En caso de que algo vaya mal con gnome-disk, mátalo el proceso con: pkill gnome-disk

---

