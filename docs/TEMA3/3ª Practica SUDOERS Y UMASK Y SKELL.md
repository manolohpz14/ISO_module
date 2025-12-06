# Práctica 3: Administración de permisos .

**Duración aprox:** 3 clases (semana 10/11/2025)

Entregable: Un documento de google o pdf con los comandos necesarios para la resolución de los ejercicios propuestos insertados.

## Índice

1. Uso de `/etc/skel/` y `umask`

   1.1 ¿Qué archivos puedo poner en `/etc/skel/`? **(Solo teoría)**

   1.2 ¿Por qué no se crean Documentos, Escritorio, Descargas... en home? **(Realizar ejercicio)**

   1.3 .bashrc **(Realizar ejercicio)**

   1.4 .profile **(Solo toería)**

   1.5 umask **(Realizar ejercicio)**

2. Restringir el uso de ciertos comandos a usuarios a grupos y crear tus propios comandos

   2.1. Ejemplo Hacer que no *cualquiera* pueda ejecutar `getent` **(Realizar ejercicio)**

   2.2. Sobre comandos que NO son binarios: los *builtins* del shell **(Solo teoría)**

   2.3.  Scripts y ejecución de scrpts **(Realizar ejercicio)**

   2.4.  Añadir scripts como comandos al path **(Realizar ejercicio)**

3. Uso de los archivos `/etc/sudoers` y el directorio `/etc/sudoers.d/` para añadir reglas sobre el uso de sudo.

   3.1 Archivo `sudoers` (`/etc/sudoers`):

   3.2 Permitir a un usuario ejecutare sudo con permisos de root **(Realizar ejercicio)**
   
   3.3 Creación de reglas propias (en sudoers.d) **(Realizar ejercicio)**

---

### 1.Uso de `/etc/skel/` y `umask`

`/etc/skel/` :  es una carpeta del sistema que funciona como plantilla para nuevos usuarios.  Cuando se crea un usuario con:

```bash
sudo useradd -m -s /bin/bash nombre_usuario
```

El sistema copia todo lo que haya dentro de `/etc/skel/` al nuevo directorio `/home/nombre_usuario/`. Por defecto suele contener:

```bash
  .bashrc
  .profile
  .bash_logout
```

 Si tú añades archivos o carpetas en `/etc/skel/`, todos los nuevos usuarios los recibirán automáticamente al ser creados.

---

  #### 1.1 ¿Qué archivos puedo poner en `/etc/skel/`?

lo más sencillo es añadir una carpeta por defecto:

```bash
sudo mkdir -p /etc/skel/Scripts
```

Luego, al crear el usuario:

```bash
sudo useradd -m -s /bin/bash pepe
```

Su `/home/pepe/` tendrá automáticamente:

  ```
  /home/pepe/Scripts
  ```

---

  #### 1.2 ¿Por qué no se crean Documentos, Escritorio, Descargas, etc.?

Si te fijas en lo anterior, al crear Pepe y hacer:

```bash
sudo ls /home/Pepe
```

Solo aparecen las siguientes carpetas:

```bash
`.bashrc`, `.profile`, `.bash_aliases`, Scripts
```

Pero por ningún lado aparecen las carpetas:

```bash
Escritorio Documentos Descargas Imágenes Música Vídeos Plantillas Público
```

Esas carpetas NO provienen de `/etc/skel/`, sino del entorno gráfico (GNOME, XFCE, KDE) y de la herramienta `xdg-user-dirs`.

Estas carpetas solo se crean si:

  - El usuario inicia sesión en el escritorio gráfico, no solo en terminal.
  - Está instalado `xdg-user-dirs`.  
  - Es la primera vez que el usuario entra al entorno gráfico.

Por eso si haces:

```bash
su pepe
```

Y luego:

```bash
ls /home/pepe
```

Solo verás lo que había en `/etc/skel/` y no todas esas carpetas típicas de lo usuarios. Pero si Pepe inicia sesión desde pantalla gráfica, se crearán automáticamente:

```makefile
  Escritorio
  Documentos
  Descargas
  Imágenes
  Música
  Vídeos
  Plantillas
  Público
```

También se pueden crear esas carpetas de forma firecta sin iniciar sesión e un entorno gráfico:

1. Ejecutando el comando como el usuario (pequeño sopiler de lo que veremos en el punto 3, donde ejecutaremos comandos como usuarios):

```bash
sudo -u pepe xdg-user-dirs-update
```

En este caso, eso es equivalente a:

```bash
su pepe
xdg-user-dirs-update
```

2. Creándolas directamente en `/etc/skel/` para futuros usuarios:

```bash
sudo mkdir -p /etc/skel/{Escritorio,Documentos,Descargas,Imágenes,Vídeos}
```

De esa forma, las carpetas se crearán aunque no haya iniciado sesión en el entorno gráfico el usuario.

**<u>Ejercicio</u>**

