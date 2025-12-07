# Sistemas de Virtualización

## 1. ¿Qué es un Sistema de Virtualización?

En informática un sistema de virtualización se refiere a la creación a través de *Software* de un recurso tecnológico, este puede ser un ordenador físico completo, u otros recursos de *Hardware*.

Nos ofrece una *abstracción* de los recursos *hardware* de un ordenador a través de lo que llamaremos *Hypervisor*.

Los sistemas virtualizados han sido un gran avance en las tecnologías de la información, las ventajas de estos sistemas en cuanto a aprovechamiento de recursos han hecho que sean una opción cada vez más usada en los sistemas de información incluidos los servidores.

Ahora es posible hacer prácticas que antes eran inviables de llevar a cabo en una clase ordinaria por la insuficiencia de recursos hardware, o por los problemas derivados de realizarlas.

Por poner algún ejemplo, cuando se estudia la interconexión de ordenadores en red, antes era necesaria la coordinación de un mínimo de dos ordenadores con sus respectivos alumnos para hacer las pruebas, mientras que ahora se pueden hacer usando una sola máquina anfitriona y otra virtualizada. Otro claro ejemplo, es la instalación de varios sistemas operativos en la misma máquina sin peligro de que un mal paso modifique el arranque, o altere accidentalmente particiones de alguno de los sistemas que se pretendía hacer “convivir” en el mismo sistema. Las ventajas de este sistema frente a otros para realizar las pruebas este modelo no modifica en ningún momento el sistema sobre el que se instala. En este modelo existe un administrador de los recursos hardware llamado Hypervisor, o monitor de máquina virtual que será el encargado de hacer las peticiones a la CPU y administrar los privilegios en dichas peticiones. En nuestro caso *Virt-manager* será este Hypervisor

Una máquina virtual es un contenedor de software perfectamente aislado que puede ejecutar sus propios sistemas operativos y aplicaciones como si fuera un ordenador físico. La máquina virtual se comporta exactamente igual que un ordenador físico y contiene su propia CPU virtual, memoria, disco duro, tarjeta de interfaz de red... A todos los efectos una máquina virtual se considera como una máquina con su sistema operativo propio incluso los otros ordenadores de la red así lo *verán*. 

---

## 2. Guest y Host

A medida que vayamos leyendo documentación veremos muchas maneras de referirse a las máquinas virtualizadas y a los **hypervisores**. Una de las nomenclaturas que más puede aparecer son las palabras *Guest y Host* :

- **Guest** (invitado) es la máquina virtualizada, la que vive *"dentro"* de la máquina real.
- **Host** (anfitrión) es la máquina real, la que dispone de hardware real y virtualiza las otras.

---

## 3. Ventajas de la Virtualización

La principal ventaja de la virtualización es poder tener varios sistemas dentro de un solo hardware físico como si de varias máquinas con sus respectivos componentes hardware se tratase, siendo independientes los unos de los otros.

Otras de las ventajas son:

- Los usuarios verán los recursos que usan como si fueran dedicados.
- Una administración centralizada. Facilita hacer recursos más homogéneos llegando a estandarizarlos.
- Soporta trasladar sistemas y configuraciones de un sistema a otro, incluso en “caliente”.
- Mejora la tolerancia a fallos, si cae un sistema los otros siguen inalterados. Facilita las copias de seguridad.

---

## 4. Desventajas de la Virtualización

La principal desventaja de la virtualización, es que lógicamente el sistema principal que soportara las máquinas virtuales, debe disponer de una mayor cantidad y potencia de recursos a mayor número de sistemas queramos tener virtualizados en él. Los componentes principales que determinarán el número de máquinas virtuales que se podrán soportar sobre un hardware y el rendimiento de cada una de ellas son: la cantidad y velocidad de memoria RAM, la potencia del procesador y la velocidad de lectura, acceso y transferencia del disco duro, aunque hay más factores que determinarán el rendimiento final del sistema.

----

## 5. Tipos de Virtualización

Cuando hablamos de virtualización es importante distinguir dos cosas:

1. **Dónde se ejecuta el hipervisor** (clasificación en tipo 1 o tipo 2).
2. **Qué técnica de virtualización se utiliza** (virtualización completa, paravirtualización o emulación).

---

#### 5.1. Hipervisor tipo 1 (bare metal)

El hipervisor tipo 1, también llamado nativo o bare metal, es aquel que se ejecuta directamente sobre el hardware físico, sin que haya un sistema operativo anfitrión por debajo, es decir, in hipervisor tipo 1 (bare metal) no necesita que antes instales un sistema operativo como Windows o Linux. El propio hipervisor se instala directamente en el hardware, igual que si fuera un sistema operativo. De este modo, el propio hipervisor se encarga de gestionar la CPU, la memoria, el almacenamiento y los dispositivos de red, y reparte esos recursos entre las máquinas virtuales. El siguiente ejemplo es bastante descriptivo:

Cuando instalas VMware ESXi en un servidor:

- Descargas la ISO de ESXi.
- La grabas en un USB o CD.
- Arrancas el servidor y lo instalas en el disco duro.

Al terminar, no tendrás “Windows” ni “Linux” de fondo: lo único que hay es el hipervisor. Desde él gestionas las máquinas virtuales.

Al ejecutarse directamente sobre el hardware, este tipo de hipervisor ofrece un mayor rendimiento y estabilidad, por lo que es el modelo preferido en servidores y entornos de producción..

