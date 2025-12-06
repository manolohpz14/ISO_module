### **2.**

Solución: B
 ¿Qué se consigue liberando la memoria ocupada por un proceso que ha finalizado?

A) Aumentar la velocidad del reloj del procesador
 B) Evitar pérdidas de rendimiento por falta de espacio
 C) Guardar copias automáticas de los datos liberados
 D) Permitir la ejecución de código privilegiado

### **3.**

Solución: C
 ¿Cuál es la principal diferencia entre un sistema monousuario y uno multiusuario?

A) B y D son correctas
 B) El número de núcleos del procesador
 C) El número de usuarios que pueden utilizar el sistema a la vez
 D) La cantidad de memoria RAM disponible para cada usuario

### **5.**

Solución: A
 ¿Qué propósito tenía el proyecto GNU cuando se inició?

A) Crear un sistema operativo libre y compatible con UNIX
 B) Desarrollar un kernel comercial
 C) Diseñar hardware de red libre
 D) Crear un lenguaje de programación  para uso comercial y elaboración de sistemas operativps

### **8**

Solución: B
 ¿Qué componente proporciona una capa de abstracción entre hardware y usuario?

A) El shell o CLI
 B) El sistema operativo
 C) El compilador
 D) El firmware

### **9**

Solución: C
 ¿Cuál es una ventaja destacada de las distribuciones basadas en Debian, como Ubuntu?

A) Utilizan el formato de paquetes .rpm
 B) Se enfocan en servidores de alto rendimiento
 C) Buscan un equilibrio entre facilidad de uso y soporte
 D) Son exclusivamente para sistemas embebidos

### 10.

Solución: C
 ¿Qué tipo de licencia utiliza GNU/Linux principalmente?

A) Copyright License
 B) Apache License
 C) GPL (Licencia Pública General de GNU)
 D) BSD License

### 11.

Solución: B
 ¿Qué ventaja proporciona la abstracción en un sistema operativo?

A) Permite que los usuarios manipulen directamente el hardware
 B) Simplifica el uso de los recursos físicos mediante funciones virtuales
 C) Incrementa el número de procesadores disponibles
 D) Reduce el tamaño del núcleo del sistema para que su uso sea mas simple

### 12.

Solución: A
 ¿Qué decisión tomó Linus Torvalds en 1992 respecto al núcleo Linux?

A) Cambió su licencia a la GPL
 B) Lo hizo privativo y trabajó en e con su equipo propio de programadores
 C) Abandonó el proyecto Linux
 D) Lo fusionó con UNIX BSD y consiguó un desarrollo exponencial del proyecto

### 13.

Solución: D
 ¿Quién fundó la Free Software Foundation (FSF)?**

A) Linus Torvalds
 B) Ken Thompson
 C) Dennis Ritchie
 D) Richard Stallman

### 14.

Solución: C
 ¿Qué característica fundamental poseen los sistemas operativos distribuidos?

A) No necesitan conexión de red
 B) Se ejecutan en un solo ordenador central
 C) El usuario percibe un único sistema aunque existan varios equipos conectados
 D) Solo permiten un usuario simultáneo

### 15.

Solución: B
 ¿Qué representa un fichero?

A) Un programa en ejecución
 B) Una secuencia de bytes con metadatos almacenada en un dispositivo
 C) Un conjunto de instrucciones para el kernel
 D) Una conexión entre procesos

### 16.

Solución: B
 ¿Cuál es uno de los objetivos fundamentales del sistema operativo?

A) Reducir el consumo eléctrico del hardware
 B) Facilitar el uso del hardware a los usuarios y aplicaciones
 C) Aumentar el tamaño de la memoria RAM
 D) Controlar la velocidad del procesador

### *21.*

Solución: B
 ¿Cuál de las siguientes afirmaciones describe mejor un hipervisor tipo 2 (hosted)?

