# 3 Directorios principales desde la raíz `/` en Ubuntu

Conviene destacar las siglas FHS (Filesystem Hierarchy Standard) (FHS).

- **`/` (raíz)**

  El directorio `/`, conocido como raíz, es el punto de partida de todo el sistema de ficheros en Ubuntu y en cualquier sistema basado en Linux. A diferencia de Windows, que organiza los discos por letras (C:, D:, E:), en Linux todo el sistema de archivos cuelga de un único árbol que comienza en `/`. Dentro de la raíz se montan todas las demás carpetas, discos, dispositivos. Esto significa que, aunque tengas varios discos duros o memorias USB, no aparecerán como unidades independientes con letras, sino que se integrarán dentro de la jerarquía del sistema bajo un directorio específico (por ejemplo, `/media/usuario/USB`). Es importante aclarar que la raíz no almacena directamente el “núcleo” del sistema (el kernel está en realidad en `/boot`), sino que actúa como el contenedor inicial que da acceso a todos los directorios esenciales. En una instalación típica de Ubuntu, la partición raíz suele contener:

  - El propio sistema operativo.
  - Todos los directorios principales (`/bin`, `/etc`, `/home`, `/var`, etc.).
  - Archivos fundamentales de configuración y bibliotecas compartidas.

- **`/bin`**

  El directorio `/bin` (abreviatura de *binary*) contiene los binarios esenciales del sistema, es decir, los programas ejecutables básicos que todo usuario y el propio sistema necesitan para funcionar. Estos comandos están disponibles incluso cuando el sistema está en un estado mínimo, como en modo de recuperación, porque son imprescindibles para la administración y operación diaria.

  Dentro de `/bin` se encuentran utilidades fundamentales como:

  - Gestión de archivos: `ls` (listar), `cp` (copiar), `mv` (mover/renombrar), `rm` (eliminar), `cat` (ver contenido).
  - Manipulación de texto: `echo`, `grep`, `more`, `less`.
  - Gestión de directorios: `pwd` (mostrar ruta actual), `mkdir` (crear directorios), `rmdir` (eliminar directorios).
  - Shells: `bash`, el intérprete de comandos más utilizado en Ubuntu.

  Un detalle importante es que, en sistemas modernos basados en el Filesystem Hierarchy Standard (FHS), muchas distribuciones han fusionado `/bin` con `/usr/bin`. Esto significa que en algunas versiones de Ubuntu los binarios se encuentran físicamente en `/usr/bin`, pero `/bin` se mantiene como un enlace simbólico por compatibilidad con programas antiguos. Ese detalle de la fusión entre `/bin` y `/usr/bin` ocurre en muchas distribuciones modernas de Linux, incluyendo Ubuntu desde la versión 16.04 en adelante (aunque se consolidó totalmente en Ubuntu 18.04 y posteriores). El proyecto detrás de esto se llama “usr merge”.

- **`/sbin`**

  El directorio `/sbin` (de *system binaries*) es el complemento de `/bin`, pero orientado a la administración del sistema. Aquí se encuentran los programas ejecutables que normalmente no son necesarios para un usuario común, sino para el superusuario (root) o administradores que requieren herramientas de bajo nivel para mantener el sistema en funcionamiento.

  Entre los comandos más representativos que suelen encontrarse en `/sbin` están:

  - `fsck` → comprobar y reparar sistemas de archivos.
  - `ifconfig` o `ip` → configurar interfaces de red.
  - `mount` y `umount` → montar y desmontar dispositivos.
  - `shutdown` y `reboot` → gestionar el apagado y reinicio del sistema.
  - `mkfs` → crear sistemas de archivos en discos o particiones.

  Un detalle importante es que, al igual que ocurrió con `/bin`, muchas distribuciones modernas (incluido Ubuntu desde la versión 18.04 LTS) han fusionado `/sbin` con `/usr/sbin` como parte del proyecto *usr merge*. Esto significa que `/sbin` ahora suele ser un enlace simbólico hacia `/usr/sbin`, manteniéndose únicamente por compatibilidad con scripts o programas antiguos.

  Para los usuarios normales, los binarios de `/sbin` no suelen estar en la variable de entorno PATH por defecto, lo que significa que si intentan ejecutarlos deben especificar la ruta completa (por ejemplo, `/sbin/ifconfig`). En cambio, el usuario root sí tiene `/sbin` en su PATH y puede acceder a estas herramientas directamente. Entramos en mas detalle de lo anterior:

  En Linux (y en general en sistemas Unix), la variable de entorno `PATH` es una lista de directorios que la shell revisa automáticamente cuando intentas ejecutar un comando.

  Por ejemplo:

  - Si escribes `ls`, la shell busca `ls` dentro de todos los directorios definidos en tu `PATH` (como `/bin`, `/usr/bin`, etc.).
  - Si el binario se encuentra en alguno de esos directorios, se ejecuta.
  - Si no está en el `PATH`, aunque exista en el disco, el sistema no lo encuentra a menos que pongas la ruta completa.

  En Linux (y en general en sistemas Unix), la variable de entorno `PATH` es una lista de directorios que la shell revisa automáticamente cuando intentas ejecutar un comando.

