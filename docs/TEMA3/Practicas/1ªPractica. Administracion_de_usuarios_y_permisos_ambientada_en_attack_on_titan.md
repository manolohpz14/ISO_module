# Práctica: Administración de usuarios y permisos. Ambientada en *Attack on Titan*

**Duración estimada:** 120–180 minutos (actividad practica en clase)

---

## 1- Descripción de la primera práctica del tema

La práctica se realizará en ubuntu 24.04 (como siempre). Esta es una de las prácticas más importantes del curso ya que tocarán conceptos que os acompañaran por bastente tiempo. Como siempre, dejo la práctica resuelta por mi en el siguiente video, pero esta vez os recomiendo intentearlo por vosotros mismos previamente porque si no, os va a costar mucho cuando hagamos el examen.

La práctica os introduce en la creación, gestión y control de usuarios y permisos en un sistema Linux (debian). El escenario está ambientado en *Attack on Titan* (una de mis series favoritas, que os recomiendo ver como mínimo, tres veces): los participantes (vosotros) gestionaréis cuentas del "Muro", y crearéis grupos (Policía Militar, Guarnición y Exploradores), y asignaréis permisos a ficheros y directorios críticos.

Se trabajará tanto a nivel práctico como teórico con los archivos y utilidades esenciales: 

`/etc/passwd`

`/etc/group`

`/etc/shadow`

`useradd`

`usermod`

`passwd`

`groupadd`

`chown`

`chmod`

`mkdir`

`chage`

`getent`

`su`

`userdel`

---

## 2- Escenario (por darle un poco de gracia)

Es la época de la amenaza de titánica 2026, no ha piedad para la raza humana y menos para los alumnos de ISO. Vuestro sistema actuará como ordenador <u>único y central</u> del Muro y deberá hacer las siguientes cuestiones: 

- A modo resumen, lo que iréis haciendo será crear cuentas para personajes (Eren, Mikasa, Armin, Levi, Hange), asignar directivas de expiración de contraseñas. agrupar a los usuarios en grupos, y dar permisos a directorios que crearéis.


---

## 3- Archivos y conceptos esenciales (breve guía)

En los sistemas GNU/Linux, los usuarios y los grupos son la base de la gestión de permisos y seguridad.  Cada usuario del sistema (ya sea una persona o un servicio) tiene un identificador propio (UID) y pertenece al menos a un grupo principal (GID).  Los grupos permiten organizar usuarios que comparten recursos o permisos comunes.  Para la elaboración de esta práctica es necesario comprender los siguientes tipos de directorios que nos encontramos:

- `/etc/passwd` : Contiene la lista de usuarios del sistema y su información básica:

```makefile
  nombre_usuario:x:UID:GID:comentario:directorio_home:intérprete_de_comandos
```

  - Nombre de usuario  
  
  - x (contraseña, se guarda en `etc shadow`)
  
  - UID (User ID)  
  
  - GID (Group ID principal)  
  
  - Directorio personal (`/home/usuario`)  
  
  - Shell por defecto (`/bin/bash`, `/usr/sbin/nologin`, etc.) . Las maas importantes, están aquí:
  
    | Shell   | Ruta                | Disponibilidad                                               | ¿Instalado por defecto? | Comentario                                                   |
    | ------- | ------------------- | ------------------------------------------------------------ | ----------------------- | ------------------------------------------------------------ |
    | Bash    | `/bin/bash`         | Prácticamente todas las distribuciones (Debian, Ubuntu, Fedora, CentOS, Arch, etc.) | Sí                      | Es el shell más común en Linux. Potente, interactivo y compatible con la mayoría de scripts del sistema. |
    | Sh      | `/bin/sh`           | Siempre presente                                             | Sí                      | Shell estándar POSIX. En muchos sistemas es un enlace simbólico a `bash` o `dash` (en Debian/Ubuntu). Debe existir por compatibilidad. |
    | Fish    | `/usr/bin/fish`     | Disponible en la mayoría de repositorios                     | No                      | Debe instalarse manualmente (`sudo apt install fish`). Shell moderno, con autocompletado avanzado y sintaxis más legible. |
    | Nologin | `/usr/sbin/nologin` | Siempre disponible                                           | Sí                      | Impide el acceso interactivo al sistema mostrando un mensaje de /etc/nologin.txt. Se usa en cuentas de servicio o de sistema (por ejemplo, `www-data`) para Apache |
    | False   | `/bin/false`        | Siempre disponible                                           | Sí                      | Impide el acceso interactivo al sistema pero sin ningún tipo de mensaje. Se usa en cuentas técnicas o sin acceso al sistema. |
  