A) No requiere un sistema operativo anfitrión
 B) Se instala como un programa dentro del sistema operativo principal
 C) Solo se usa en servidores ya que añaden una capa más de abstracción
 D) No permite ejecutar más de una máquina virtual a la vez debido a las limitaciones de software que no poseen los de tipo 1.

### **29.**

Solución: B
 Estás trabajando con una máquina virtual y creas un snapshot antes de instalar un programa nuevo. Más tarde, el programa genera errores y decides volver al snapshot anterior. ¿Qué ocurre al restaurarlo?

A) Se mantiene el programa instalado, pero mejora el rendimiento del disco
 B) Se eliminan los cambios realizados desde el snapshot y la máquina vuelve al estado exacto anterior
 C) Solo se restaura la configuración básicas del snapshot, como la de red o presonalizaciones del sistema
 D) La máquina virtual debe reinstalarse, pero guarda cierta información previa.

### **34.**

Solución: A
En virt-manager: ¿Qué diferencia tiene un pool tipo “disk” respecto a uno tipo “fs”?

A) “disk” usa un disco entero, mientras que “fs” utiliza una partición formateada
 B) “fs” usa disco limpio y “disk” una partición
 C) “disk” solo se usa en servidores y “fs” en escritorios
 D) Ambos son equivalentes, solo cambia el nombre

### **35.**

Solución: B
 ¿Qué es la emulación dentro de la virtualización?

A) Ejecutar máquinas virtuales dentro de otras, como en la Práctica 1 del Tema 2.
 B) Traducir instrucciones de una arquitectura diferente a la del host
 C) Usar hardware compartido entre varias VMs
 D) Dividir la memoria RAM entre varios procesos

### **36**

Solución: B
 Al clonar una máquina virtual, ¿qué se recomienda respecto al disco virtual?

A) Reutilizar el mismo disco para ahorrar espacio
 B) Crear un nuevo disco virtual para el clon
 C) Compartir el disco con el nuevo guest
 D) Siempre usar un formato de disco diferente al original, es decir, si uso qcow, usar raw.

### **39.**

Solución: C
 ¿Qué formato de disco permite compresión, snapshots y cifrado de forma nativa en KVM/QEMU y a su vez, usamos en clase?

A) vmdk
 B) vdi
 C) qcow2
 D) raw

### **40.**

Solución: B
 En tu ordenador con procesador x86_64 (Intel/AMD) decides usar exclusivamente QEMU para probar una versión de Debian ARM. El sistema logra arrancar, aunque el rendimiento es muy bajo.

¿Qué técnica de virtualización está ocurriendo en este caso?

A) Emulación parcial, ya que ambas arquitecturas son iguales
 B) Emulación, porque el host está simulando una arquitectura distinta a la suya
 C) Paravirtualización, usando dispositivos virtio, que son los que siempre usan KVM y QEMU.
 D) Virtualización anidada dentro de KVM.

### **42.**

Solución: B
 ¿Cuál es la principal característica de una versión LTS (Long Term Support)?

A) Recibe actualizaciones continuas de nuevas versiones de software
 B) Ofrece soporte y actualizaciones de seguridad durante varios años
 C) Está enfocada únicamente en usuarios domésticos
 D) No se actualiza nunca después de la instalación

### **45.**

Solución: B
 ¿De dónde provienen los nombres en clave de las versiones de Debian?

A) De nombres de constelaciones
 B) De personajes de la película *Toy Story*
 C) De planetas del sistema solar
 D) De nombres de animales mitológicos

### **50.**

Solución: B
 ¿Cuál era la principal limitación del sistema de archivos FAT16?

A) No podía almacenar archivos mayores de 4 GB
 B) Solo soportaba archivos de hasta 2 GB
 C) No era compatible con Windows
 D) Requería journaling para funcionar correctamente

### **51**

Solución: B
 ¿Qué ventaja clave ofreció FAT32 respecto a FAT16?

A) Soporte para cifrado nativo
 B) Compatibilidad con particiones mayores y más eficiencia
 C) Permitir archivos de más de 4 GB
 D) Compatibilidad exclusiva con Linux