- **`/boot`**

  El directorio `/boot` contiene todos los archivos esenciales para el proceso de arranque del sistema operativo. Aquí se encuentra el kernel de Linux en forma de archivo ejecutable, acompañado de la imagen de memoria inicial (`initramfs`), que es un sistema de archivos temporal cargado en memoria al inicio y que permite al kernel acceder a los controladores y módulos necesarios antes de montar el sistema real. Se carga en memoria RAM durante los primeros segundos del arranque de Linux.

  También en `/boot` se aloja el cargador de arranque, generalmente GRUB (GRand Unified Bootloader) en el caso de Ubuntu, el cual se encarga de mostrar el menú de inicio y de cargar el kernel junto con los parámetros adecuados. Si tienes más de un sistema operativo instalado, GRUB es el que ofrece la posibilidad de elegir entre ellos al encender el ordenador.

  En la práctica, sin la carpeta `/boot`, Ubuntu no podría arrancar, ya que el firmware (BIOS o UEFI) no tendría forma de acceder al kernel ni al cargador de arranque. Por eso, en muchas configuraciones, sobre todo en servidores o sistemas con cifrado completo, `/boot` suele colocarse en una partición independiente para garantizar que siempre esté accesible en el arranque.

- **`/dev`**

  El directorio `/dev` es uno de los más particulares de Linux, ya que contiene los llamados archivos de dispositivo. En este sistema, todo se trata como un archivo: desde un documento hasta un disco duro o un puerto de comunicación. Esto significa que los dispositivos físicos (como discos, impresoras o terminales) y algunos dispositivos virtuales tienen una representación dentro de `/dev`.

  Por ejemplo, un disco duro completo puede aparecer como `/dev/sda`, y sus particiones como `/dev/sda1`, `/dev/sda2`, etc. También existen dispositivos especiales como `/dev/null`, que funciona como un “agujero negro”: cualquier información que se envíe allí se descarta inmediatamente. Otro caso es `/dev/zero`, que genera un flujo infinito de ceros, útil en pruebas y configuraciones. Estos archivos no contienen datos en sí mismos, sino que son interfaces de comunicación con el kernel y el hardware.

   

- **`/etc`**

  El directorio `/etc` es el lugar donde se guardan todos los archivos de configuración del sistema en Ubuntu y en la mayoría de distribuciones Linux. A diferencia de Windows, que utiliza un registro centralizado, Linux organiza la configuración en múltiples archivos de texto plano, fáciles de leer y editar con un editor como `nano` o `vim`.

  Dentro de `/etc` se encuentran parámetros críticos del sistema: configuraciones de red, información de usuarios y grupos, ajustes de servicios, demonios y prácticamente todas las aplicaciones instaladas. Por ejemplo, el archivo `/etc/passwd` almacena la lista de usuarios del sistema junto con información básica de cada uno, mientras que `/etc/hosts` permite definir asociaciones entre direcciones IP y nombres de dominio de manera local.

  Muchos "servicios" (palabra ya comentada en apuntes anteriores) tienen su propia carpeta dentro de `/etc`. Por ejemplo, `/etc/ssh/` contiene la configuración del servidor SSH, y `/etc/apache2/` la del servidor web Apache. Esto permite que cada componente del sistema mantenga sus parámetros organizados en un lugar específico.

  Es importante destacar que `/etc` contiene principalmente archivos de configuración global, que afectan a todo el sistema y a todos los usuarios, a diferencia de las configuraciones personales que suelen guardarse en carpetas ocultas dentro del directorio `/home` de cada usuario.

  En resumen, `/etc` es el “cerebro” de la configuración de Linux: cualquier cambio en sus archivos puede modificar profundamente el comportamiento del sistema operativo. Algunos otros archivos importantes son:

  `/etc/sudoers` define quién puede usar `sudo`, desde qué hosts, qué comandos y bajo qué condiciones. Nunca edites `/etc/sudoers` con un editor normal: usa `visudo` (bloquea el archivo y verifica la sintaxis antes de guardar). Además de `/etc/sudoers`, muchos sistemas usan el directorio `/etc/sudoers.d/` para fragmentos de configuración (más manejable y seguro).