- `/etc/shadow` : Almacena las contraseñas encriptadas de los usuarios y las políticas de expiración de las mismas.  Sólo puede ser leído por el usuario `root` por motivos de seguridad. Lo veremos en detalle en el punto 2.

- `/etc/group` : Define los grupos del sistema y sus miembros. Cada línea tiene el formato:

```makefile
  nombre_grupo:x:GID:miembro1,miembro2,miembro3
```

  El **primer campo** es el nombre del grupo
   El **segundo campo**, normalmente una `x`, indica que la contraseña del grupo (si existe) se almacena de forma cifrada en el archivo **`/etc/gshadow`**.
   El **tercer campo** es el **GID** o identificador numérico del grupo, que el sistema usa internamente para asociar permisos y propiedades.
   El **cuarto campo** es una lista opcional de usuarios separados por comas que pertenecen a ese grupo como miembros secundarios.

- `change`:  El comando `chage` (change age) se usa para **administrar la caducidad de contraseñas** y las políticas de renovación de las mismas. Permite, por ejemplo, obligar a los usuarios a cambiar su contraseña cada cierto tiempo o establecer una fecha de expiración de la cuenta.

---

## 4-Comienzo de la Práctica

### 4.1. Primera parte. Crear grupos y usuarios (DEBÉIS HACER TODO LO QUE SE ESPECIFIQUE en los apartados a), b)...)

a) En este apartado, creamos primero cada uno de los grupos de los que consta la práctica

``` bash
sudo groupadd exploradores
sudo groupadd guarnicion
sudo groupadd policia_militar
cat /etc/group #mira todos los grupos

```

Una vez hecho el apartado a) la idea ahora es añadir es crear cada usuario y añadirlo a un grupo. No lo haremos como en la serie, lo haremos un poco distinto:

| Usuario                              | Grupo asignado  |
| ------------------------------------ | --------------- |
| **eren**                             | exploradores    |
| **armin**                            | exploradores    |
| **mikasa**                           | guarnicion      |
| **levi**                             | policia_militar |
| **hange**                            | exploradores    |
| **Elige a tu compañero de práctica** | exploradores    |
| **reiner**                           | exploradores    |
| **Eligete a ti**                     | policia_militar |

A continuación necesitamos crear primero los usuarios que aparecen en la tabla, para crear usuarios ejecutaremos el comando `useradd`. El comando `useradd` sirve para crear nuevos usuarios en el sistema. Sin embargo, utilizaremos opciones sobre dicho comando parámetros que modifican su comportamiento. Dos de los más importantes son `-m` y `-s`. También especificamos `G`y `g`.

- `-m` (make home directory)
   Indica que se debe crear automáticamente el directorio personal del usuario (normalmente en `/home/<usuario>`). se crea `/home/eren` y se copian dentro los archivos de plantilla de `/etc/skel/` (como `.bashrc`, `.profile`, etc.). Sin `-m`, el usuario se crearía sin un directorio personal, lo cual no suele ser deseable en un entorno de trabajo normal.
   
- `-s` (shell)
   Especifica qué intérprete de comandos o shell usará el usuario por defecto (de las vistas y explicadas antes)
   
- El parámetro `-G` se utiliza en `useradd` para asignar al usuario uno o varios grupos secundarios (también llamados *grupos suplementarios*). Un usuario en Linux siempre pertenece al menos a un grupo principal (Grupo predeterminado del usuario y los archivos que crea pertenecen a este grupo), pero puede pertenecer a más de uno. Al crear un usuario siempre se crea un 

