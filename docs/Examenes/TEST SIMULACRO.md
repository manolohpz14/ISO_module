### **1.** ¿Qué elemento del sistema operativo decide qué proceso usa la CPU en cada momento?

A) El gestor de memoria
 B) El planificador de procesos
 C) El intérprete de comandos
 D) El sistema de archivos

### **4.** ¿Qué característica permite a varios equipos compartir recursos como si fuesen uno solo?

A) Multitarea
 B) Virtualización
 C) Sistema distribuido
 D) Multiprocesamiento

### **6.** ¿Qué elemento fue clave para que UNIX se volviera portátil?

A) Su compatibilidad con Windows
 B) Estar escrito en lenguaje C
 C) Su interfaz gráfica avanzada
 D) Su integración con Internet

### **7.** ¿Qué sistema BSD es conocido por su extrema portabilidad?

A) FreeBSD
 B) OpenBSD
 C) NetBSD
 D) DragonFly BSD

### **17** ¿Qué función cumple el hipervisor dentro de un sistema de virtualización?

A) Controlar la velocidad de red del host
 B) Gestionar los recursos hardware y repartirlos entre las máquinas virtuales
 C) Servir como sistema operativo invitado
 D) Ejecutar el arranque del sistema anfitrión

### **19.** ¿Qué recurso tiene mayor influencia en el número de máquinas virtuales que puede soportar un host?

A) La tarjeta gráfica
 B) La memoria RAM del guest
 C) El firmware del guest
 D) El número de núcleos lógicos del host

### **18.** ¿Qué diferencia principal hay entre una máquina “guest” y una “host”?

A) La guest gestiona hardware físico y la host es simulada
 B) La host es la máquina real que ejecuta las virtuales
 C) Ambas son sistemas virtualizados iguales
 D) La guest es el sistema operativo base del ordenador

### **20.** ¿Qué caracteriza a un hipervisor de tipo 1 (bare metal)?

A) Se ejecuta dentro de un sistema operativo anfitrión
 B) Se instala directamente sobre el hardware físico
 C) Solo funciona con equipos de escritorio
 D) Depende del sistema operativo host para acceder a la CPU

### **22** En la paravirtualización, los dispositivos virtuales...

A) Emulan hardware físico, siempre idéntico al real
 B) Son versiones optimizadas que reducen la sobrecarga del sistema
 C) Son incompatibles con las CPUs modernas
 D) Requieren traducción binaria constante

### **23** ¿Qué hace la emulación en un entorno virtualizado?

A) Ejecuta directamente las instrucciones de la CPU física
 B) Traduce instrucciones entre arquitecturas diferentes
 C) Solo acelera el acceso a memoria
 D) Reparte recursos entre máquinas del mismo tipo

### **24** ¿Qué papel cumple KVM dentro de la combinación QEMU/KVM?

A) Es la interfaz gráfica de gestión
 B) Es el módulo del kernel que convierte a Linux en hipervisor
 C) Es el emulador de arquitecturas distintas
 D) Es un formato de disco virtual

### **25** ¿Qué ventaja aporta el formato de disco **qcow2** frente al formato **raw**?

A) Ocupa todo el espacio asignado desde el inicio
 B) Ofrece mayor rendimiento pero menos flexibilidad
 C) Crece dinámicamente y permite snapshots y compresión
 D) Es exclusivo para VMware

### **26** ¿Qué ocurre si un sistema virtualizado no tiene acceso a las extensiones VT-x o AMD-V?

A) No puede usar KVM
 B) Ejecuta los guests con rendimiento nativo
 C) Puede arrancar máquinas sin hipervisor
 D) Usa directamente la GPU del host y la RAM del host

### **27.** ¿Qué define un “pool de almacenamiento” en libvirt/Virt-Manager?

A) El tipo de red de las máquinas virtuales
 B) El contenedor donde se guardan discos e ISOs virtuales
 C)El contenedor donde se guardan las Maquinas y snapshots
 D) La configuración de red de la MV