- **`/home`**

  El directorio `/home` es el espacio reservado para los archivos y configuraciones personales de cada usuario del sistema. Dentro de él, se crea una carpeta individual para cada cuenta, cuyo nombre coincide con el nombre de usuario. Por ejemplo, si el usuario se llama `juan`, su directorio personal será `/home/juan`. En estas carpetas, cada usuario guarda sus documentos, imágenes, descargas, música y cualquier otro tipo de archivo personal. Además, dentro de cada directorio de usuario existen carpetas ocultas (cuyos nombres empiezan con un punto `.`), que almacenan configuraciones específicas de aplicaciones y del entorno de escritorio. Por ejemplo:

  - `.bashrc` → configuración personalizada de la terminal.
  - `.mozilla/` → configuraciones y perfiles de Firefox.
  - `.config/` → ajustes de aplicaciones modernas según el estándar XDG.

  De esta manera, cada usuario puede tener un entorno completamente personalizado, independiente del resto. Incluso si varias personas usan la misma computadora, cada una tendrá sus preferencias, documentos y configuraciones guardadas en su propia carpeta dentro de `/home`. En instalaciones más avanzadas, es común que `/home` se ubique en una partición separada del sistema. Esto permite que, aunque se reinstale Ubuntu o se actualice la distribución, los archivos y configuraciones personales de los usuarios se mantengan intactos, aumentando la seguridad y la comodidad.

- **`/lib`**

  El directorio `/lib` contiene las bibliotecas compartidas que permiten que los programas y comandos esenciales ubicados en `/bin` y `/sbin` puedan ejecutarse correctamente. Dentro de `/lib` se encuentran archivos con extensiones como `.so` (*shared object*), que representan estas bibliotecas dinámicas. Por ejemplo, funciones matemáticas, manejo de cadenas de texto, acceso a la red o comunicación con el kernel están centralizadas en estas librerías. Esto permite que el sistema sea más ligero, modular y fácil de actualizar: si una biblioteca se corrige o se mejora, todos los programas que dependen de ella se benefician automáticamente.

  Además, en `/lib` también se almacenan los módulos del kernel, que son componentes cargables dinámicamente y que amplían las capacidades del núcleo sin necesidad de reiniciar el sistema. Estos módulos permiten, por ejemplo, añadir soporte para un nuevo tipo de hardware, sistemas de archivos o controladores de red.

  En distribuciones modernas que han adoptado la fusión de directorios (*usr merge*), es común que el contenido de `/lib` se encuentre realmente en `/usr/lib`, manteniéndose `/lib` como un enlace simbólico por compatibilidad.

- **`/media`**

  El directorio `/media` está reservado para el montaje automático de dispositivos externos en Ubuntu y en la mayoría de distribuciones Linux. Cada vez que conectas una memoria USB, un disco duro portátil, una tarjeta SD o incluso un CD/DVD, el sistema crea de manera temporal una carpeta dentro de `/media` y allí monta el contenido del dispositivo para que el usuario pueda acceder a él fácilmente.

  Por ejemplo, si un usuario llamado `juan` conecta una memoria USB llamada *USB*, Ubuntu montará ese dispositivo en la ruta `/media/juan/USB`. Esto significa que todos los archivos de la memoria estarán accesibles en esa carpeta mientras el dispositivo permanezca conectado.

  El proceso es gestionado automáticamente por udev (el sistema que administra dispositivos en Linux) y por los entornos de escritorio como GNOME o KDE, de modo que el usuario no necesita montar manualmente los discos, a diferencia de lo que ocurría en versiones antiguas de Linux. Es importante diferenciar `/media` de `/mnt`. Mientras que `/media` está pensado para los dispositivos externos que se montan de manera automática y temporal, `/mnt` se reserva para montajes manuales realizados por administradores, normalmente en tareas de mantenimiento o pruebas.

- **`/mnt`**

  El directorio `/mnt` se utiliza como punto de montaje temporal en Linux. Tradicionalmente, era el lugar donde los administradores montaban manualmente discos duros, particiones adicionales o incluso recursos de red para poder acceder a ellos de forma momentánea. Por ejemplo, si se conectaba un disco que no se montaba automáticamente, un administrador podía ejecutar un comando como:

  ```bash
   sudo mount /dev/sdb1 /mnt
  ```

  directorio volvía a estar vacío y listo para usarse con otro dispositivo. Aunque hoy en día el montaje automático de dispositivos externos suele hacerse en `/media`, el directorio `/mnt` sigue existiendo y mantiene su función como espacio flexible para pruebas, configuraciones temporales o situaciones en las que un administrador necesita montar sistemas de archivos manualmente.