- grupo con el mismo nombre que el usuario.

- El grupo principal del que hablabamos antes se define con la opción `-g` (minúscula) si no se pone nada, se añade al grupo (principal) que tiene el mismo nombre que el usuario creado.

---


b) En este apartado y en nuestro caso, ejecutamos los siguientes comandos cuyo objetivo es crear los usuarios, asignarles la terminal por defecto y añadirles a un grupo complementario a cada uno segúm la tabla anterior. Por último, no olvides por favor crearte un usuario y añadirte a un grupo.

``` bash
sudo useradd -m -s /bin/false -G exploradores eren
sudo useradd -m -s /bin/bash -G exploradores armin
sudo useradd -m -s /bin/bash -G guarnicion mikasa
sudo useradd -m -s /bin/bash -G policia_militar levi
sudo useradd -m -s /bin/bash -G exploradores hange
sudo useradd -m -s /bin/bash -G exploradores reiner
sudo useradd -m -s /usr/sbin/nologin -G tu_grupo_favorito tu_compañero
sudo useradd -m -s /bin/bash -G tu_grupo_favorito tu_nombre
```

Ojo, para ver todos los grupos de un usuario, es suficiento con

```bash
groups eren
```

En vuestro caso, os devolverá eren y exploradores Después de haber creado los usuarios, asignareis contraseñas, ¿qué pasará si no lo hacemos?

``` bash
sudo passwd eren
sudo passwd mikasa
sudo passwd armin
sudo passwd levi
sudo passwd hange
sudo passwd tu_compañero
sudo passwd reiner
sudo passwd tu_nombre
```

Una vez hecho el apartado b), lo que se pida ahora es que comprobéis que se han creado correctamente los usuarios y sus grupos, para ello usaréis el comando `getent`: El comando `getent` (de *get entries*, “obtener entradas”) permite consultar información de las bases de datos del sistema definidas en `/etc/nsswitch.conf`. Estas bases de datos incluyen:

- usuarios (`passwd`)
- grupos (`group`)
- contraseñas cifradas (`shadow`)
- hosts, redes, servicios, etc.

Esto hace que funcione tanto con usuarios locales (definidos en `/etc/passwd`) como con usuarios remotos, si el sistema usa autenticación por red (LDAP, que veremos en el tema 5) Por eso se considera una forma más moderna y segura de obtener información que leer directamente los archivos con `cat /etc/passwd`. 

Comando para ver el grupo principalm de un usuario;

```bash
id -g nombre_usuario
```

---

c) Comprueba, por tanto, la salida de los siguientes comandos y verifica que efectivamente devuelven lo mismo. Para ello, buscad información de que hace el comando grep, aunque lo veremos en prácticas posteriores. Lo que quiero que os quede claro es que aunque getent es una herramienta poderosa para buscar en el archivo /etc/shadow, /etc/passwd o /etc/group  se puede buscar directamente sin usar el comando getent.

``` bash
getent passwd eren
cat /etc/passwd | grep eren

getent group exploradores
cat /etc/group | grep exploradores

sudo getent shadow eren
sudo grep "^eren:" /etc/shadow
```

El último comando debe dar una salida parecida a la siguiente (no exactamente igual porque lo que os doy, no es más que un ejemplo):

```makefile
eren:$6$GdX9p7Ds$eEybZK2....:19443:0:99999:7:::
```

---

### 4.2.(Apartado Teórico) Segunda Parte. Revisar `/etc/shadow` y caducidad de contraseñas

Volviendo a la salida del último comando vamos a ver qué significa cada cosa que allí aparecía.

```makefile
eren:$6$GdX9p7Ds$eEybZK2....:19443:0:99999:7:::
```

Cada línea devuelta de `/etc/shadow` tiene **9 campos** separados por `:` (dos puntos) y significan lo siguiente:

| Campo | Ejemplo                  | Significado                                                  |
| ----- | ------------------------ | ------------------------------------------------------------ |
| 1     | `eren`                   | Nombre del usuario                                           |
| 2     | `$6$GdX9p7Ds$eEybZK2...` | Contraseña cifrada (*hash*) o símbolo especial (`!` o `*` si está bloqueada) |
| 3     | `19443`                  | Fecha del último cambio de contraseña (días desde 1/1/1970)  |
| 4     | `0`                      | Este campo define cuántos días deben pasar desde el último cambio antes de que el usuario pueda cambiar su contraseña otra vez. |
| 5     | `99999`                  | Este campo indica cuánto tiempo puede usarse una contraseña antes de que caduque. |
| 6     | `7`                      | Días de aviso antes de la caducidad                          |
| 7     | (vacío)                  | Días tras la caducidad antes de la contraseña antes de desactivar la cuenta |
| 8     | (vacío)                  | especifica el número de días desde el 1 de enero de 1970 (epoch) en que la cuenta será desactivada completamente (no se podrá iniciar sesión, aunque la contraseña sea válida). |
| 9     | (vacío)                  | este campo está reservado para uso futuro o para extensiones específicas del sistema.<br/> En la práctica, la mayoría de las distribuciones lo dejan vacío. |

El **<u>comando chage</u>** sirve para cambiar estas configuraciones de la contraseña, las opciones mas usuales que tiene el comando son las siguientes:

- `-l` : listar la información de caducidad de la cuenta.

- `-d <FECHA|0>` : fijar la fecha del último cambio de contraseña (poner `0` fuerza a cambiar la contraseña en el próximo inicio). Ojo, ejemplo:

```bash
sudo chage -l armin
  
sudo chage -d 2025-09-15 armin #Esto registra que Armin cambió su contraseña el 15 de septiembre de 2025.
  #El sistema calculará los plazos de caducidad (-M, -W, etc.) a partir de esa fecha.
  
sudo chage -l armin
  
sudo chage -d 0 armin #Si pones 0, estás diciendo al sistema:Haz como si el usuario nunca hubiera cambiado la contraseña.forzamos que cambie la contraseña al siguiente inicio
  
sudo chage -l armin
```

- `-m <DÍAS>` : mínimo de días entre cambios de contraseña (no puede cambiar antes).

```bash
sudo chage -m 1 armin #m 1: evita que el usuario cambie la contraseña varias veces seguidas (por ejemplo, para saltarse la política de seguridad) ojo, root puede...
  
sudo chage -l armin
  
sudo chage -m 0 armin  #m 0: Sin restricción mínima. (valor por defecto en muchos sistemas)
  
sudo chage -l armin
  
  
```

- `-M <DÍAS>` : máximo de días que la contraseña es válida (caducidad).

```bash
sudo chage -M 90 armin #M 90: obliga al usuario a cambiar la contraseña cada 3 meses
  
sudo chage -l armin
  
sudo chage -M 99999 armin #Este es el valor por defecto en muchas distribuciones.Equivale a decir “la contraseña no tiene fecha de caducidad”.
  
sudo chage -l armin
```

- `-W <DÍAS>` : días de aviso antes de la caducidad.

```bash
sudo chage -W 10 armin #W 10: muestra un aviso 10 días antes de la expiración para que el usuario tenga tiempo de cambiarla.
  
sudo chage -l armin
  
sudo chage -W 0 armin #Sin aviso
  
sudo chage -l armin
```

- `-I <DÍAS>` : días de inactividad tras caducidad antes de desactivar la cuenta (inactividad).

```bash
sudo chage -I 15 armin #si la contraseña ha caducado y el usuario no la cambia en 15 días,el sistema bloqueará la cuenta por inactividad.
  
sudo chage -l armin
  
sudo chage -I -1 armin #Si le pasas el valor -1, estás indicando que no hay límite de inactividad, el usuario nunca será bloqueado automáticamente por este motivo.
  
sudo chage -l armin
```

- `-E <FECHA|0>` : fecha en la que la cuenta queda desactivada / expirada (formato `YYYY-MM-DD` o `0` para sin expiración).