Crea en /etc/skell la carpeta Archivos_secretos con mkdir y dentro de la carpeta Archivos secretos crea un txt (con nano) que contenga: "Este archivo es de un usuario".

Después crea un nuevo usuario y comprueba con ls que tanto la carpeta como el archivo dentro de la carpeta se han creado. ¿Con qué permisos se crean por defecto?

---

  #### 1.3. `.bashrc`

Archivo que se ejecuta automáticamente cada vez que abres una terminal interactiva en Bash con un usuario (o al cambiar de sesión en terminal con `su`). Aquí se pueden definir <u>alias, variables</u> o funciones personalizadas. Como todavía no sabemos que es una función, solo veremos las dos primeras cosas.

Un *alias* es como crear un “atajo” para un comando largo. Estos se pueden definir en .bashrc agregando una línea. Por ejemplo, si hacemos:

```bash
nano /home/manolo/.bashrc
```

  Podemos añadir al final del archivo los siguientes "alias"

```bash
# Mostrar todos los archivos, incluso ocultos (los que empiezan por .)
alias la='ls -a'
  
# Actualizar el sistema rápido (en Debian/Ubuntu)
alias actualizar='sudo apt update && sudo apt upgrade -y'
  
# Limpiar la terminal
alias cls='clear'
  
```

y después, recargar:

```bash
source ~/.bashrc
```

De esta forma, cuando el usuario `manolo` escriba en terminal

```bash
actualizar
```

será equivalente a escribir

```bash
sudo apt update && sudo apt upgrade -y
```

Por otro lado, sobre .bashrc podemos definir variables de entorno. Una variable de entorno es un valor (texto) que el sistema o el usuario guarda temporalmente y que puede ser usado por programas, scripts o la terminal. Puedes ver todas las variables ejecutando:

```bash
  printenv
```

algunas de las más importante son las siguientes (dependerán del usuario con quien las ejecutes)

  | Variable | ¿Qué contiene?                            | Ejemplo                        |
  | -------- | ----------------------------------------- | ------------------------------ |
  | `$USER`  | Nombre del usuario actual                 | `manolo`                       |
  | `$HOME`  | Carpeta personal del usuario              | `/home/manolo`                 |
  | `$PATH`  | Rutas donde el sistema busca los comandos | `/usr/local/bin:/usr/bin:/bin` |
  | `$SHELL` | Qué intérprete de comandos estás usando   | `/bin/bash`                    |
  | `$PWD`   | Directorio en el que estás ahora          | `/home/manolo/scripts`         |
  | `$LANG`  | Idioma y codificación del sistema         | `es_ES.UTF-8`                  |

Para ver el valor de una variable de entorno de las anteriores basta hacer:

```bash
echo $HOME
```

Puedes definir variables de entorno específicas para el usuario en .bashrc

```bash
export EDITOR=nano  
# Nombre del usuario como mensaje
export MI_NOMBRE="Manolo"
```

de esta forma,  al hacer (con el usuario manolo) en terminal:

```bash
echo $MI_NOMBRE
```

la salida será: `manolo`

De hecho, podemos incluso añadir más información a variables del sistema ya predefinidas. Si añadimos la siguiente linea en .bashrc

```bash
# Ruta personalizada para guardar scripts propios
export PATH="$HOME/scripts:$PATH"
```

estamos haciendo que `$PATH` pase de valer:

```makefile
/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
```

a valer:

```makefile
/home/tu_usuario/scripts:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
```

  **<u>Ejercicio</u>**

-En /etc/skell no vas a añadir nada más (a diferencia del apartado anterior). Lo que vas a hacer es:

 -Con el editor `nano` vas a editar `.bashrc` en el perfil en `manolo`y vas a añadir un alias llamado "crear" que será crea un usuario con las opciones: `-m`, `-s /bin/bash`. para que hagan efecto, deberás recargar este archivo:

```bash
  source ~/.bashrc
```

-Después haz lo mismo y añade este alias en `.bashrc` pero en el de `/etc/skell`, ¿cuál es la diferencia a hacerlo sobre manolo?

-Con el editor `nano` vas a editar `.bashrc` en el perfil en `manolo`y vas a añadir una variable llamada "dia" que se referirá al día. Para añadirla, deberás añadir lo siguiente:

```bash
  export date=$(date '+%Y-%m-%d %H:%M:%S')
```

-Después haz lo mismo y añade este alias en `.bashrc` pero en el de `/etc/skell`, ¿cuál es la diferencia a hacerlo sobre manolo?

-Por último, crea un usuario nuevo y comprueba que este usuario nuevo que has creado va a poder usar el `alias`: "crear" y puede utilizar la `variable de entorno` `$date` en su terminal.

---

  #### **1.4. `.profile`**

Se ejecuta al entrar en el sistema. No se ejecuta cada vez que abres una terminal nueva como `.bashrc`. Se usa para configurar el entorno del usuario, especialmente variables de entorno globales.