- **`/opt`**

  El directorio `/opt` está reservado para aplicaciones adicionales que se instalan de forma manual o que no forman parte del sistema base ni del gestor de paquetes de Ubuntu. Su nombre proviene de *optional*, lo que refleja su propósito: albergar software opcional que complementa al sistema.

  A diferencia de otros directorios como `/usr/bin` o `/usr/lib`, que están gestionados directamente por el sistema de paquetes de Ubuntu (APT), `/opt` se utiliza cuando un programa se distribuye en un directorio completo que incluye todos sus binarios, bibliotecas y archivos de configuración. Este método es común en aplicaciones propietarias o comerciales que no se integran al 100 % con el sistema.

  Por ejemplo, si descargas un programa en forma de paquete comprimido o un instalador que no usa APT, probablemente cree su propia carpeta en `/opt`. Dentro de esa carpeta, el software suele tener su propia estructura organizada, por ejemplo:

  - `/opt/google/chrome/` → instalación de Google Chrome.

  Esto permite que dichas aplicaciones estén separadas del resto del sistema y sean fáciles de identificar, actualizar o eliminar simplemente borrando su carpeta correspondiente.

- **`/proc`**

  El directorio `/proc` es un sistema de archivos virtual, lo que significa que no ocupa espacio real en el disco ni contiene archivos convencionales. En lugar de eso, se genera dinámicamente por el kernel de Linux y ofrece una ventana en tiempo real al estado interno del sistema y de los procesos en ejecución.

  Cada archivo o subdirectorio dentro de `/proc` representa información o una interfaz hacia alguna parte del kernel. Por ejemplo:

   - `/proc/cpuinfo` muestra detalles sobre el procesador: modelo, núcleos, velocidad, etc.
   - `/proc/meminfo` ofrece datos sobre la memoria RAM disponible y utilizada.
   - `/proc/uptime` indica cuánto tiempo lleva encendido el sistema.

   Además de los archivos de información general, `/proc` también contiene un subdirectorio por cada proceso en ejecución, identificado por su PID (Process ID). Por ejemplo, `/proc/1234/` corresponde al proceso con PID 1234 y dentro de esa carpeta se encuentran archivos que permiten consultar —e incluso modificar— parámetros de ese proceso en tiempo real.

   Un detalle interesante es que `/proc` no solo sirve para leer información, sino que también permite al administrador interactuar con el kernel. Por ejemplo, el archivo `/proc/sys/` contiene parámetros ajustables del sistema que pueden modificarse temporalmente mediante el comando `sysctl`.

   Es posible consultar información de hardware como el procesador en `/proc/cpuinfo`, que muestra modelo, velocidad y número de núcleos, o la memoria en `/proc/meminfo`, donde aparecen los detalles del uso y disponibilidad de la RAM. También permite obtener datos generales del sistema, como la versión del kernel en `/proc/version`, el tiempo que lleva encendido el equipo en `/proc/uptime`, o la carga promedio en `/proc/loadavg`.

   Además, cada proceso en ejecución tiene su propio directorio dentro de `/proc`, identificado por su PID. Allí se puede ver con qué comando fue iniciado (`cmdline`), qué variables de entorno utiliza (`environ`), qué archivos tiene abiertos (`fd/`) o su consumo de memoria y estado actual (`status`).

   Por último, `/proc` también permite modificar configuraciones del kernel en caliente a través de `/proc/sys/`, como habilitar el reenvío de paquetes en red o ajustar parámetros de seguridad, algo que se suele gestionar con el comando `sysctl`.