### **53**

Solución: A
 ¿Qué característica distingue a exFAT frente a FAT32?

A) Elimina la limitación de 4 GB por archivo
 B) Solo funciona en discos duros internos
 C) Primer tipo de datos que puede ser leído por macOS
 D) No puede ser leído por macOS

### 56

Solución: B
 ¿Qué papel cumple la partición ESP (EFI System Partition) en sistemas UEFI?

A) Almacenar el kernel del sistema operativo
 B) Contener los archivos de arranque en formato FAT32
 C) Servir como área de intercambio entre discos y uso de GRUB
 D) Guardar copias de seguridad automáticas

### **57**

Solución: B
 ¿Cuál de las siguientes afirmaciones sobre GPT es correcta?

A) Solo admite cuatro particiones primarias
 B) Permite hasta 128 particiones y distribuye metadatos de forma redundante
 C) No es compatible con discos de más de 2 TB
 D) Requiere BIOS tradicional para arrancar

### 59

Solución: B
 ¿Cuáles de los siguientes son formatos de sistema de archivos usados en macOS?

A) NTFS y FAT32
 B) HFS+ y APFS
 C) ext4 y Btrfs
 D) ZFS y XFS



### **65**

Solución: A
 En un ordenador moderno con GPT, ¿por qué sigue existiendo un MBR protector en el sector 0?

A) Para asegurar compatibilidad con software antiguo que solo reconoce MBR y así poder arrancar discos que no son GPT.
 B) Porque el firmware UEFI lo necesita para arrancar, da igual las circunstancias.
 C) Para guardar el kernel de Linux
 D) Para almacenar los controladores SATA

### **66**

Solución: A
 ¿Qué valor decimal equivale al número hexadecimal 1234h?

A) 4660
 B) 5678
 C) 5432
 D) 4096

### **67**

Solución: C
 En el sistema binario, ¿qué base utiliza para contar?

A) Base 8
 B) Base 10
 C) Base 2
 D) Base 16

### **69**

Solución: B
 ¿Qué relación existe entre el sistema binario y el hexadecimal?

A) Cada grupo de 3 bits se traduce en 1 dígito hexadecimal
 B) Cada grupo de 4 bits se traduce en 1 dígito hexadecimal
 C) Cada grupo de 8 bits se traduce en 1 dígito hexadecimal
 D) No tienen relación directa

### **71**

Solución: C
 ¿Cuántos bits usa el código EBCDIC desarrollado por IBM?

A) 6 bits
 B) 7 bits
 C) 8 bits
 D) 9 bits

### **73**

Solución: D
 ¿Cuántos bytes usa UTF-32 por carácter?

A) 1
 B) 2
 C) 3
 D) 4

### **74**

Solución: C
 ¿Cuál es el límite teórico de memoria en una arquitectura de 64 bits?

A) 4 TB
 B) 256 TB
 C) 16 exabytes
 D) 64 GB

### **75**

Solución: A
 En un sistema con HyperThreading, ¿qué diferencia hay entre núcleos físicos y lógicos?

A) Los lógicos son procesadores virtuales que comparten recursos con los físicos
 B) Los físicos son simulados por software
 C) Cada núcleo físico contiene un solo hilo lógico
 D) No hay ninguna diferencia

### **78.**

Solución: A
 ¿Qué característica añadió el ASCII extendido (ISO 8859-1) respecto al ASCII original?

A) Duplicó el número de caracteres de 128 a 256, añadiendo letras acentuadas y símbolos europeos
 B) Redujo el número de bits por carácter, para otorgar simplicidad al procesamiento de caracteres
 C) Eliminó las letras mayúsculas
 D) Hizo incompatible el uso de tildes

### **79.**

Solución: A
 ¿Qué objetivo tuvo la creación de Unicode en 1991?

A) Representar todos los caracteres y símbolos del mundo en una codificación única
 B) Mejorar la compresión de archivos de texto, que usaban base 64
 C) Reemplazar el formato binario de imágenes
 D) Crear un lenguaje de programación único y sencillo