![Virtualizadores Bare Metal](https://gitlab.com/aberlanas/ASIR-ISO/-/raw/master/UD02_Virtualizacion/SistemasDeVirtualizacion/Hipervisor_PrimerNivel.svg.png)

Ejemplos de esta tecnologia:

- VMware ESXi (free y de pago)
- Xen (libre)
- Microsoft Hyper-V Server (gratis)
- Proxmox VE (gratis/de pago, basado en KVM)

Un pequeño apunte sobre lo anterior es que hay veces que el hipervisor puede ser considerado de tipo 1 aunque corra sobre un sistema operativo, pero son casos muy especiales. Por ejemplo:

Proxmox VE se basa en Debian Linux. En este caso sí hay un sistema operativo, pero está tan integrado con el hipervisor (KVM + LXC) que, a efectos prácticos, se considera tipo 1, porque el control de hardware y la gestión de VMs la hace directamente el kernel de Linux.

---

#### 5.2. Hipervisor tipo 2 (HOSTED)

El hipervisor tipo 2, también llamado **hosted**, funciona como una aplicación que se instala dentro de un sistema operativo anfitrión (Windows, Linux o macOS). En este modelo, es el sistema operativo host el que controla directamente el hardware, mientras que el hipervisor se limita a pedirle recursos para repartirlos entre las máquinas virtuales.

Este enfoque introduce una capa adicional, lo que implica algo menos de rendimiento que en el caso del tipo 1. Sin embargo, ofrece la gran ventaja de la simplicidad: basta con instalarlo como un programa más y en pocos minutos se pueden crear y ejecutar máquinas virtuales. Por eso se usa sobre todo en entornos de escritorio, pruebas de software, formación o desarrollo.



![Virtualizadores Completos](https://gitlab.com/aberlanas/ASIR-ISO/-/raw/master/UD02_Virtualizacion/SistemasDeVirtualizacion/Hipervisor_SegundoNivel.svg.png)



Ejemplos :

- VirtualBox (gratis)
- VMware Server (gratis)
- QEMU (libre, sin KVM, qunque veremos que también funciona como emulador)

---

#### 5.3 Técnicas de virtualización

Independientemente de que un hipervisor sea tipo 1 o tipo 2, puede utilizar diferentes **técnicas** para hacer funcionar a los sistemas invitados:

- **Virtualización completa**: se refiere principalmente a la ejecución del procesador invitado. 

  Puede implementarse tanto en un hipervisor tipo 1 como en uno de tipo 2. La diferencia entre tipo 1 y 2 es la capa sobre la que corre el hipervisor.

  En este modelo, el sistema invitado funciona como si estuviera en un hardware físico real y no necesita modificaciones especiales. 

  Si la CPU no soporta estas instrucciones especiales: El hipervisor no puede ejecutar directamente las instrucciones del guest en la CPU física. Para que la VM funcione, se recurre a técnicas de emulación o “traducción binaria”, donde el hipervisor intercepta y traduce las instrucciones que el guest intenta ejecutar. Esto implica una sobrecarga enorme y un rendimiento mucho más bajo

- **Paravirtualización**: se aplica sobre todo a los dispositivos de entrada/salida (E/S), como discos, tarjetas de red o memoria. Aunque la CPU pueda ejecutarse directamente gracias a la virtualización completa, los periféricos virtuales aún deben presentarse al sistema invitado. Esto puede hacerse de dos formas:

  En lugar de emular hardware real (como en el caso de la virtualización completa), el hipervisor ofrece dispositivos virtuales diseñados específicamente para entornos virtuales. Aquí entra en juego virtio, un estándar que define cómo se comunican estos dispositivos con el hipervisor de manera más eficiente. No son copias de hardware físico, sino interfaces simplificadas y optimizadas que reducen la sobrecarga y aumentan el rendimiento.
  
  Ejemplos de dispositivos virtio son:
  
  - virtio-blk: discos paravirtualizados más rápidos que los IDE o SATA emulados.
  - virtio-scsi: discos con interfaz SCSI optimizada.
  - virtio-net: tarjetas de red más rápidas y ligeras que las emuladas.
  
  La ventaja de virtio es clara: mayor rendimiento en disco, red y memoria, junto con un menor uso de CPU en el host frente a la emulación tradicional.

En cuanto a los controladores, los sistemas Linux ya incluyen drivers virtio en su kernel, por lo que funcionan de inmediato. En cambio, Windows no los incorpora por defecto, y es necesario instalarlos manualmente desde el proyecto oficial: VirtIO drivers (Fedora Project).

---

## 6.Emulación

La emulación es cuando un software simula por completo un hardware diferente al real.

- El host (tu máquina física) tiene una arquitectura de CPU concreta, por ejemplo x86_64 (Intel/AMD).
- Con emulación puedes ejecutar un sistema operativo invitado de otra arquitectura, por ejemplo ARMv7, MIPS o PowerPC.
- El emulador traduce cada instrucción que espera esa CPU “falsa” a instrucciones que entienda la CPU real.

Esto permite una compatibilidad total (puedes correr casi cualquier sistema en casi cualquier host), pero el rendimiento es bajo porque cada instrucción debe traducirse.

---

## 7. QEMU + KVM (Explicación + Instalación en Ubuntu)

QEMU/KVM es la combinación de dos piezas de software que, al trabajar juntas, convierten a Linux en un potente entorno de virtualización. Sin embargo, hay que distinguir cuál es su funcionamiento cuando van juntos y cuando van separados.

Por un lado está QEMU, que nació como un emulador capaz de reproducir distintas arquitecturas de CPU y de crear máquinas virtuales completas con sus discos, redes y dispositivos. Por sí solo, QEMU puede funcionar tanto como *emulador* (ejecutando arquitecturas distintas, aunque con menor rendimiento) como <u>*hipervisor tipo 2*</u> (cuando ejecuta la misma arquitectura que el host, pero sin aceleración por hardware).

La segunda pieza es <u>KVM (Kernel-based Virtual Machine),</u> un módulo del kernel de Linux que aprovecha las extensiones de virtualización de las CPUs modernas (Intel VT-x, AMD-V, ARM-VT) y transforma al propio kernel en un <u>hipervisor tipo 1</u>. Esto significa que, gracias a KVM, Linux ya no solo actúa como un sistema operativo, sino también como el gestor que reparte el hardware físico entre las máquinas virtuales.

Cuando se combinan, QEMU y KVM se reparten el trabajo: KVM se encarga de la ejecución directa de la CPU invitada sobre el hardware, logrando un rendimiento prácticamente nativo, mientras que QEMU proporciona la infraestructura alrededor de esa CPU, es decir, los dispositivos virtuales (discos, red, gráficos, memoria, snapshots, etc.). Así, el resultado práctico es un sistema que ofrece la potencia de un hipervisor tipo 1, con la flexibilidad de un hipervisor tipo 2, porque puedes gestionarlo desde el espacio de usuario como si fuera una aplicación más.

Aquí es donde entran en juego las técnicas de virtualización:

- Para la CPU, QEMU + KVM utiliza virtualización completa. Gracias a las extensiones de hardware, las instrucciones del sistema invitado se ejecutan directamente en el procesador físico, sin necesidad de modificaciones y con un rendimiento casi igual al nativo.
- Para los dispositivos de entrada/salida, QEMU ofrece dos posibilidades: emulación de hardware real conocido (compatible, pero lenta) o paravirtualización mediante virtio, un estándar que define dispositivos optimizados para entornos virtuales. Con virtio, discos, tarjetas de red o controladores de memoria funcionan de forma mucho más rápida y eficiente, reduciendo la carga de CPU en el host.

En cuanto a dónde puede ejecutarse, QEMU/KVM corre en hosts que utilicen Linux como sistema operativo y que dispongan de CPU con soporte de virtualización por hardware. Es habitual usarlo en servidores con distribuciones como Ubuntu, Debian, CentOS, Fedora o Proxmox (que, de hecho, se apoya en QEMU/KVM). También funciona en ordenadores de escritorio con Linux, lo que lo convierte en una solución tanto para entornos de producción como para desarrollo y pruebas.

Su funcionalidad principal es crear y ejecutar máquinas virtuales de alto rendimiento. Con QEMU/KVM puedes levantar sistemas invitados Windows, Linux, BSD o incluso macOS (con algunas limitaciones), asignándoles recursos de tu host como CPU, memoria, disco y red. Además, gracias a virtio y otros controladores paravirtualizados, esas máquinas alcanzan velocidades de disco y red muy cercanas al hardware real.La instalación en Ubuntu es bastante directa. Primero se actualizan los paquetes:

```bash
sudo apt update && sudo apt upgrade
```

Después se instalan QEMU, KVM y las utilidades necesarias para gestionar máquinas virtuales:

```bash
sudo apt install qemu-kvm libvirt-daemon-system libvirt-clients bridge-utils virt-manager
```

Las explicamos a continuación:

----

**`qemu-kvm`**

Es el paquete que instala QEMU con soporte para KVM. Aquí tienes la base del sistema de virtualización: QEMU para emular y gestionar las máquinas virtuales, y la integración con KVM para que la CPU invitada use directamente el hardware del host con rendimiento casi nativo.

---

**`libvirt-daemon-system`**

Libvirt es una capa de gestión que se comunica con KVM y QEMU. Permite administrar máquinas virtuales de forma uniforme, tanto desde la línea de comandos como desde interfaces gráficas. Este paquete instala el **demonio de libvirt**, que se ejecuta en segundo plano y actúa como “intermediario” entre tus comandos y el hipervisor.

---

**`libvirt-clients`**

Son las **herramientas de cliente**  mencionadas de libvirt, como `virsh`. Con ellas puedes crear, arrancar, apagar, pausar y configurar máquinas virtuales desde la terminal. Ejemplo:

```bash
virsh list --all
```

---

**`bridge-utils`**

Herramientas para crear y administrar **puentes de red (bridges)** en Linux. Esto sirve para que tus máquinas virtuales puedan conectarse a la red como si fueran equipos físicos más, con su propia dirección IP. Sin esto, solo tendrías NAT básico, y las VMs no serían accesibles directamente desde la red local.

---

**`virt-manager`**

Es la interfaz gráfica oficial para gestionar máquinas virtuales con QEMU/KVM y libvirt. Muy útil si no quieres hacerlo todo desde la línea de comandos. Te permite crear nuevas VMs, asignar CPU, RAM, discos y redes, además de ver la consola gráfica de los invitados.

---

Finalmente, se añade al usuario al grupo `libvirt` para gestionar las VMs sin permisos de root:

``sudo usermod -aG libvirt $USER``

E inicia el demonio y comprueba el estado

```sudo systemctl enable --now libvirtd```
```sudo systemctl status libvirtd```

Después, reinicia el sistema

---

## 8. Virtualización Anidada

QEMU es solo un software de emulación/virtualización en espacio de usuario. Siempre puedes instalarlo en cualquier Linux, incluso dentro de una VM, y ejecutar máquinas virtuales en modo emulación (por ejemplo, un ARM sobre un host x86).

Para poder usar KVM dentro de una máquina virtual necesitas lo que se conoce como virtualización anidada. KVM, a diferencia de QEMU puro, requiere acceso directo a las extensiones de virtualización de la CPU (Intel VT-x o AMD-V). Si la máquina virtual que actúa como “host dentro del host” no recibe esas extensiones, entonces dentro de ella solo podrás usar QEMU en modo emulación, lo que es mucho más lento.

Que esto funcione o no depende del hipervisor en el que esté corriendo tu primera máquina virtual. Por ejemplo, en VirtualBox puedes instalar QEMU sin problemas, pero no tendrás KVM porque VirtualBox no expone VT-x al invitado, de modo que estarás limitado a la emulación. En  QEMU/KVM  o en ESXi sí es posible habilitar la virtualización anidada desde la configuración de la máquina virtual, lo que permite que KVM funcione dentro de ella, allí puedes activar la opción `nested=1` en el módulo KVM del kernel para que los invitados puedan aprovechar estas extensiones de hardware.

Para comprobar si KVM está disponible dentro de tu máquina virtual basta con ejecutar:

```
egrep -c '(vmx|svm)' /proc/cpuinfo
```

Si el resultado es 0, significa que tu hipervisor no está exponiendo las extensiones de virtualización y solo podrás usar QEMU en modo emulación. En cambio, si el resultado es mayor que 0, tu invitado sí tiene acceso a VT-x o AMD-V y por tanto podrás usar KVM, siempre que la virtualización anidada esté activada en el host principal.

**<u>En caso de no poder, sigue los siguientes pasos:</u>**

Edita la configuración de KVM para permitir “nested virtualization”:

```
sudo nano /etc/modprobe.d/kvm.conf
```

Añade una de estas líneas según tu CPU:

- Para Intel:

  ```
  options kvm-intel nested=1
  ```

- Para AMD:

  ```
  options kvm-amd nested=1
  ```

Recarga el módulo KVM:

```
sudo modprobe -r kvm_intel
sudo modprobe kvm_intel
```

Comprueba si está activo:

```
cat /sys/module/kvm_intel/parameters/nested
```

debe mostrar `Y` (yes).

---

## 9. Emulación con QEMU

QEMU permite no solo virtualizar sistemas de la misma arquitectura que el host, sino también emular arquitecturas diferentes. Esto es útil, por ejemplo, para ejecutar un sistema operativo compilado para ARM en un ordenador con CPU x86.

En este caso, se puede crear una máquina virtual que emule un sistema ARM y arrancar en ella una distribución Debian ARM. Para lograrlo, primero hay que tener algunas arquitecturas previas instaladas para QEMU

---

## 10. Gestión de Máquinas virtuales a través de comandos (NO ESTUDIAR COMANDOS NI EJECUTAR)

En esta sección veremos conceptos muy sencillos de nuestra máquina virtual. Aunque aquí deje comandos, lo más importante es que sepáis hacerlo desde la GUI (desde la propia aplicación). Repito, no teneis que saber los comandos, teneis que saber hacer esto desde la interfaz gráfica.

---

####  10.1. Ver máquinas virtuales

Listar las máquinas que están **encendidas**:

```
virsh list
```

Listar **todas** (incluidas apagadas):

```bash
virsh list --all
```

<u>Evidentemente, ¿cómo es testo desde la interfaz gráfica?</u> Dejo captura:

![image-20251004123214024](../../img/image-20251004123214024.png)

---

#### 10.2. Encender / apagar / reiniciar

**Encender** una VM:

```bash
virsh start NOMBRE_VM

```

Apagar de forma normal (como pulsar el botón de apagado):

```bash
virsh shutdown NOMBRE_VM
```

**Forzar apagado** (como quitar el cable de corriente):

```bash
virsh destroy NOMBRE_VM
```

**Reiniciar**:

```bash
virsh reboot NOMBRE_VM
```

<u>Evidentemente, ¿cómo es esto desde la interfaz gráfica?</u> 

Es tan sencillo como encenderlas y apagarlas desde la aplicación. No adjunto captura porque es evidente.

---

#### 10.3 Redes y almacenamiento

Cuando gestionas máquinas virtuales con **QEMU/KVM** y **libvirt**, no solo controlas el encendido o apagado de las VMs, sino también dos recursos fundamentales: **la red** y **el almacenamiento**. Estos son los que permiten que las máquinas virtuales puedan comunicarse y guardar sus datos de forma persistente.

Por defecto, al instalarlo, aparece una red llamada **`default`**, que funciona en modo **NAT**. Esto significa que tus máquinas virtuales tienen acceso a Internet usando la IP del host como “puerta de enlace”, pero desde la red local no se puede acceder directamente a ellas. Además del modo NAT, libvirt permite crear otros tipos de redes:

- **Puente (bridge)**: la VM se conecta directamente a la misma red que el host, con su propia IP visible en el router.

En la práctica, puedes **listar las redes** que existen con:

```bash
virsh net-list --all
```

Arrancar una red:

```bash
virsh net-start default
```

Ojo, se pueden instalar más tipos de redes, como las isolated. Investigad al respecto.

En cuanto al almacenamiento, libvirt organiza los discos virtuales dentro de lo que llama **pools de almacenamiento**. Un *pool* es como un “contenedor” donde se guardan los discos de las máquinas virtuales (imágenes `.qcow2`, `.raw`, etc.). 

Es fundamental aclarar **qué tipos de discos virtuales existen** en KVM/QEMU/libvirt, porque de eso depende el rendimiento, la flexibilidad y hasta el tamaño que ocuparán en tu sistema. Cuando hablamos de discos virtuales nos referimos a las unidades de memoria paravirtualizadas donde se instalará el sistema operativo (usaremos ISO's para instalar el sistema operativo en los discos paravirtualizados).

- El más básico es el **formato raw** (no lo usaremos, pero saber que existe es importante), que no es más que un archivo plano que ocupa en el disco exactamente el tamaño que le hayas asignado. Por ejemplo, si decides crear un disco de 20 GB en formato raw, ese archivo pesará 20 GB desde el primer momento, aunque dentro de la máquina virtual solo hayas guardado unos pocos archivos. La ventaja de este formato es que ofrece el mejor rendimiento posible, ya que no hay ninguna capa de gestión adicional, y además es muy compatible con otros sistemas de virtualización. Su desventaja es que consume espacio de forma innecesaria si no se llena todo el disco.

- Por otro lado, el formato más popular en entornos de QEMU/KVM es **qcow2** (es el que usaremos). A diferencia de raw, este formato es dinámico: el archivo crece a medida que la máquina virtual va escribiendo datos, de manera que si le asignas 20 GB pero solo usas 2 GB, en el host ese archivo ocupará aproximadamente esos 2 GB. Además, qcow2 incorpora funciones avanzadas muy útiles como la posibilidad de hacer **snapshots** (puntos de restauración de la máquina), compresión para ahorrar espacio e incluso cifrado. El precio a pagar es un pequeño descenso en el rendimiento respecto a raw, ya que necesita gestionar esta capa extra de funcionalidad. Crear un disco qcow2 es lo más común en virt-manager. lo podemos crear de forma dinámica cuando instalamos, o podemos crear un disco virtual vacío donde instalaremos un sistema operativo.

  En el siguiente vídeo se detalla conceptos básicos que necesitais saber de los discos qcow2 en vuestras maquinas virtuales, como redimensionarlos:

  https://drive.google.com/file/d/1CcsgfeX-QKxMTJq-pRBTaAmoTakn_z_t/view?usp=sharing
  
- Existen también otros formatos pensados principalmente para la compatibilidad entre plataformas. Uno de ellos es el **vmdk** (no lo veremos), que es el formato nativo de VMware. Usarlo en KVM puede ser útil si planeas mover máquinas virtuales entre entornos de VMware y QEMU/KVM. Otro formato similar es **vdi**, que es el que utiliza VirtualBox. De la misma forma, resulta práctico cuando quieres migrar máquinas desde VirtualBox hacia KVM o al revés.

- La siguiente captura muestra cómo he creado un disco virtual vacío, que servirá posteriormente para instalar un sistema operativo (es decir, para crear una máquina virtual). Para ello, accedí al menú de creación y pulsé el botón “+” situado junto a la sección Volúmenes. Allí generé el disco virtual vacío, que quedó almacenado en la carpeta /home/manolo/Escritorio/Mis maqunas virtuales”. Esta carpeta la he creado yo y está montada como un directorio en el que puedo guardar máquinas virtuales, tanto vacías como con sistemas ya instalados. Las carpetas en las que se almacenan las máquinas virtuales (archivos .qcow2 o las imágenes ISO ) se denominan “pools de almacenamiento”. En el apartado 10.4 explicamos este concepto con más detalle. Por ahora, lo importante es quedarse con la idea de que hay que crear una máquina virtual vacía o un disco virtual vacío (los términos pueden usarse indistintamente). En este punto, he salido del asistente de creación de la máquina virtual tras generar el disco. la siguiente captura muestra cómo cree el disco virtual vacío.

  ![image-20251004123214024](../../img/image-20251004130105191.png)

  

- Una vez he creado el disco virtual vacío, vamos a proceder a instalar una sistema operativo en él (es decir, una máquina virtual). para ello, elegimos iniciar una máquina virtual a través de una iso.

  ![image-20251004123214024](../../img/image-20251004125757231.png)

  

- Cuando ya hemos elegido la ISO y hemos asignado memoria y CPU lógicas, podemos escoger entre dos opciones: Un disco vacío existente:

  ![image-20251004123214024](../../img/image-20251004125901711.png)

  

- O elegir crear uno nuevo de forma dinámica (por defecto se guardará en /var/lib/libvirt/images)

  ![image-20251004123214024](../../img/image-20251004125914920.png)

  

---

#### 10.4 Pools de almacenamiento

Un pool de almacenamiento en libvirt/Virt-Manager es básicamente un contenedor lógico que define dónde se guardan los discos virtuales (donde hemos instalado el sistema operativo en el punto anterior) y las ISOs que usarán tus máquinas virtuales.

- **dir: Directorio del Sistema de Archivos****

  El más común y sencillo.Es simplemente una carpeta en tu host (ej. `/var/lib/libvirt/images/` o `/home/usuario/isos`).Dentro de ella guardas archivos `.qcow2`, `.raw` o `.iso`.

  Para crearlo, vasta ir adarle el boton de crear una máquina virtual a través de una ISO o un medio de almacenamiento existente (spoiler, no crearemos una máquina virtual) y cuando nos pregunte que disco virtual quiero elegir para iniciarla, le damos al botón "+" de abajo a la izquierda:

  ![image-20251004123214024](../../img/image-20251004130553392.png)

  

  Una vez le damos, nos da la opción de creaar un pool (un lugar donde guardar nuestros discos virtuales)

  ![image-20251004123214024](../../img/image-20251004130836907.png)

  En este caso, al pool le he puesto el nombre "Mis_volumenes", y le he asignado la carpeta /home/manolo/Escritorio/Mis  maquinas virtuales. Ahí es donde guardaré tanto mis discos qcow2 (vacíos como con Sistemas operativos) cómo mis ISOS. De hecho, aquí guardé el disco virtual del punto anterior.

  El siguiente video muestra como crear un pool de almacenamientos en virt manager:

  https://drive.google.com/file/d/1B0IeVjZ5RCvOnhJReWRR3JhB2r_bWwAB/view?usp=sharing

- **disk: Dispositivo de Disco Físico****

  Usa un disco entero (`/dev/sdb`, `/dev/sdc`, etc.) como pool. El USB debe estar **sin particionar o con todo el disco libre**. Cuando lo declares como pool tipo *disk* (`/dev/sdb`, `/dev/sdc`, etc.), libvirt lo formateará y lo dedicará exclusivamente a guardar volúmenes. No podrás usar el USB desde el explorador de archivos del host porque no tendrá un sistema de ficheros tradicional (será gestionado directamente por libvirt)

- **fs: Dispositivo de Bloque Preformateado**

  Sirve para guardar dentro de una partición del USB. El usb deberñá tener esquema de particiones GPT y una particion con formato de datos ext4. En el video siguiente explico esto. Más concretamente, en el minuto:
  
  https://drive.google.com/file/d/1TanUWvXigGdC3ImH65bwfNMAaGITSrAl/view?usp=sharing

Cuando vas a crear una máquina virtual, puedes crear tu propio pool de almacenamiento según te convenga. Hay más opciones, pero con estas tres, por ahora es más que suficiente. La primera de las opciones es la mas comoda y te permitirá compartir tus MV's facilmentes una vez comprimidas. Los USB también es buena idea. El problema será el almacenamiento del que disponfra el sub. En el siguiente video os dejo como podeis instalar vuestras maquinas virtuales en un usb para poder llevaroslas a casa.

---


#### 10.5. Gestión de snapshots

Un snapshot es como una “foto” de tu máquina virtual en un momento dado. Incluye: El estado del disco (qué datos tenía en ese instante). Opcionalmente, el estado de la memoria RAM y la CPU (es decir, la VM en pausa, lista para reanudar).

![image-20251004123214024](../../img/image-20251004142127661.png)



Para crear una instantanea le dariamos a +

![image-20251004123214024](../../img/image-20251004142213832.png)



y le damos una descripcion

![image-20251004123214024](../../img/image-20251004142337433.png)

Para recuperar el estado de la maquina antes del snapshot, basta con seleccionar el snapshot:

![image-20251004123214024](../../img/image-20251004142549043.png)



Y darle a start abajo a la derecha

En el siguiente video os dejo la gestión de snapshots en vuestras máquinas virtuales:
https://drive.google.com/file/d/1jHOEC1C7SAE8vjoMVcKHFLnPulCEODGe/view?usp=drive_link

---

#### 10.6. Clonación de Maquinas Virtuales y redimensionar una máquina virtual

Desde la interfaz gráfica, click derecho + clonar. Importante crear un disco virtual nuevo al clonar (se hace por defecto) y hacerlo con la maquina apagada.

![image-20251004123214024](../../img/image-20251004142657697.png)

En esencia es clonar el disco (copiar y pegar el disco donde esta instalado la maquina que queremos clonar)

![image-20251004123214024](../../img/image-20251004142801149.png)



En este caso le doy un nuevo nombre para diferenciarlo del anterior. De esta forma, parto desde el mismo punto de mi maquina anterior sin necesidad de hacer un snapshot, y puedo tener dos maquina virtuales con un mismo punto de partida pero que en esencia, son distintas.

Para redimensionar una máquina virtual, se hacer lo siguiente: 

https://drive.google.com/file/d/1CcsgfeX-QKxMTJq-pRBTaAmoTakn_z_t/view?usp=sharing

---

#### 10.7 Compartir carpetas con Windows y Linux 

Por brevedad en el documento, dejo el siguiente enlace. Son 5 minutos de video y debéis mirar desde el minuto 6 al 13.

[Compartir carpetas virt manger a MV Windos o Linux](https://www.youtube.com/watch?v=CdWjyJCBYA0)

Se pedirá que lo hagáis en algunas prácticas.