### **28.** ¿Para qué sirve un snapshot en una máquina virtual?

A) Aumentar la velocidad del disco virtual
 B) Hacer una copia exacta del estado del sistema en un momento dado
 C) Guardar solo la configuración de red
 D) Convertir una máquina física en virtual

### **30** Creas dos discos virtuales de 20 GB: uno en formato **raw** y otro en formato **qcow2**. Al comprobarlos con el comando `du -h`, observas que el primero ocupa casi 20 GB y el segundo solo unos pocos megas.

¿Qué conclusión puedes sacar?

A) El formato **qcow2** es dinámico y ocupa solo el espacio realmente utilizado, mientras que **raw** reserva todo el tamaño asignado desde el inicio
 B) El formato **qcow2** tiene un error de lectura y no muestra el tamaño real
 C) El formato **raw** comprime automáticamente el contenido del disco
 D) Ambos formatos ocupan lo mismo, pero `du` no puede mostrarlo correctamente

### **31** ¿Qué representa la virtualización anidada?

A) Ejecutar máquinas virtuales dentro de otras máquinas virtuales
 B) Ejecutar varios hipervisores en el mismo nivel de hardware
 C) Compartir discos entre distintos hosts
 D) Usar un emulador sin hipervisor

### **32.** En un sistema QEMU/KVM, ¿qué función cumple **virtio**?

A) Emular hardware físico antiguo para compatibilidad
 B) Proporcionar dispositivos virtuales optimizados para mejorar el rendimiento de E/S
 C) Limitar el acceso de las máquinas virtuales al hardware del host
 D) Sustituir al hipervisor en la gestión de CPU y memoria

### **33.** En un entorno de virtualización, ¿qué caracteriza a un **pool de tipo “dir”** en libvirt/Virt-Manager?

A) Utiliza un disco físico completo sin sistema de archivos
 B) Es una carpeta del sistema donde se guardan discos e ISOs
 C) Requiere un USB preformateado en ext4
 D) Solo puede usarse para snapshots

### **37.** ¿Qué ventaja ofrece **KVM** al integrarse con QEMU?

A) Permite ejecutar arquitecturas distintas como ARM en un host x86
 B) Convierte a Linux en un hipervisor tipo 1
 C) Sustituye la interfaz gráfica de Virt-Manager
D) Permite emular cualquier tipo de arquitectura.

### **38.** ¿Qué diferencia principal hay entre **virtualización completa** y **paravirtualización**?

A) La completa emula hardware real; la paravirtualización usa interfaces diseñadas para VMs
 B) La paravirtualización solo existe en hipervisores tipo 1
 C) La completa necesita virtio; la paravirtualización no
 D) Ambas técnicas dependen exclusivamente del sistema operativo host

### **41.** ¿Qué caracteriza a una distribución Rolling Release?

A) Se actualiza solo cuando se lanza una nueva versión estable cada varios años
 B) Recibe actualizaciones constantes y no tiene versiones fijas
 C) No permite actualizar paquetes una vez instalada
 D) Solo está pensada para servidores de larga duración

### **44.** ¿Qué tipo de entorno suele usarse para distribuciones LTS?

A) Entornos de desarrollo rápido y prueba
 B) Servidores y entornos de producción que requieren estabilidad
 C) Equipos de laboratorio o pruebas experimentales
 D) Máquinas virtuales con poca RAM

### **46.** ¿Qué relación tiene el nombre de cada versión de **Ubuntu** con su número (por ejemplo, 22.04)?

A) El número representa el kernel que usa
 B) Indica año y mes de lanzamiento (AA.MM)
 C) Representa la cantidad de actualizaciones anuales
 D) Es un código interno sin significado

### **47.** ¿Qué patrón siguen los nombres de Ubuntu?