- **`/root`**

  El directorio `/root` es el directorio personal exclusivo del superusuario (root) en Linux. Aunque el nombre pueda inducir a error, no debe confundirse con `/`, que es la raíz del sistema de archivos. La diferencia es clara:

  - `/` → punto de partida del sistema, donde se montan todos los directorios y particiones.
  - `/root` → carpeta personal del usuario root, equivalente a `/home/usuario` para cuentas normales.

  Los usuarios normales guardan sus datos en `/home/usuario`, pero la cuenta root no lo hace por dos razones principales:

  1. Disponibilidad en arranque y recuperación: si el sistema se daña y la partición `/home` no está disponible, el superusuario necesita tener su propio espacio dentro de la partición raíz (`/`). De este modo, root siempre puede iniciar sesión y reparar el sistema.
  2. Seguridad y separación: los archivos de configuración y datos de root no deben mezclarse con los de los usuarios normales.

  Por eso `/root` está directamente bajo `/`, y no dentro de `/home`.

  El usuario root es la cuenta administrativa por antonomasia en Linux: tiene UID 0, lo que le otorga permisos ilimitados sobre el sistema. Root puede leer/escribir cualquier archivo, cargar o descargar módulos del kernel, cambiar permisos, crear o borrar usuarios, y más. Por seguridad, muchas distribuciones (como Ubuntu) bloquean la contraseña de root por defecto y recomiendan usar `sudo` para acciones administrativas en su lugar.

  Los datos sobre las cuentas están en `/etc/passwd` (datos públicos de la cuenta) y las contraseñas (hashed) en `/etc/shadow` (accesible solo por root). Si una línea en `/etc/passwd` tiene UID 0, esa cuenta tendrá privilegios root. Aquí entra una clasificación muy importante de comandos.

  - `su` (substitute user): cambia de usuario a otro (por defecto a root). Con `su -` obtienes el entorno de root completo. `su` pide la contraseña de la cuenta objetivo (ej. la contraseña de root).
  - `sudo` (superuser do): permite ejecutar un comando con privilegios elevados sin conocer la contraseña de root, solicitando la contraseña del usuario actual (si está autorizado en sudoers). `sudo` deja un rastro más fino y controlado y es la práctica recomendada hoy en día.

- **`/run`**

  El directorio `/run` es un sistema de archivos temporal que se crea en memoria cada vez que el sistema arranca. Su función principal es almacenar información dinámica sobre procesos y servicios en ejecución, como identificadores de procesos (PID), sockets de comunicación, archivos de bloqueo (*lock files*) y otros datos que los programas necesitan mientras están activos.

  Por ejemplo, cuando un servicio como `sshd` (el servidor de conexiones seguras) se inicia, crea un archivo como `/run/sshd.pid` para indicar qué proceso está corriendo y con qué identificador. De igual forma, servicios como `NetworkManager` o `systemd` almacenan en `/run` información de estado que desaparece en cuanto el sistema se apaga o reinicia.

  Antes de la introducción de `/run`, distintos directorios cumplían este rol: algunos programas guardaban estos datos en `/var/run` o `/var/lock`. Hoy en día, por compatibilidad, esos directorios suelen ser enlaces simbólicos que apuntan a `/run`.

  Es importante tener en cuenta que `/run` reside en un sistema de archivos temporal en RAM (tmpfs), por lo que todo su contenido se pierde automáticamente al reiniciar. Esto garantiza que solo contenga información fresca y relevante para la sesión actual del sistema.

- **`/srv`**

  El directorio `/srv` está destinado a almacenar los datos que sirven los distintos servicios del sistema. Su nombre proviene de *service*, y la idea es que cualquier aplicación que actúe como servidor —ya sea un servidor web, de archivos o de bases de datos— disponga aquí de un espacio estándar para organizar su contenido.

  Por ejemplo, si el equipo funciona como servidor web, los archivos del sitio podrían ubicarse en `/srv/www/`; si actúa como servidor FTP, los datos compartidos estarían en `/srv/ftp/`. La ventaja de esta organización es que permite mantener claramente separados los archivos de servicio de los archivos del sistema operativo y de los datos de los usuarios.

  Cabe señalar que no todas las distribuciones de Linux utilizan activamente `/srv` de forma predeterminada. En Ubuntu, por ejemplo, los servidores web como Apache suelen usar `/var/www/` para sus documentos. Sin embargo, el estándar FHS (Filesystem Hierarchy Standard) recomienda `/srv` como el lugar correcto para almacenar los datos ofrecidos por servicios de red.

- **`/sys`**

  El directorio `/sys` es un sistema de archivos virtual, al igual que `/proc`, pero con un propósito diferente: sirve como interfaz entre el kernel de Linux y los dispositivos y controladores del sistema. 

  Un ejemplo práctico es `/sys/class/net/`, donde cada interfaz de red aparece como un directorio (por ejemplo, `eth0`, `wlan0`). Dentro, se pueden consultar archivos que muestran su estado (si está activa, su dirección MAC, velocidad del enlace) o parámetros que pueden ajustarse dinámicamente.

  A diferencia de `/proc`, que ofrece principalmente información sobre procesos y el estado general del sistema, `/sys` está centrado en la gestión de dispositivos y sus controladores, lo que lo convierte en una herramienta clave para el kernel, los administradores y programas de configuración.