Lo que os tiene que quedar claro es que aquí definiremos variable de entorno que siempre deben de existir, no sólo cuando abramos la terminal. No tiene mucho sentido entrar en detalle en .profile pero lo dejo como concepto teórico.

---

  #### **1.5. `.umask`**

  `umask` significa User Mask o máscara de permisos del usuario. Define qué permisos se desactivan por defecto cuando un usuario crea un archivo o un directorio. Se suele decir que, `umask` resta permisos de los valores por defecto del sistema bajo la siguiente fórmula:

```bash
permisos finales = permisos base - umask
```

Pero esta fórmula sólo funcionará en casos concretos, ya que en el caso de los binario, los permisos base son: 666 mientras que en el caso de las carpetas los permisos base son 777. Vamos a ver como aplicar la fórmula con un umask de 023.

<u>Carpetas:</u>

##### Permisos base (777)

Cada dígito octal → 3 bits binarios:

- 7 → 111
- 7 → 111
- 7 → 111

##### Umask (023)

- 0 → 000
- 2 → 010
- 3 → 011

##### Negamos la umask (~umask)

umask: 111 101 100

**Finalizamos haciendo un AND digito a digito**

```makefile
permisos_base: 111 111 111 
    
umask:         111 101 100 
    
--------------------------
    
AND resultado: 111 101 100
```

```makefile
Estamos haciendo una operación AND dígito a dígito, recordar que:
  
1 AND 1=0
  
1 AND 0=0
  
0 AND 1= 0
0 AND 0 =0

luego obtenemos = 111 101 100 que nos da el número 754=777-023: en este caso funciona la fórmula!
```

<u>Binarios</u>

##### Permisos base (666)

Cada dígito octal → 3 bits binarios:

- 6 → 110
- 6 → 110
- 6 → 110

##### Umask (023)

- 0 → 000
- 2 → 010
- 3 → 011

##### Negamos la umask (~umask)

umask: 111 101 100

**Finalizamos haciendo un AND digito a digito**

```makefile
permisos_base: 110 110 110 
    
umask:         111 101 100
    
---------------------------
    
AND resultado: 111 100 100
```

cuyo resultado es 644, que no es 666-023=643 luego en este caso, la fórmula no funciona.

Visto lo anterior, ya sabemos los permisos finales que vamos a obtener, es decir, si creo un archivo los permisos serán 644 y si creo un directorio, 754. La pregunta es, ¿cómo miro que máscara tiene mi equipo y cómo la cambio? Vamos a ver primero como cambiarla:

```bash
umask 023 #la cambio
nano archivo1 #creo un archivo
mkdir carpeta1 #creo una carpeta
ls -ld archivo1 carpeta1 #visualizo los permisos
```

Salida esperada:

```makefile
-rw-r--r-- archivo1 #644
drwxr-xr-x carpeta1 #754
```

Justo lo esperado. 

En lo anterior, umask 023 sólo servirá para la sesión actual. Si quieres que un usuario específico tengo un umask (específico del usuario) para siempre sin tener que cambiarlo cada vez que me meto en terminal , lo lógico es meter este umask dentro de `.bashrc`. Es decir, si root quiere que manolo siempre tenga 027 como `umask` deberá ejecutar lo siguiente:

```bash
echo 'umask 027' >> /home/manolo/.bashrc
```

  si quiere que paco tenga el `umask 022` deberá hacer lo siguiente:

```bash
echo 'umask 027' >> /home/paco/.bashrc
```

  Ojo, lo anterior también se podría meter en `.profile`.

  Si quieres que todos los usuarios tengan siempre el umask `027` por defecto lo que habría que hacer es meter este `umask` en el `.bashrc` o `.profile` de `/etc/skell` para que cada vez que creee un usuario herenden ese `umask`. 

  También se puede editar `/etc/profile` ( archivo de configuración global  para todos los usuarios del shell Bash y otros shells de login, se ejecuta cada vez que un usuario inicia sesión en el sistema). Si se añade ahí un nuevo umask,  se cambiará para todos los usuarios al que pongamos en este archivo.

  **<u>Ejercicio</u>**

  - Inicia sesión con el usuario que creaste al iniciar la máquina virtual.
  
  - Comprueba con

```bash
umask
```

el umask por defecto.

  - Después, ejecuta:

```bash
umask -S
```

¿Que observas?

  - Desde ese mismo usuario, vas a editar con `nano` su propio `.bashrc` de forma que su umask va a ser 022. Una vez edites `.bashrc` no olvides recargarlo. Crear un archivo y un directorio y comprueba que efectivamente tienen los permisos que por defecto dicta `umask`.
  
  - Desde ese mismo usuario, vas a editar con `nano` al `.bashrc` de `/etc/skel` de forma que su umask va a ser 025. Crea un usuario nuevo y crea un archivo y un directorio con ese nuevo usuario. Comprueba sobre el archivo y el directorio recien creados que sus permisos de creación son los definidos en el umask de `/etc/skell/.bashrc`