```bash
sudo chage -E 2025-12-31 armin #fecha en la que la cuenta expirara
  
sudo chage -l armin
  
sudo chage -E 0 armin #cuenta ano expira
  
sudo chage -l armin
```

-Otros ejemplos combinados son:

```bash
sudo chage -m 1 -M 90 -W 10 armin #m 1: evita que el usuario cambie la contraseña varias veces seguidas (por ejemplo, para saltarse la política de seguridad).M 90: obliga al usuario a cambiar la contraseña cada 3 meses.W 10: muestra un aviso 10 días antes de la expiración para que el usuario tenga tiempo de cambiarla.

sudo chage -l armin #mostramos la información de caducidad

```

Lo que da la siguiente salida:

```makefile
Último cambio de contraseña                                 : Oct 19, 2025
La contraseña puede cambiarse                               : Oct 20, 2025
La contraseña debe cambiarse                                : Jan 17, 2026
El usuario recibe aviso antes de que caduque la contraseña   : 10 días
Cuenta expira                                               : nunca
Inactividad después de caducar la contraseña                 : 0 días
```

Otros ejemplos:

```bash
sudo chage -m 5 -M 60 -W 7 -I 10 usuario4 #La contraseña debe mantenerse al menos 5 días antes de poder cambiarse. Caduca a los 60 días. El usuario será avisado con 7 días de antelación. Si no cambia la contraseña tras caducar, la cuenta se bloqueará 10 días después.

sudo chage -m 0 -M 99999 -I -1 -E -1 usuario #Dejas sin restricciones: m 0 → sin restricción mínima de cambio. M 99999 → caduca dentro de mucho tiempo (≈273 años). I -1 → sin bloqueo por inactividad. E -1 o -E 0 → sin fecha de expiración
```

---

### 4.3. Aplicar el comando chage

Tras la teoría anterior, lo que os pido ahora es que ejecuteis un comando para que el usuario de vuestro compañero solo pueda cambiar la contraseña una vez por dia, y que solo pueda usarse una contraseña cada 3 meses (90 dias), además, el usuario deberá ser avisado con 10 días de antelación. El usuario deberá cambiar la contraseña en 15 días o su cuenta expirará

```bash
sudo chage -m 1 -M 90 -W 10 -I 15 usuario
```

Tras realizar lo anterior, resetea manualmente la última fecha de cambio de contraseña de tu compañero a ayer y ejecuta el comando `chage -l` sobre tu compañero.

```bash
sudo chage -d 2025-11-5 usuario
sudo chage -l usuario
```

---

### 4.4. (Apartado teórico) Moverse por directorios de Linux y creación de directorios.

Antes de realizar el siguiente punto, es conveniente que os sepáis mover vía terminal por los distintos directorios que posee vuestro sistema Ubuntu:

En los sistemas GNU/Linux, el terminal es una herramienta fundamental para navegar y trabajar con directorios y archivos.  
Los dos comandos más básicos para orientarte son `cd` (cambiar de directorio) y `pwd` (mostrar la ruta actual).

---

-El <u>comando `pwd`</u> muestra la ruta completa del directorio actual en el que te encuentras. Sintáxis básica:

```bash
$ pwd
salida-> /home/alumno
```

---

-El <u>comando `cd`</u> sirve para moverte entre carpetas (directorios) dentro del sistema de archivos. Sintáxis básica:

```bash
cd <ruta>
```

Donde <ruta> puede ser:

```makefile
absoluta → comienza desde la raíz /

relativa → depende del directorio en el que estás ahora
```

---

-<u>Una ruta absoluta</u> indica el camino completo desde la raíz `/`.

```bash
cd /home/manolo/Documentos
```

Después de ejecutarlo, estarás en la carpeta `documentos` del usuario `manolo`.

Puedes comprobarlo con:

```bash
pwd
```

----

-<u>Una ruta relativa</u> parte del directorio actual.

Supón que estás en `/home/manolo` y quieres entrar en `documentos`:

```bash
cd Documentos
```

Ahora estarás en `/home/alumno/Documentos`.

----

-<u>El símbolo `..` representa el directorio padre</u> (un nivel superior).

```bash
cd ..
```

Si estabas en `/home/alumno/documentos`, ahora estarás en `/home/alumno`.

Puedes comprobarlo con `pwd`:

```bash
/home/alumno
```

----

-<u>El símbolo `~` representa tu carpeta personal</u> (home).

```bash
cd ~
#ó
cd
```

Ambos comandos te llevarán a /home/manolo

----

-Para regresar al <u>último directorio visitado, usa un guion</u>:

```bash
cd -
```

Ejemplo:

```bash
$ pwd
/home/alumno/documentos
$ cd /etc
$ cd -
/home/alumno/documentos
```

---

-El comando <u>`mkdir` (*make directory*) sirve para crear carpetas</u>.  
Sintaxis básica:

```bash
mkdir nombre_carpeta
```

Opciones más comunes

- `-p` → crea también las carpetas padre si no existen.  (ojo si pongo / es una ruta absoluta)

```bash
  mkdir -p /home/tu_nombre/proyectos/2025/enero
```

- `-v` → modo “verbose”, muestra un mensaje por cada carpeta creada.  

```bash
  mkdir -v nueva_carpeta
```

- Crear varias carpetas a la vez:

```bash
  mkdir carpeta1 carpeta2 carpeta3
```

---

### 4.5 Aplicar lo anterior

El objetivo de este apartado es aprender a moverse cómodamente por el sistema de archivos usando los comandos básicos `cd` , `pwd` y `mkdir`. Realiza paso a paso los siguientes ejercicios en tu terminal, observando siempre la salida de `pwd` después de cada movimiento. Adjunta capturas de pantalla.

1.Abre una terminal desde tu usuario normal (no el que creaste en esta práctica) y escribe:

```bash
pwd
```

 Verás algo como:

   ```
   /home/tu_nombre
   ```

---

2.Crea una carpeta llamada `entrenamiento`:

```bash
mkdir entrenamiento
```

---

3..Entra en ella y comprueba dónde estás:

```bash
cd entrenamiento
pwd
```

 Salida esperada:

   ```
   /home/tu_nombre/entrenamiento
   ```

---

4.Dentro de `entrenamiento`, crea tres carpetas:

```bash
mkdir titan pared tropa
```

---

5.Usa `ls` para comprobar que existen:

```bash
ls
```

---

6.Entra en `tropa`:

```bash
cd tropa
pwd
```

---

7.Sube un nivel:

```bash
cd ..
pwd
```

---

8.Vuelve directamente a tu *home*:

```bash
cd ~
pwd
```

---

9.Crea una ruta completa con `mkdir -p`:

```bash
mkdir -p /home/tu_usuario/muro/exterior/seccionA
```

---

10.Entra en esa ruta para comprobar:

```bash
cd ~/muro/exterior/seccionA
pwd
```

---

11.Sube dos niveles de golpe:

```bash
cd ../..
pwd
```

---

12.Vuelve al último directorio visitado: ¿donde acabas?

```bash
cd -
```

---

### 4.6 Asignar permisos con `chown` y `chmod`. Uso de `usermod`

Queremos crear dos recursos compartidos del *Muro* en el sistema: Para conseguir colaboración segura y ordenada tenemos que hacer uso de los siguientes comandos, que explicamos primero con algunos ejemplos para después detallaros un poco más que tenéis que hacer:

---

#### Propietario y grupo propietarios (`chown`)

Cada archivo o directorio en Linux pertenece a:

- un usuario propietario

- un grupo (normalmente el <u>grupo principal</u> del usuario que lo creó)

Esto se puede comprobar con `ls -l` en el directorio en que se encuentre el archivo, por ejemplo si  plan_toma_muro.txt se encuentra en la carpeta home:

```bash
-rw-r--r-- 1 eren exploradores 0 oct 12 10:15 plan_toma_muro.txt
```