- **`/tmp`**

  El directorio `/tmp` está destinado a almacenar archivos temporales generados tanto por el sistema como por las aplicaciones y los usuarios. Cualquier programa puede crear ficheros en esta carpeta —por ejemplo, instaladores que necesitan espacio intermedio, navegadores web que guardan descargas incompletas, o editores de texto que almacenan copias de seguridad mientras se trabaja en un archivo—.

  Por diseño, todos los usuarios tienen permisos de escritura en `/tmp`, lo que lo convierte en un espacio compartido y de uso abierto. Para evitar problemas de seguridad, los ficheros creados aquí suelen tener permisos restringidos a su propietario, y en muchos sistemas la carpeta está montada con opciones especiales (como `sticky bit`) que impiden a un usuario borrar los archivos de otro.

  Una característica clave es que el contenido de `/tmp` no es permanente: normalmente se borra automáticamente al reiniciar el sistema, ya que suele estar montado en un sistema de archivos temporal en memoria (`tmpfs`). Esto garantiza que no se acumulen datos obsoletos o innecesarios.

- **`/usr`**

  A día de hoy contiene muchos enlaces simbólicos a los comentados anteriormente

- El directorio `/var` está dedicado a almacenar datos variables, es decir, información que cambia de forma constante a medida que el sistema y las aplicaciones se ejecutan. A diferencia de directorios como `/usr`, que suelen contener archivos estáticos y de solo lectura, en `/var` se concentran los ficheros dinámicos y en crecimiento continuo, lo que lo convierte en un punto clave para la administración y el mantenimiento del sistema.

  Dentro de `/var` destacan varios subdirectorios esenciales:

  - `/var/log`: aquí se guardan los registros del sistema, que documentan prácticamente todas las actividades relevantes. Encontramos, por ejemplo, los registros de inicio de sesión, de errores del kernel, de servicios como Apache o SSH, y también los relacionados con la administración mediante `sudo`. En Debian y Ubuntu, las acciones realizadas con `sudo` se registran en `/var/log/auth.log`, mientras que en sistemas como RHEL o CentOS aparecen en `/var/log/secure`. Estos archivos permiten rastrear quién ejecutó qué comando, en qué momento y si la autenticación fue exitosa o fallida, por lo que resultan fundamentales para la auditoría de seguridad y la depuración de problemas.
  - `/var/spool`: almacena colas de procesos que deben ser atendidos, como trabajos pendientes de impresión o mensajes de correo electrónico aún no entregados. Funciona como un área de espera donde los procesos gestionan tareas en curso.
  - `/var/cache`: contiene archivos de caché generados por aplicaciones, que permiten acelerar su funcionamiento al evitar recalcular o descargar de nuevo información. Por ejemplo, los gestores de paquetes como `apt` almacenan aquí los paquetes descargados para instalaciones o actualizaciones.

  Además, `/var` puede incluir otros subdirectorios como `/var/tmp`, usado para temporales que deben sobrevivir a reinicios, o `/var/lib`, donde se guardan datos persistentes de servicios (por ejemplo, las bases de datos de `dpkg` o `apt`).

## 2.4 Instalación de aplicaciones en Linux

En Linux, los programas pueden instalarse de dos formas: a nivel de sistema o solo para un usuario concreto.

Cuando se instalan con el gestor de paquetes, como `apt` en Ubuntu, los archivos se copian en directorios compartidos y quedan disponibles para todos los usuarios del sistema. Los binarios suelen ir a **`/usr/bin`** o **`/usr/sbin`**, las bibliotecas a **`/usr/lib`**, y los datos a **`/usr/share`**. Por ejemplo, si instalas Firefox con `sudo apt install firefox`, su ejecutable se encuentra en `/usr/bin/firefox`, las bibliotecas en `/usr/lib/firefox/` y los iconos o manuales en `/usr/share/firefox/`. Otro caso es el de programas propietarios como Google Chrome, que se instalan en `/opt/google/chrome/` y crean un enlace en `/usr/bin/` para que cualquier usuario pueda ejecutarlos.

## 2.5 Buenas prácticas para particionado de Linux

#### 1. Siempre separar la raíz `/`

La partición **raíz `/`** es obligatoria, ya que contiene el sistema operativo. Lo recomendable es darle un tamaño suficiente para instalar programas y librerías sin preocupaciones: Escritorio: entre 20 y 30 GB suele bastar. Servidor: 50 GB o más, dependiendo de cuántos paquetes o servicios se instalen.

#### 2. Separar `/home` para proteger los datos de usuario