---

## 2.Restringir el uso de ciertos comandos a usuarios a grupos y crear tus propios comandos

Aunque no lo parezca, cuando ejecutamos comandos como los vistos hasta ahora (`ls`, `cat`, `useradd`, `groupadd`, `getent`, `chown`, `chmod`, `nano`, `mkdir`, `chage`...), todos se ejecutan a partir de un archivo binario ejecutable (o de un script ejecutable). Como buen archivo, tienen permisos y una ubicación en el sistema de ficheros.

Los ejecutables del sistema se organizan en varios directorios estándar:

- `/bin` y `/usr/bin`comandos esenciales para todos los usuarios (p. ej. `ls`, `cat`, `mkdir`, `chmod`). Contiene los *comandos esenciales* del sistema que pueden ser usados por cualquier usuario, no solo por root.

- `/sbin` y `/usr/sbin` comandos para administración del sistema (p. ej. `useradd`, `groupadd`, `fdisk`). Suelen ser para el administrador (root). Contiene comandos esenciales para la administración del sistema, generalmente destinados a root o administradores. No están pensados para usuarios normales.

```makefile
En sistemas modernos como Ubuntu o Debian actuales, muchos de estos directorios se han unificado, es decir, se han juntado en uno solo de la siguiente forma:
  
  - `/bin con /usr/bin`
  - `/sbin con /usr/sbin`
```

  Esto se puede ver en la siguiente captura

  ![image-20251105230550907](/home/manolo/.config/Typora/typora-user-images/image-20251105230550907.png)

- `/usr/local/bin`, `/usr/local/sbin` programas instalados localmente por el administrador (no gestionados por el gestor de paquetes del sistema). Veremos mas tarde como usar esto.

Aquí os dejo a modo de resumen las rutas donde se encuentras los comandos utilizados hasta ahora en las práctica 1 y 2.

| Comando          | Ruta típica                     |
| ---------------- | ------------------------------- |
| ls               | /bin/ls                         |
| cat              | /bin/cat                        |
| useradd          | /usr/sbin/useradd               |
| usermod          | /usr/sbin/usermod               |
| passwd           | /usr/bin/passwd                 |
| groupadd         | /usr/sbin/groupadd              |
| groupdel         | /usr/sbin/groupdel              |
| chown            | /bin/chown (o /usr/bin/chown)   |
| chmod            | /bin/chmod                      |
| mkdir            | /bin/mkdir                      |
| chage            | /usr/bin/chage                  |
| getent           | /usr/bin/getent                 |
| su               | /bin/su                         |
| userdel          | /usr/sbin/userdel               |
| nano             | /usr/bin/nano                   |
| groups           | /usr/bin/groups                 |
| gpasswd          | /usr/bin/gpasswd                |
| newgrp           | /usr/bin/newgrp                 |
| pkill            | /usr/bin/pkill                  |
| sudo             | /usr/bin/sudo                   |
| visudo           | /usr/sbin/visudo                |
| echo             | builtin (shell) — /usr/bin/echo |
| chmod (ejemplos) | /bin/chmod                      |

#### 2.1. Ejemplo Hacer que no *cualquiera* pueda ejecutar `getent`

(Nunca editar los permisos de los comandos del sistema, esto puede corromper vuestro equipo, de hecho, si hacéis que no cualquiera pueda ejecutar getent, programas como firefox o chrome VAN a dejar de funcionar directamente)

Normalmente `getent` ya es ejecutable por todos. Para comprobarlo:

```bash
ls -l $(which getent) #es equivalente a ls -ld /usr/bin/getent

# salida típica: -rwxr-xr-x 1 root root 123456 jul  1 12:34 /usr/bin/getent
```

Si por alguna razón no fuera ejecutable por otros, puede hacerlo con:

```bash
sudo chmod o-rx /usr/bin/getent
# o, explícito con los permisos octales:
# sudo chmod 755 /usr/bin/getent
```

Comprobación:

```bash
# como usuario normal distinto de root
getent passwd yourusername
#debería dar error
```

Si prefieres que sólo miembros de un grupo puedan ejecutar `getent` (p. ej. `useradmins`):

```bash
# crear grupo si no existe
sudo groupadd useradmins
# cambiar grupo del ejecutable
sudo chown root:useradmins /usr/bin/getent
# quitar permisos para otros y poner permisos de grupo ejecutables
sudo chmod 750 /usr/bin/getent   # rwx r-x ---
# añadir un usuario al grupo (por ejemplo, manolo)
sudo usermod -aG useradmins manolo
```

Ahora sólo root y los miembros del grupo `useradmins` podrán ejecutar `getent`.

**<u>Ejercicio entregable</u>**

Crear el usuario "useradmin". Deberás hacer que él sea el unico usuario del sistema que pueda gestionar los usuarios, es decir, nadie excepto el podrá ejecutar:

`usermod`, `useradd`, `getent`, `userdel`, `groupadd`, `groupdel`, `passwd`, `gpasswd`.

Después, crea el grupo "groupadmins" y añade a un usuario al grupo (para que no te pete el SO). Todas las personas de este grupo podrán ejecutar:

`groupadd`, `groupdel`, `passwd`, `gpasswd`.

pero no podrán leer esto archivos.

<u>**Una vez terminado esto, deshaz los cambios hechos.**</u>

---

#### 2.2 Sobre comandos que NO son binarios: los *builtins* del shell

Algunos comandos como `echo`, `cd`, `pwd`, `exit`, `history`, `umask`, `read`, entre otros, no existen como archivos ejecutables en /bin o /usr/bin, sino que están integrados dentro del propio shell (bash, zsh, dash…). A estos comandos se les llama builtins.

##### ¿Por qué es importante esto?

Porque no podemos cambiar sus permisos con `chmod` ni restringirlos desde el sistema de archivos, ya que no son archivos, sino código interno del intérprete de comandos. Puedes comprobar si un comando es builtin así:

```bash
type comando
```

Ejemplos:

```bash
type echo   # echo is a shell builtin
type cd     # cd is a shell builtin
type pwd    # pwd is a shell builtin
```

#### 2.3 Scripts y ejecución de scripts

Un **script** es un archivo de texto que contiene comandos escritos para el sistema operativo para ejecutarse del tirón.  Se ejecuta línea por línea como si los escribieras manualmente en la terminal.

Características básicas:

- Es un archivo de texto plano que se puede crear con nano
- Comienza con una línea llamada **shebang**, que indica qué intérprete debe ejecutarlo (`bash`, `sh`, etc.). En el siguiente ejemplo decimos que el script se ejecute con /bin/sh
- Necesita permisos de ejecución para poder ejecutarse como programa.

Ejemplo de script (`hola.sh`):

```bash
#!/bin/sh


echo "Hola, mundo"
```

##### <u>Ejemplo: Crear un script</u>

Hacemos nano y en nombre del script.

```bash
nano hola.sh
```

Contenido:

```bash
#!/bin/sh
echo "Hola, mundo"
```

Luego guardas el archivo (`Ctrl + O`, `Enter`, `Ctrl + X`). En este caso este script lo único que hará sera escribir en pantalla "hola mundo". La pregunta es, ¿cómo ejecutamos un script? Basta convertirlo a ejecutable

```bash
chmod +x hola.sh #damos permisos de ejecucion a todos.
```

Para ejecutarlo solo hace falta poner <u>la ruta absoluta del script</u>, que se puede hacer de dos formas:

- se escribe un `.`(carpeta actual) + `ruta relativa del script`, es decir, si la ruta absoluta del script es:

`/home/manolo/scripts/hola.sh` podemos ejecutar el script así:

```bash
manolo@manolo:~/hola$ ./hola.sh
```

- o si quieras directamente la ruta absoluta

```bash
manolo@mi_lapto:~$ /home/manolo/scripts/hola.sh
```

Si quitáis los permisos de ejecución, no os habría dejado ejecutar.

Un apunte importante es que también se puede ejecutar como:

```bash
bash hola.sh
```

y en este caso, solo haría falta permisos de lectura y no de ejecución.

Quedan muchíiiiiiiiiiisimas cosas sobre scripts, que iremos viendo conforme avanzamos.

**<u>Ejercicio entregable</u>**

Crear el script "creación_hola_ls.sh". El script deberá crear una carpeta llamada "hola" de forma relativa a donde lo ejecutes. Hazto seguido, deberá de mirar los permisos de la carpeta hola creada. Después, da permisos de ejecución al script creado y por último, ejecútalo y comprueba que se obtiene lo esperado.

#### 2.4 Añadir scripts como comandos al path

`PATH` le dice al sistema dónde buscar los comandos que escribes en la terminal. Puedes ver su valor con:

```bash
echo $PATH
```

Ejemplo de salida típica:

```
/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
```

Esto significa: Cuando escribes un comando, por ejemplo `ls`, el sistema busca en cada carpeta del PATH (en ese orden) hasta encontrarlo, es decir, cuando haces:

```bash
ls
```

En realidad estás ejecutando el script `/bin/ls` (de esto ya hemos hablando en el punto 1). Por tanto, es equivalente a esto:

```bash
manolo@manolo:$ ./bin/ls
```

Ahora vamos a añadir al path el script creado en el punto anterior, primero, lo movemos a una carpeta que esté en el path (la que quieras)

```bash
sudo mv /home/manolo/scripts/hola.sh /usr/local/bin/hola
```

ojo, en este caso no estamos sólo moviendo, si no que estamos renombrando quitando la extensión sh al archivo. Eliminar `.sh` hace que parezca un comando real del sistema y es lo normal. Si no quisieramos renombrarlo y hacer que mantenga el `.sh` después de hola, en el destino pondríamos lo siguiente:

```bash
sudo mv /home/manolo/scripts/hola.sh /usr/local/bin/
```

de esta forma mantendríamos el nombre original.  Después de moverlo, damos los permisos a nuestro script.

```bash
sudo chown root:root /usr/local/bin/hola
sudo chmod 755 /usr/local/bin/hola
```

ya estamos en condiciones de usar nuestro comando propio. Como le hemos dado el nombre "hola", basta hacer:

```bash
hola
# Salida: Hola, mundo
```

Y... ¡Tu script ahora es un comando del sistema! Los haremos más difíciles, no os precupéis ;)

##### Resumen de como crear tu propio comando a partir de un script

| Paso | Acción                                                       |
| ---- | ------------------------------------------------------------ |
| 1    | Crear `hola.sh` con nano y añade qué quieres que haga el comando |
| 2    | `chmod +x hola.sh` (dale permisos de ejecución para todos y prueba que funciona) |
| 3    | `sudo mv hola.sh /usr/local/bin/hola` (muevelo a una carpeta del paz y quita el .sh) |
| 4    | `sudo chmod 755 /usr/local/bin/hola` (dale permisos de ejecución que tu quieras al comando) |
| 5    | Ejecutar simplemente con `hola`                              |

**<u>Ejercicio entregable</u>**

Crea el comando "mkdir_ls" a partir del script creado en la práctica anterior.

---

## -3. Uso de los archivos `/etc/sudoers` y el directorio `/etc/sudoers.d/` para añadir reglas sobre el uso de sudo.

**`su` vs `sudo`:**

- Ya hemos visto `su` en la práctica 1 y 2 sirve para cambiar de usuario a otro (por ejemplo `su - root` o sólo `su` cambia al usuario `root`) y requiere la contraseña del otro usuario. 
- `sudo` va por otro lado, en vez de tener que cambiar a otro usuario, permite ejecutar comandos específicos con privilegios de otro usuario (es decir, con sudo puedo ejecutar un comando con los permisos de otro usuario sin necesidad de saberme su contraseña)

Supongamos que un comando (por ejemplo `gpasswd`) tienen permisos asociados para que solo levi o root los pueda ejecuta.  Si manolo ejecuta:

```bash
sudo -u levi gpasswd exploradores 
```

en realidad está diciendo: "la persona que ejecuta `gpasswd exploradores` no es `manolo` si no que es `levi`. básicamente sería equivalente a:

```bash
manolo@mi_lapto:~$ su levi
levi@mi_lapto:~$ gpasswd exploradores 
```

pero con la ventaja de no tener que cambiar de usuario ya que puede ser que no nos conozcamos la contraseña de `levi`. 

Otro ejemplo es el siguiente:

```bash
sudo gpasswd exploradores 
```

en realidad está diciendo: "la persona que ejecuta `gpasswd exploradores` no es `manolo` si no que es `root`. básicamente sería equivalente a:

```bash
sudo -u root gpasswd exploradores 
```

y esto a su vez sería equivalente a_

```bash
manolo@mi_lapto:~$ su root
root@mi_lapto:~$ gpasswd exploradores
```

la ventaja es que con sudo no tenemos que sabernos la contraseña de root.

Ojo, en lo anterior, si `levi` no es administrador de exploradores, dará fallo igualmente, pero eso no es lo que configuramos en `/etc/sudoers` lo que configuramos en este archivo llamado `sudoers` es que `manolo` tendrá permisos para ejecutar `gpasswd` como si fuera levi, si el comando falla después porque `levi` en realidad no era administrador de un grupo no es competencias de `sudoers`.

---

#### **3.1 Archivo `sudoers` (`/etc/sudoers`):**

El archivo `sudoers` determina tres cosas fundamentales:

1. **Quién puede usar `sudo`**
    (usuarios o grupos autorizados)
2. **Sobre qué comandos puede usar `sudo`**
    (todos los comandos o solo algunos específicos)
3. **Con los privilegios de qué usuario o grupo se ejecutarán esos comandos**. Por ejemplo, como el usuario `levi` (ver ejemplos de arriba), como el usuario `root` (sin poner -u en sudo), como el grupo `exploradores` (poniendo la opción -g en sudo), etc.

El archivo que indica los permisos con los que puede usar sudo es: `/etc/sudoers` (nunca editar directamente con un editor normal, usar `visudo`).  Realmente la buenas práctica: añadir reglas en archivos dentro de `/etc/sudoers.d/` en vez de editar `/etc/sudoers` directamente.

Para arbir el archivo `sudoers`, se hace así:

```bash
sudo visudo /etc/sudoers
#o solamente
sudo visudo
```

Esto lo abre de forma segura para que no puedas editarlo. Vemos al final del archivo lo siguiente:

```makefile
# User privilege specification
root    ALL=(ALL:ALL) ALL

# Members of the admin group may gain root privileges
%admin ALL=(ALL) ALL

# Allow members of group sudo to execute any command
%sudo   ALL=(ALL:ALL) ALL
```

Estas líneas siguen los siguientes pratrones:

```bash
usuario HOST = (USUARIOS:GRUPOS) COMANDOS
%grupo HOST = (USUARIOS:GRUPOS) COMANDOS
```

Vamos a ver que significa cada una de las tres lineas que vemos con visudo:

---

```bash
root ALL=(ALL:ALL) ALL
```

   Entonces, en este caso:

   - `ALL` antes del `=` → desde cualquier host (ordenador).
   - `(ALL:ALL)` → puede ejecutar comandos como cualquier usuario y como cualquier grupo.
   - Después, `ALL` → puede ejecutar cualquier comando

**Ejemplo práctico**

Supongamos que estás conectado como `root`, podrías ejecutar:

```bash
sudo -u manolo whoami
```

Que devolvería manolo. De hecho en lugar de -u manolo, peudes poner el usuario que quieres y en donde hemos puesto el comando `whoami` podrías poner el comando que quieras. En conclusión: root puede ejecutar cualquier comando (en este caso `whoami`) como cualquier usuario (en este caso `manolo`)

---

```bash
%admin ALL=(ALL:ALL) ALL
```

Entonces, en este caso:

- `admin` nombre del grupo

- `ALL` antes del `=` → desde cualquier host (ordenador).

- `(ALL:ALL)` → puede ejecutar comandos como cualquier usuario

- Después, `ALL` → puede ejecutar cualquier comando

**Ejemplo práctico**

Supongamos que estás conectado como `rigobertoñ`, usuario que está dentro del grupo `admin`. Podrías ejecutar:

```bash
sudo whoami
```

Que devolvería root. De hecho podrías poner -u `levi`, o incluso puedes poner el usuario que quieres y en donde hemos puesto el comando `whoami` podrías poner el comando que quieras. En conclusión: root puede ejecutar cualquier comando (en este caso `whoami`) como cualquier usuario (en este caso `root`)

---

```bash
%sudo   ALL=(ALL:ALL) ALL
```

- `sudo` nombre del grupo

- `ALL` antes del `=` → desde cualquier host (ordenador).

- `(ALL:ALL)` →  puede ejecutar comandos como cualquier usuario y como cualquier grupo.

- Después, `ALL` → puede ejecutar cualquier comando

**Ejemplo práctico**

Vale exactamente el mismo ejemplo que en el caso anterior. El grupo sudo es el más comúnmente usado, así que si quieres que un usuario pueda hacer `sudo` con permisos de root, gracias a esta regla es más que suficiente.

```bash
paco ALL=(root) /usr/bin/passwd
```

- `paco` nombre del usuario

- `ALL` antes del `=` → desde cualquier host (ordenador).

- `(root)` →  puede ejecutar comandos como usuario root

- Después, `passwd` →solo puede ejecutar este comando como root

**Ejemplo práctico**

Desde paco podemos ejecutar solo passwd con permisos de root y cambair la contraseña a cualquier otro usuario.

```bash
sudo passwd manolo
```

Después pedirá la contraseña de root para hacer esto.

---

```bash
armin ALL=(ALL:ALL) NOPASSWD: ALL
```

- `amin` nombre del usuario
- `ALL` antes del `=` → desde cualquier host (ordenador).
- `(ALL:ALL)` →  puede ejecutar comandos como cualquier usuario y como cualquier grupo.
- Después, `ALL` → puede ejecutar cualquier comando, en este caso, sin necesidar de tener que poner la contraseña del usuario con el que va a ejecutar el sudo

**Ejemplo práctico**

Desde paco podemos ejecutar solo passwd con permisos de root y cambair la contraseña a cualquier otro usuario.

```bash
sudo passwd manolo
```

Sin embargo con NOPASSWD no pedirá la contraseña.

---

```bash
eren ALL=(root) /sbin/shutdown, /sbin/reboot
```

- `eren` nombre del usuario
- `ALL` antes del `=` → desde cualquier host (ordenador).
- `(root)` →  puede ejecutar comandos como root
- Después, ` /sbin/shutdown, /sbin/reboot` → puede ejecutar estos comandos

**Ejemplo práctico**

El usuario **eren** puede ejecutar **solo** los comandos `/sbin/shutdown` y `/sbin/reboot` con privilegios de `root`, y **ningún otro** comando mediante `sudo` (a menos que haya otra regla que se lo permita).

---

```bash
%develop ALL=(root) NOPASSWD: /usr/bin/apt update, /usr/bin/apt upgrade
```

- `%develop ` nombre del usuario
- `ALL` antes del `=` → desde cualquier host (ordenador).
- `(root)` →  puede ejecutar comandos como root
- Después, ` /usr/bin/apt update, /usr/bin/apt upgrade` : puede ejecutar estos comandos