Aquí, eren es el propietario y exploradores es el grupo. El propietario y el grupo determinan quién puede hacer qué según los permisos definidos. El comando que permite cambiar estas asociaciones es:

```bash
sudo chown propietario:grupo archivo
```

Por ejemplo:

```bash
sudo chown root:exploradores /srv/inteligencia
```

    significa que:
    
    - root (administrador) es el propietario del directorio.
    
    - El grupo exploradores tiene derechos sobre él.

---

#### **Permisos del directorio** 

Que permitan lectura/escritura al grupo pero no a otros (`chmod`). Linux usa tres conjuntos de permisos (lectura, escritura, ejecución), aplicados a tres tipos de usuarios:

- **u** (*user*): el propietario.
- **g** (*group*): el grupo asignado.+
- **o** (*others*): el resto de usuarios.

Cada conjunto puede tener combinaciones de:

| Permiso   | Letra | Valor numérico | Significado                                                  |
| --------- | ----- | -------------- | ------------------------------------------------------------ |
| Lectura   | `r`   | 4              | Permite ver el contenido (o listar archivos en un directorio `ls`). Necesito previamente permisos de ejecución en el directorio `x` |
| Ejecución | `x`   | 1              | Permite acceder al interior (si es directorio).              |
| Escritura | `w`   | 2              | Permite modificar o borrar información del archivo o Permite *crear, borrar o renombrar archivos dentro del directorio*. Necesito previamente permisos de ejecución en el directorio `x` |

Por ejemplo, un directorio `rwxrwx---` (770) permite acceso total a propietario y grupo, pero bloquea completamente a los demás.

Los permisos se expresan de la siguiente forma:

##### **de forma simbólica:**

```bash
sudo chmod u=rwx,g=rwx,o=--- archivo
```

```bash
sudo chmod u=rwx,g=rwx,o=w hola.txt
```

##### **de forma numérica (octal):**

```bash
sudo chmod 770 archivo
```

(4+2+1 = 7 para usuario y grupo, 0 para otros)

```bash
sudo chmod 772 archivo
```

    (4+2+1 = 7 para usuario y grupo, 2 para otros)

---

#### Comando usermod

Vemos ahora el comando usermod. El comando `usermod` (abreviatura de *user modify*) se usa en Linux para modificar las propiedades de un usuario existente del sistema. Permite cambiar desde su grupo principal o secundarios hasta su shell, su carpeta personal o su fecha de expiración. Algunas de las opciones más interesantes con ejemplos son las siguientes:

| Opción | Significado                                                  | Ejemplo                            |
| ------ | ------------------------------------------------------------ | ---------------------------------- |
| `-aG`  | Añadir el usuario a uno o varios **grupos secundarios**, sin eliminar los existentes. a de append | `usermod -aG exploradores armin`   |
| `-g`   | Cambiar el **grupo principal** del usuario                   | `usermod -g guarnicion mikasa`     |
| `-d`   | Cambiar el **directorio personal (home)**                    | `usermod -d /home/eren_nuevo eren` |
| `-s`   | Cambiar el **shell por defecto**                             | `usermod -s /bin/bash levi`        |
| `-L`   | **Bloquear** la cuenta (no se puede iniciar sesión)          | `usermod -L hange`                 |
| `-U`   | **Desbloquear** la cuenta                                    | `usermod -U hange`                 |
| `-e`   | Establecer una **fecha de expiración** para la cuenta (formato AAAA-MM-DD) | `usermod -e 2025-12-31 armin`      |

 Si quieres quitar a un usuario de un grupo, puedes mirar los grupos en los que está con:

```bash
groups manolo
```

y verás grupo1, grupo2, grupo3, grupo4

y después hacer:

```bash
sudo usermod -G grupo1,grupo2,grupo3 nombre_usuario
```

De esta forma estará en todos menos en grupo 4. Por último vemos el comando `su`, que es **uno de los más clásicos y poderosos** en Linux. Sirve para **cambiar de usuario dentro de la misma terminal** sin cerrar sesión ni abrir otra ventana. Vamos a verlo paso a paso con ejemplos claros.