### **81.**

Solución: C
 ¿En qué parte del ciclo de instrucción se ejecuta la operación matemática o lógica?

A) Fetch
 B) Decode
 C) Execute
 D) Update

### **82.**

Solución: B
 ¿Qué característica define la arquitectura de von Neumann?

A) Memoria separada para datos e instrucciones
 B) Uso de una única memoria para datos e instrucciones
 C) Procesamiento paralelo de varias instrucciones
 D) Almacenamiento en ROM

### **84.**

Solución: A
 Cuando ejecutas Firefox, el sistema crea varios procesos e hilos. ¿Qué sucede si matas (kill) el proceso principal?

A) Se cierran todas las pestañas y procesos asociados
 B) Ninguna de las respuestas es correcta
 C) El navegador sigue funcionando en segundo plano
 D) Se borra el historial de navegación, las cookies y todos los datos de la aplicación. En esencia, debes reinstalar

### 86

Solución: B
 ¿Cuál es la principal diferencia entre un programa y un proceso?
 A) Un proceso es un conjunto de instrucciones almacenadas en disco y un programa está en memoria
 B) Un programa es un conjunto de instrucciones pasivo; un proceso es el programa en ejecución
 C) Ambos son lo mismo
 D) Un proceso solo existe en sistemas Linux

### **89**

Solución: B
 ¿Qué significa el identificador PID?

A) Physical Instruction Data
 B) Process Identifier
 C) Primary Input Device
 D) Parallel Instance Descriptor

### 90

Solución: C
 ¿Qué indica el valor PPID en un proceso del sistema operativo?

A) El número total de procesos en ejecución
 B) El identificador del proceso principal del sistema (PID 1)
 C) El identificador del proceso que creó o inició ese proceso (proceso padre)
 D) La prioridad con la que se ejecuta un hilo

### **92.**

Solución: B
 Si ejecutas el comando `ps -eo comm,%cpu,%mem --sort=-%mem | head -15`, ¿qué información obtienes?

A) La lista de usuarios conectados, filtrada a 15
 B) Los 15 procesos que más memoria consumen
 C) Los 15 procesos que más cpu consumen
 D) B y C correctas

### **93.**

Solución: A
 ¿Qué estados puede tener un proceso en Linux?

A) R (ejecución), S (suspendido), Z (zombi), T (detenido)
 B) A (activo), I (inactivo), P (pausado), F (fallo)
 C) N (nuevo), R (read), D (dead), C (close)
 D) Solo activo o detenido

### **94**

Solución: B
 ¿Qué diferencia hay entre 1 GB y 1 GiB?

A) No hay diferencia, ambos equivalen a lo mismo
 B) 1 GiB usa base 2 (1024³), mientras que 1 GB usa base 10 (1000³)
 C) 1 GiB se usa solo en Linux y 1 GB solo en Windows (por defecto)
 D) 1 GiB es mayor que 1 TB

### **95.**

Solución: B
 ¿Por qué un disco duro de “500 GB” suele mostrar menos capacidad real al instalarse en un ordenador?

A) Porque parte del espacio se reserva para el sistema operativo lo que confunde a la hora de visualizar la memoria
 B) Porque el fabricante usa GB (decimal) y el sistema muestra GiB (binario)
 C) Porque el fabricante usa GB (binario) y el sistema muestra GiB (decimal)
 D) A y C son correctas

### **97.**

Solución: A
 Si un disco tiene 500.107.862.016 bytes, ¿cuántos GiB tiene aproximadamente?

A) 465 GiB
 B) 480 GiB
 C) 500 GiB
 D) 512 GiB

### **98**

Solución: D
 Convierte 8192 bytes a KiB.

A) 2 KiB
 B) 4 KiB
 C) 6 KiB
 D) 8 KiB

### **100.**

Solución: C
 ¿Cuál sería el tamaño en GB de un disco que tiene 2 GiB?