Tener `/home` en una partición independiente permite que los archivos personales y configuraciones de los usuarios sobrevivan incluso si reinstalas o actualizas el sistema operativo.

- Escritorio: darle la mayor parte del espacio disponible.
- Servidor: menos relevante, salvo que haya muchos usuarios locales.

#### 3. Usar una partición `/var` independiente en servidores

En servidores, los logs, cachés y datos variables pueden crecer rápidamente. Si `/var` se llena en la misma partición que `/`, el sistema podría dejar de funcionar. Separar `/var` garantiza que el sistema operativo siga operativo aunque los registros crezcan demasiado. Tamaño recomendado: depende de la carga; **10–20 GB mínimo** en servidores pequeños, más en sistemas críticos.

#### 4. Considerar separar `/tmp` para mayor seguridad

La carpeta temporal `/tmp` es usada por cualquier aplicación. Si un atacante o proceso malicioso llena esta carpeta, podría provocar problemas graves en el sistema. Al poner `/tmp` en una partición separada, puedes montarla con opciones de seguridad como `noexec`, `nosuid` y `nodev`, que reducen riesgos de ejecución de código malicioso. Tamaño: 2–4 GB suele ser suficiente en escritorios, más en servidores que procesen archivos grandes.

#### 5. La partición `/boot` en escenarios especiales

Separar `/boot` no siempre es necesario, pero sí recomendable en:

- Sistemas con cifrado completo (LUKS). Ojo:

  Cuando se utiliza cifrado completo de disco con LUKS (Linux Unified Key Setup), la partición raíz `/` y todo lo que contiene quedan cifrados. Eso incluye el kernel de Linux, la imagen de memoria inicial (`initrd/initramfs`) y el cargador de arranque (GRUB). El problema es que, para poder arrancar el sistema, el firmware (BIOS/UEFI) necesita cargar primero el gestor de arranque y luego el kernel. Pero si `/boot` estuviera dentro de la partición cifrada, el sistema se encontraría en un círculo vicioso: Para cargar el kernel necesitas abrir el cifrado. Pero para abrir el cifrado necesitas cargar primero el kernel y los módulos de LUKS.

  Por eso, en esquemas de cifrado completo se suele crear una partición `/boot` separada y sin cifrar, que contiene: El cargador de arranque (GRUB2). El kernel de Linux. La imagen initramfs, que lleva consigo los módulos necesarios para montar la partición cifrada.

  El flujo sería así:

  1. El firmware (UEFI/BIOS) carga GRUB desde `/boot`.
  2. GRUB carga el kernel y el initramfs, también desde `/boot`.
  3. El initramfs contiene el soporte para LUKS y pide la contraseña de cifrado.
  4. Una vez descifrada la partición raíz, el sistema operativo continúa arrancando normalmente.

  De este modo, solo la carpeta `/boot` permanece sin cifrar, y el resto del disco (incluyendo `/`, `/home`, `/var`, etc.) queda protegido.


#### 6. Swap: ¿partición o archivo?

El swap actúa como memoria virtual cuando se agota la RAM. Hoy en día, muchas distribuciones (incluido Ubuntu) crean un archivo swap en lugar de una partición dedicada, lo que simplifica la gestión. Sin embargo, la partición swap aún tiene usos:

- Para hibernación, es obligatorio que el swap tenga al menos el mismo tamaño que la RAM.
- En servidores o PCs con poca RAM, una partición swap garantiza memoria adicional aunque más lenta.
- Como regla general:
  - RAM < 4 GB → swap = 2× RAM.
  - RAM 4–8 GB → swap = igual a RAM.
  - RAM > 8 GB → swap = 4–8 GB suele bastar

En un entorno más profesional o multiusuario hay que hablar sí o sí de cuotas de disco, RAID y otras estrategias de particionado avanzadas. Vamos a comentarlo.

## 2.6 Cuotas de disco: limitar el uso de espacio por usuario o grupo

Cuando un sistema lo utilizan varios usuarios (p. ej. un servidor educativo o empresarial), es recomendable separar ciertas particiones para poder aplicar cuotas de disco. Esto evita que un único usuario llene todo el sistema y afecte a los demás.

- Lo habitual es separar `/home` en su propia partición y activar cuotas de disco allí.
- También puede hacerse en `/var` si se quiere limitar el uso de espacio por parte de servicios como correo, logs o colas de impresión.
- Ejemplo: si cada usuario tiene 5 GB de cuota en `/home`, aunque copie archivos enormes no afectará al resto del sistema.

Las cuotas se configuran editando `/etc/fstab` para activar la opción `usrquota` o `grpquota` en la partición correspondiente, y luego gestionando con comandos como `edquota` o `repquota`.