---

#### Comando su

`su` viene de “substitute user” (sustituir usuario). Permite abrir una sesión temporal como otro usuario. Por defecto, si no le indicas a qué usuario, asume que quieres ser root (el superusuario).

​	🔹 Cambiar al usuario `root`: `su`

​	🔹 Cambiar al usuario `root`: `su root`

​	🔹 Cambiar al usuario `root` como si hubiera iniciado sesión desde 0: `su`

​	🔹 Cambiar al usuario `eren`: `su eren`

​	🔹 Cambiar al usuario `eren` como si hubiera iniciado sesión desde 0 (espacio ente - y eren): `su - eren`

----

#### Un ejemplo de todo lo anterior

Ejemplo de todo lo anterior;

- `/srv/inteligencia` : Crear este espacio para el Cuerpo de Exploración (grupo `exploradores`). Los miembros de ese grupo deben poder colaborar: crear, modificar y borrar ficheros en ese directorio. El resto, sin permisos.

- `/srv/arsenal` : Crear este espacio para la Policía Militar (grupo `policia_militar`) Los miembros de ese grupo deben poder colaborar: crear, modificar y borrar ficheros en ese directorio. Otros podrán listar el contenido

- Comprueba que reiner solo puede listar el directorio (cambia al usuario reiner) pero no puede acceder a la carpeta ni menos crear algo en esa carpeta

- Añadimos a reiner al grupo de policia_militar también (ahora puede acceder a ambos directorios)

  <u>Solución:</u>

``` bash
sudo mkdir -p /srv/inteligencia
sudo mkdir -p /srv/arsenal
sudo chown root:exploradores /srv/inteligencia
sudo chown root:policia_militar /srv/arsenal
sudo chmod 774 /srv/inteligencia
sudo chmod 770 /srv/arsenal
su reiner
cd /srv/arsenal (denied)
su (tu_usuario_real) #Esto nos los ahorraremos cuando veamos sudoers
sudo usermod -aG policia_militar reiner
su reiner
cd /srv/arsenal (allowed)
```

----

### 4.7 Practica de permisos con `chown` y `chmod`. Uso de `usermod`

Explicado lo anterior, se pide lo siguiente:

El Muro sigue reforzando su sistema de información. Se han creado dos nuevas zonas de trabajo que deben ser seguras y accesibles solo para los equipos designados. La administración central (root) supervisará todo, pero cada división gestionará sus propios datos internos.

- Crea dos directorios dentro de `/srv`:
  - `/srv/planificacion` para el grupo `policia_militar`.
  - `/srv/abastecimiento` para el grupo `guarnicion`.

- El propietario debe ser `root` en `/srv/planificacion` , y `hange` en `/srv/abastecimiento`. Cada grupo debe tener acceso completo a su respectivo directorio al igual que el propietario del mismo. Los demás usuarios no deben tener ningún permiso en `/srv/planificacion` y solo read en `/srv/abastecimiento`.

- Comprueba que `armin` no pueder hacer un ls a `/srv/planificacion` y si a `/srv/abastecimiento`

- Comprueba que a pesar de no ser de la `guarnicion`, `hange` puede crear directorios en `/srv/abastecimiento.`

- Además, da a `armin` acceso adicional al directorio `/srv/planificacion` sin cambiar el grupo del archivo (añadiendo a `armin` al grupo correspondiente). Comprueba ahora que armin puede entrar en `/srv/planificacion`


---

### 5. Borrar toda la info de los usuarios creados y grupos

OJO! Antes de borrarlos lo suyo es que mates los procesos de cada usuario así:

```bash
sudo pkill -KILL -u usuario
```

```bash
sudo userdel -r eren

```

Para borrar los grupos ejecuta lo siguiente:

```bash
sudo groupdel nombre_grupo

```

¿Qué pasa si ese grupo es el principal de un usuario?

---

¡Fin de la práctica!
**¡Por la humanidad!** :)