A) 1,86 GB
 B) 2,00 GB
 C) 2,14 GB
 D) 2,24 GB

### **101.**

Solución: B
 Si un fabricante anuncia un disco de 1 TB, ¿cuántos GiB** reales verías en tu sistema operativo?

A) 1000 GiB
 B) 931 GiB
 C) 1024 GiB
 D) 950 GiB

### **108**

Solución: B
 Convierte 2048 MiB a GiB.

A) 1 GiB
 B) 2 GiB
 C) 4 GiB
 D) 1,5 GiB

### **112**

Solución: A
 Convierte el número decimal 200₁₀ a binario.

A) 11001000₂
 B) 11000100₂
 C) 11100000₂
 D) 10101000₂

### **114**

Solución: B
 Convierte el número decimal 240₁₀ a hexadecimal.

A) EA
 B) F0
 C) F2
 D) E8

### **116**

Solución: B
 Convierte el número hexadecimal 96₁₆ a binario.

A) 10011001₂
 B) 10010110₂
 C) 10011110₂
 D) 10001110₂

### **118**

Solución: A
 Convierte el número decimal 175₁₀ a hexadecimal.

A) AF
 B) AE
 C) AD
 D) B0

### **119**

Solución: A
 Convierte el número hexadecimal A5₁₆ a binario.

A) 10100101₂
 B) 11000101₂
 C) 10010101₂
 D) 10100011₂

### **120**

Solución: B
 Convierte el número binario 10011110₂ a decimal.

A) 156
 B) 158
 C) 160
 D) 162

### **122**

Solución: B
 Convierte el número hexadecimal B4₁₆ a decimal.

A) 178
 B) 180
 C) 182
 D) 184

### 124

Solución: D

¿Qué es GRUB?

A) Un sistema de archivos usado en Linux
 B) Un gestor de arranque que permite seleccionar y cargar sistemas operativos Linux
 C) Un intérprete de comandos
 D) Ninguna de las anteriores

### 125

Solución: A

En GRUB, ¿qué representa la opción os-prober?

A) Un módulo que detecta automáticamente otros sistemas operativos instalados
 B) Un comando que muestra el kernel en ejecución
 C) Una utilidad para clonar discos
 D) Un paquete a instalar que borra entradas antiguas del GRUB para evitar causar conflictos con LVM

### 126

Solución: B

¿Se puede usar GParted solo con una ISO sin instalar el sistema operativo?

A) No, GParted solo funciona desde un sistema ya instalado
 B) Sí, puede ejecutarse desde una ISO en modo Live
 C) Solo si el disco está vacío
 D) Solo en versiones de pago



### 127

Solución: B

Si las particiones ocupan todo el espacio del disco, ¿el backup GPT se copiará correctamente?

A) Sí, siempre se guarda al principio del disco
 B) No, porque el espacio al final del disco está ocupado
 C) Sí, el GPT no usa espacio físico
 D) Solo si el disco es mayor de 2 TB



### 128

Solución: C

¿Qué herramienta hemos usado en clase para el particionado GPT?

A) fdisk
 B) gdisk
 C) gparted
 D) osprove

### 129

Solución: B

En el comando `dd`, ¿qué significan los parámetros if, of y bs?

A) *if* = imagen final, *of* = origen físico, *bs* = tamaño del bloque
 B) *if* = input file (archivo de entrada), *of* = output file (archivo de salida), *bs* = block size (tamaño de bloque)
 C) *if* = información formateada, *of* = opción forzada, *bs* = bitstream
 D) *if* = índice de formato, *of* = origen fijo, *bs* = buffer secundario

### 130

Solución: A

¿Cuáles son operaciones permitidas con particiones en una herramienta como GParted?

A) Crear, eliminar, redimensionar, mover y formatear
 B) Solo crear, borrar y redimensionar
 C) Solo renombrar y comprimir
 D) Ninguna. Gparted no es una herramienta destinada al particionado de discos.