A) Dos palabras que empiezan con la misma letra: un adjetivo y un animal
 B) Una palabra aleatoria de origen africano
 C) El nombre de una constelación y un planeta
 D) Tres letras que identifican el kernel

### **48** ¿Qué tipo de versión es **Ubuntu 22.04 LTS**?

A) Rolling Release
 B) Versión de soporte extendido de 5 años
 C) Versión experimental sin actualizaciones
 D) Beta permanente

### **49** ¿Cuál es el nombre en clave oficial de **Ubuntu 24.04 LTS**?

A) Jammy Jellyfish
 B) Noble Numbat
 C) Lunar Lobster
 D) Mantic Minotaur

### **54** ¿Qué es una **partición** en un disco duro?

A) Un archivo comprimido con formato NTFS
 B) Una división lógica que organiza el espacio de almacenamiento
 C) Una copia de seguridad del sistema
 D) Un formato de datos que define cómo se guardan los archivos

### **55.** ¿Cuál era la principal limitación del esquema de particiones **MBR**?

A) Solo permitía un sistema operativo por disco
 B) Solo permitía cuatro particiones primarias y discos de hasta 2 TB
 C) No era compatible con discos SSD
 D) No funcionaba con FAT32

### **58.** Quieres formatear un **pendrive** para usarlo indistintamente en **Windows, macOS y Linux**.

¿Qué formato deberías elegir para lograr la **mayor compatibilidad**?

A) NTFS
 B) APFS
 C) exFAT
 D) ext4

### **60** ¿Cuántas particiones primarias puede definir como máximo el esquema **MBR**?

A) 8
 B) 4
 C) 16
 D) 2

### **61** ¿Qué sistema reemplazó al MBR en los equipos modernos?

A) BIOS
 B) GPT (GUID Partition Table)
 C) FAT32
 D) RAID

### **62** Estás instalando Linux en un ordenador nuevo con firmware **UEFI**.

El instalador te pide crear una partición **ESP (EFI System Partition)**.
 ¿Qué formato debes asignarle?

A) ext4
 B) FAT32
 C) NTFS
 D) exFAT

### **63** Al intentar instalar Windows en un equipo UEFI, el instalador muestra el error:

“Windows no puede instalarse en este disco. El disco seleccionado tiene una tabla de particiones MBR.”
 ¿Qué solución corresponde?

A) Convertir el disco de MBR a GPT
 B) Formatear el disco en FAT32
 C) Desactivar la partición EFI
 D) Cambiar el sistema de archivos a exFAT

### **68** En el sistema **hexadecimal**, ¿qué letra representa el valor decimal 15?

A) E
 B) F
 C) D
 D) G

### **70** ¿Cuántos caracteres puede representar una codificación de **7 bits**, como ASCII original?

A) 64
 B) 127
 C) 128
 D) 255

### **72** ¿Qué formato de codificación utiliza entre 1 y 4 bytes por carácter y es el más usado en Internet?

A) UTF-8
 B) UTF-16
 C) UTF-32
 D) ASCII extendido

### **76.** En una arquitectura de **16 bits**, el procesador puede direccionar un máximo de **64 KB** de memoria.

Siguiendo la misma lógica, ¿por qué una arquitectura de **32 bits** tiene un límite teórico de **4 GB** de RAM?

A) Porque 2³² direcciones posibles equivalen a 4.294.967.296 bytes (4 GB)
 B) Porque 32 bits son suficientes para direccionar 4 registros
 C) Porque cada bit representa 1 KB de memoria
 D) Porque el sistema operativo reserva 4 GB por proceso

### **77.** ¿Cuántos bits usa el **ASCII original** para representar cada carácter?

A) 6 bits
 B) 7 bits
 C) 8 bits
 D) 16 bits

### **80.** ¿Qué parte de la CPU se encarga de **realizar operaciones matemáticas y lógicas** como suma o comparaciones?

A) Unidad de control (CU)
 B) Unidad aritmético-lógica (ALU)
 C) Registro de instrucción (IR)
 D) Contador de programa (PC)

