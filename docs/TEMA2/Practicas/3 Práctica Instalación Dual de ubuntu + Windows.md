# 3 Práctica: Instalación de Ubuntu (sin LUKS, sin LVM) en UEFI/GPT + Dual Boot con Windows

## Pasos

Video a seguir: https://drive.google.com/file/d/1Dc1_ymx2eRcDjbyKJYRrLrS7X0Ya5Tgv/view?usp=sharing

(MUY IMPORTANTE) sigue los pasos del video pero lee estas instrucciones. <u>En el video hay dos cosas que he introducido nuevas y antes no estaban</u>

- La primera es cómo compartir una carpeta entre guest y host para pasarte la iso, más abajo se detalla al dedillo (en los siguientes puntos)

- La segunda y más importante de todas es que, a la hora de añadir el disco de la práctica anterior donde instalaste UBUNTU particionado, deberás añadirlo con el tipo de BUS "SATA", si lo añades como "Virtio", Windows no lo detectará y tendrás que usar Drivers específicos de Virtio desarrollados por RED HAT (esto salía mencionado en la tería)

  ![image-20251016131505788](/home/manolo/.config/Typora/typora-user-images/image-20251016131505788.png)

- Una vez has instalado Ubuntu (práctica anterior, deberás darle + tamaño al disco en el que está ubuntu instalado,para ello, ejercutar: qemu-img resize /var/lib/libvirt/images/tu_vm.qcow2 +40G )

- Como devía antes, a la hora de añadir el disco de la práctica anterior donde instalaste UBUNTU particionado, deberás añadirlo con el tipo de BUS "SATA", si no, Windows no lo detectará y tendrás uq eusar Drivers específicos de Virtio desarrollados por RED HAT (esto salía mencionado en la tería)

- Ojo, debereis descargar la iso oficial de Windows, para ello, teneis que descargarla desde un Guest "Linux" que posea por defecto navegador (ya que todavía no hemos visto las herramientas para instalar paquetes) y enlazar a una carpeta compartida con tu host, para así llevarte la ISO a tu máquina real. El video a seguir para esto está en la teoría y este: https://www.youtube.com/watch?v=CdWjyJCBYA0 (minuto 6)

- En caso de que lo anterior no vaya, descargad la iso desde aqui: https://drive.google.com/file/d/1H0AH9SUJ2XsJ5_Fscm8FLb87FtMKuYwJ/view?usp=drive_link

- Una vez ya veis la carpeta /mnt/compartir, es posible que al pegar la iso en la carpeta "/mnt/compartir" necesiteis ejecutar el siguiente comando para copiar

```bash
sudo cp ruta_origen (donde teneis la iso) ruta destino (en este caso /mnt/compartir)
```

 Es decir, si vuestra iso se llama "Windows_11.iso" y está en descargas, para pegarla en /mnt/compartir deberéis hacer:

```bash
sudo cp /home/(tu usuario)/Descargas/Windows_11.iso /mnt/compartir
```

  Esto os copiará la iso en vustra carpeta /mnt/compartir que se podrá ver en el host.

- Posteriormente, se procede como en el video y se pide instalar Windows en el espacio no asignado de la particion, para ello, cuando pida dónde instalar, selecciona el espacio no asignado. Vamos a explicar esto un poco más:

  - Windows **no toca la partición EFI existente**, solo añadirá sus archivos de arranque en `/boot/efi/EFI/Microsoft/Boot/`.

  - También creará sus particiones típicas:
    - **MSR** (16 MB, reservada).
    - **C:** (NTFS, donde va Windows).
    - **Recovery** (~500 MB, para herramientas de recuperación).
    
    Al final de la instalación, muestra en Gparted estas particiones que se han creado y explícalas.

- Una vez instalado windows, vete el boot menú de tu disco virtual y selecciona ubuntu por defecto. ¿Sale siempre grub cada vez que enciendes la máquina virtual? Realiza las configuraciones de Grub (es decir, sigue los pasos que yo hago en el video)

  

**¡Haced capturas a pantalla completa del proceso de instalación. En el video que os dejo se ve qué tenéis que hacer, paso a paso!**