**Ejemplo práctico**

cualquiera en el grupo develop puede ejecutar solo los comandos usr/bin/apt update, /usr/bin/apt upgrade con privilegios de `root`, y ningún otro comando mediante `sudo` (a menos que haya otra regla que se lo permita), es decir, pueden hacer:

```bash
sudo apt update
```

sin problemas.

---

#### **3.2 Permitir a un usuario ejecutare sudo con permisos de root**

Con las reglas de antes, si añadimos a alguien al grupo sudo, bastará

```bash
sudo usermod -aG sudo nombre_usuario
```

Con esto, el usuario podrá ejecutar cualquiero comando con sudo, ya que vimos que `/etc/sudoers`sudo tenía la siguiente regla:

```bash
%sudo ALL=(ALL:ALL) ALL
```

la cualquier permite `sudo` ejecutar a todos los usuarios del grupo desde:

- cualquier usuario, cualquier grupo, cualquier comando

**<u>Ejercicio</u>**

Añade a un usuario de tu sistema al grupo sudo y comprueba que podrá usar los siguiente comandos con permisos de root o permisos de otros usuarios (devuelve la salida de los mismo):

```bash
sudo whoami
```

```bash
sudo -u manolo whoami
```

```bash
sudo passwd root
```

```bash
sudo mkdir /ejemplo_practica_3
```

---

#### 3.3 Creación de reglas propias (en sudoers.d)

También podemos crear reglas sobre como los usuarios específicos que no estén en sudo tendrán permisos específicos al ejecutar el comando sudo. Esto no se hace directamente sobre `etc/sudoers` ya que para eso se usa el archivo: `/etc/sudoers.d/`.

Para crear reglas en `/etc/sudoers.d` usaremos el editor `visudo` de nuevo. Imaginamos que queremos crear una nueva regla para eren:

```bash
sudo visudo -f /etc/sudoers.d/eren
```

y al final del editor, añadimos:

```text
eren ALL=(root) /sbin/shutdown, /sbin/reboot
```

Con esto, ya tendríamos lo que buscabamos , que es que eren pueda apagar y reiniciar como root (sobre el resto de comandos que necesiten de root no decimos nada aunque puede que haya otras reglas que le permita ejecutarlos)

**<u>Ejercicio</u>**

1. Abrir con `visudo` (seguro) y edita al usuario armin. En una linea haz que armin pueda ejecutar el comando mkdir, chown y chmod con permisos de root, pero solo esos. Haz en otro linea que armin pueda ejecutar ls con permisos de eren, pero solo ese usuario.

2. Muevete al usuario eren (metelo en el grupo sudo) y en la raiz "/" crea una carpeta llamada "solo_eren" con permisos 771 y cambiale el propietario a eren. Después crea un fichero en esa carpeta llamado "ejemplo.txt" y añade lo que quieras dentro. Comprueba que eren puede hacer un ls sobre /solo_eren y que `armin` no puede.
   Después, con armin ejecuta: 

```bash
   sudo -u eren ls /solo_eren
```

   ¿qué linea de la que añadiste en el punto 1 te ha permitido hacer esto y por qué?

   Después comprueba que armin no puede crear nada sobre ``solo_eren`` pero ejecutando:

```bash
   sudo mkdir /solo_eren/armin_tambien_puede
```

   comprueba que si puedes, ¿qué linea de la que añadiste en el punto 1 te ha permitido hacer esto y por qué?

   Por último, y utilizando sudo, armin decide apropiarse de la carpeta de eren y darle permisos para que sólo el puede utilizarla. Explica como lo harías.

3. Abrir con `visudo` (seguro) y edita al usuario armin, en este caso, armin podrá ejecutar todos los comandos con permisos del grupo exploradores (Si no tienes este grupo debes crearlo y solo meter a eren). En este caso, eren creará en /solo_eren la carpeta /exploradores y le dará el grupo propietario exploradores y le asignará permisos 771.  Sobre esta carpeta eren creará el txt llamado (asuntos_secretos.txt)
4. Con armin, intenta borrar este .txt sin sudo ¿influyen los permisos del txt o los permisos de la carpeta que lo contiene? ¿Puedes? Si no puedes, ejecuta el comando apropiado con sudo para borrar este txt.

5. Armin tiene una pelea con eren y trata sobre lo siguiente. Armin dice que si tuviese privilegios para ejecutar con el grupo sudo  cualquier comando (es decir, esta regla)

```bash
armin ALL=(armin:sudo) ALL
```

Podría ejecutar cualquier comando así:

```bash
armin@asir:~$ sudo -g sudo rm -r /solo_eren

```

Sin embargo eren le dice que si puede, pero que el comando lo está escribiendo mal y que todavía no entiende como usar sudo correctamente.

¿Qué opinas? ¿Quien tiene razón?