### **83.** ¿Qué es un **hilo (thread)** en un programa?

A) Un programa completo almacenado en disco
 B) Una secuencia de instrucciones que se ejecutan de forma independiente dentro de un proceso
 C) Una copia exacta del sistema operativo
 D) Una interrupción generada por hardware

### **85.** ¿Qué es un **proceso** en un sistema operativo?

A) Un programa que está almacenado en disco
 B) Un programa en ejecución con recursos asignados (CPU, memoria, etc.)
 C) Un hilo que se ejecuta dentro de otro hilo
 D) Una tarea que siempre corre en segundo plano

### **87.** Si un proceso tiene 5 hilos, ¿cuántos **TID** y **PID** diferentes existirán?

A) 5 TID y 1 PID
 B) 1 TID y 5 PID
 C) 5 TID y 5 PID
 D) 1 TID y 1 PID

### **88.** ¿Qué representa el **TID** (Thread ID)?

A) El identificador del proceso que contiene varios hilos
 B) El identificador único de un hilo dentro de un proceso
 C) El identificador global de todos los hilos del sistema
 D) El número de prioridad de ejecución

### **91.** ¿Qué diferencia principal hay entre **GUI** (Interfaz Gráfica) y **CLI** (Interfaz de Línea de Comandos)?

A) GUI usa texto; CLI solo iconos
 B) GUI se maneja con comandos; CLI con ratón y teclado
 C) GUI es visual e intuitiva, CLI es textual y más directa
 D) Ambas funcionan solo en servidores

### **99** Convierte **5 MiB** a **KiB**.

A) 2048 KiB
 B) 4096 KiB
 C) 5120 KiB
 D) 1024 KiB

### **103.** En la conversión binaria, ¿por qué se usa **1024** en lugar de **1000**?

A) Porque los sistemas digitales trabajan con potencias de 2
 B) Porque 1024 es más fácil de representar en decimal
 C) Porque 1000 es un valor reservado
 D) Porque los discos duros usan base 4

### **104** Convierte **2 GiB** a **GB** (decimal).

A) 2,00 GB
 B) 1,86 GB
 C) 2,14 GB
 D) 1,75 GB

### **105** Convierte **500 GB (decimal)** a **GiB (binario)**.

A) 500 GiB
 B) 465 GiB
 C) 512 GiB
 D) 476 GiB

### **106** Si un disco tiene **1 TB** anunciado por el fabricante, ¿qué mostrará el sistema operativo en **TiB** reales?

A) 1 TiB
 B) 0,91 TiB
 C) 0,95 TiB
 D) 1,05 TiB

### **107** Convierte **1024 MiB** a **GB (decimal)**.

A) 1,00 GB
 B) 0,98 GB
 C) 1,02 GB
 D) 1,10 GB

### **109.** Convierte el número binario **11001010₂** a decimal.

A) 198
 B) 202
 C) 208
 D) 210

### **110** Convierte el número binario **11110000₂** a hexadecimal.

A) F0
 B) F1
 C) F2
 D) E0

### **111** Convierte el número hexadecimal **7F₁₆** a decimal.

A) 124
 B) 125
 C) 126
 D) 127

### **113** Convierte el número hexadecimal **C8₁₆** a decimal.

A) 192
 B) 198
 C) 200
 D) 202

### **115** Convierte el número binario **10101100₂** a hexadecimal.

A) A4
 B) AC
 C) B0
 D) BC

### **117** Convierte el número binario **11111111₂** a decimal.

A) 254
 B) 255
 C) 256
 D) 257

### **121** Convierte el número decimal **255₁₀** a hexadecimal.

A) FF
 B) F0
 C) FE
 D) EF

### **123** Convierte el número decimal **100₁₀** a binario.

A) 1100100₂
 B) 1110010₂
 C) 1011000₂
 D) 1100000₂