## 2.7 RAID: redundancia y rendimiento (lo utilizaremos cuando sepamos más sobre comandos linux y el S.O)

El RAID (Redundant Array of Independent/Inexpensive Disks) es esencial en servidores donde se requiere alta disponibilidad o gran velocidad. La elección del esquema RAID condiciona cómo particionar y distribuir discos:

- **RAID 0 (striping):** divide datos entre discos:  máximo rendimiento pero sin tolerancia a fallos. No recomendable en sistemas críticos.

- **RAID 1 (mirroring):** duplica los datos en dos discos los datos se duplican en dos discos. Por ejemplo, si tienes dos discos de 1 TB, solo podrás usar 1 TB, porque el segundo será una copia exacta. La ventaja es que, si un disco falla, la información sigue estando disponible en el otro, lo que lo hace ideal para servidores pequeños o para quienes priorizan la seguridad frente a la capacidad.

- **RAID 5 (paridad distribuida): **funciona con un mínimo de tres discos y combina reparto de datos y bloques de paridad distribuidos.  La paridad es un bloque de datos especial que no guarda directamente la información original, sino un cálculo matemático sobre ella (normalmente un XOR, es decir, una suma binaria). Ese bloque sirve como “seguro”: si uno de los discos se rompe, el RAID puede reconstruir los datos perdidos usando la paridad y lo que queda en los demás discos. Esto significa que si tienes tres discos de 1 TB, obtendrás 2 TB utilizables: uno se reserva para la información de paridad. Si se rompe un disco, los datos se pueden reconstruir a partir de los demás. Es una solución muy usada en NAS domésticos porque ofrece un buen equilibrio entre capacidad, velocidad y tolerancia a fallos, aunque la reconstrucción tras un fallo puede ser lenta.

- **RAID 6 (doble paridad):**  pero guarda dos bloques de paridad en lugar de uno, lo que permite que se puedan estropear dos discos sin perder datos. Necesita un mínimo de cuatro discos y la capacidad útil es el total menos dos. Así, con cuatro discos de 1 TB, tendrías 2 TB utilizables

## 2.8 Instalacion desantendida

Una instalación desatendida (o *unattended installation*) es aquella en la que el sistema operativo se instala sin intervención manual del usuario. Normalmente, el instalador hace preguntas: idioma, zona horaria, particiones, usuario, contraseña…

En una instalación desatendida, todas esas respuestas se definen antes en un archivo de configuración o script.

```yaml
#cloud-config
autoinstall:
  version: 1

  locale: es_ES.UTF-8
  keyboard:
    layout: es
    variant: ''
  timezone: Europe/Madrid

  identity:
    hostname: ubuntu-desktop-clase
    username: alumno
    password: "$6$abcd1234$Fv9N..." #Hay que hashear la contraseña
  ssh:
    install-server: false   # en Desktop no suele hacer falta

  storage:
    layout:
      name: direct   # usar todo el disco de manera automática

  updates: security

  packages:
    - vim
    - curl
    - htop
```



En una instlación como la de la práctica 4 tendriamos algo así

```yml
#cloud-config
autoinstall:
  version: 1

  locale: es_ES.UTF-8
  keyboard:
    layout: es
    variant: ''
  timezone: Europe/Madrid

  identity:
    hostname: ubuntu-desktop-clase
    username: alumno
    password: "$6$abcd1234$Fv9N..." 
  ssh:
    install-server: false

  storage:
    config:
      - { type: disk, id: disk0, match: { size: largest }, ptable: gpt }

      # Partición EFI (ESP) obligatoria en UEFI
      - { type: partition, id: efi, device: disk0, size: 512M, flag: boot }
      - { type: format, volume: efi, fstype: fat32 }
      - { type: mount, device: efi, path: /boot/efi }

      # Partición /boot
      - { type: partition, id: boot, device: disk0, size: 1G }
      - { type: format, volume: boot, fstype: ext4 }
      - { type: mount, device: boot, path: /boot }

      # Partición raíz /
      - { type: partition, id: root, device: disk0, size: 30G }
      - { type: format, volume: root, fstype: ext4 }
      - { type: mount, device: root, path: / }

      # Partición /home
      - { type: partition, id: home, device: disk0, size: 60G }
      - { type: format, volume: home, fstype: ext4 }
      - { type: mount, device: home, path: /home }

      # Partición swap
      - { type: partition, id: swap, device: disk0, size: 8G }
      - { type: format, volume: swap, fstype: swap }

  updates: security

  packages:
    - vim
    - curl
    - htop

```



