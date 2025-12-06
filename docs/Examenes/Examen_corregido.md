# EXAMEN TEMA 3 ISO. ADMINISTRACIÓN DE USUARIOS GRUPOS Y PERMISOS DE FORMA LOCAL EN LINUX

## Ejercicio 1 (3,5pts)

Todo este ejercicio se deberá hacer con un umask 002.

#### Apartado a) (0,4pts)

1-Se pide que crees los siguiente grupos:

```text
analitica, cumplimiento, calidad
```

```bash
sudo groupadd analitica
sudo groupadd cumplimiento
sudo groupadd calidad
```



#### Apartado b) (0,6pts)

1-Acto seguido, crea los siguientes usuarios: 

`manolo`, `isai`, `bea`, `celia`, `alvaro`, `enric`, de forma que, todos tendrán la terminal /bin/bash excepto isai, que no tendrá acceso a terminal. Los grupos principales de cada usuario serán:

```bash
sudo useradd -m -s /bin/bash manolo
sudo useradd -m -s /bin/false -g cumplimiento isai
sudo useradd -m -s /bin/bash bea
sudo useradd -m -s /bin/bash celia
sudo useradd -m -s /bin/bash alvaro
sudo useradd -m -s /bin/bash enric
```



`manolo:manolo`, `isai:cumplimiento`, `bea:bea`, `celia:celia`, `alvaro:alvaro`, `enric:enric`

y cada grupo tendrá los siguiente usuarios:

`analitica: celia, alvaro,enric, manolo` , `cumplimiento: bea, manolo, isai`, `calidad: enric`

```bash
sudo usermod -aG analitica,cumplimiento manolo
sudo usermod -aG cumplimiento bea
sudo usermod -aG cumplimiento isai
sudo usermod -aG calidad enric
sudo usermod -aG analitica celia
sudo usermod -aG analitica alvaro
```



2-Ponle a todos los usuarios la misma contraseña (lo mas sencilla posible).

```bash
sudo passwd manolo
sudo passwd bea
sudo passwd isai
sudo passwd enric
sudo passwd celia
sudo passwd alvaro
```



3-Importante, añade también al usuario manolo al grupo sudo. Esto le permitirá ejecutar comandos con sudo.

```bash
sudo usermod -aG sudo manolo
```



#### Apartado c) (0,3pts)

1-Usando getent, muestra que los grupos creados tienen a dichos usuarios dentro de cada grupo. 

```bash
getent gshadow analitica
getent gshadow cumplimiento
getent gshadow calidad
```



2-Usa también getent para comprobar que isai no tiene acceso a terminal.

```bash
getent passwd isai
```



#### Apartado d) (0,6pts)

1-Ejecutad un comando para ver la política de expiración de contraseñas de manolo

```bash
sudo chage -l manolo
```

2-Después, ejecutad un comando para que manolo solo pueda cambiar la contraseña una vez por semana, y que solo pueda usarse una contraseña cada 6 meses, además, el usuario deberá ser avisado con dos semanas de antelación antes de que caduque. El usuario deberá cambiar la contraseña en 15 días depues de que caduque o su cuenta expirará

```bash
sudo chage -m 7 -M 180 -W 14 -I 15 manolo

```

3-Una vez hecho todo lo anterior, comprueba que efectivamente, se ha cambiado viendo nuevamente la política de contraseñas de manolo, después con sudo, cambia la fecha de cambio de la ultima contraseña a el martes pasado.

```bash
sudo chage -l manolo
sudo chage -d manolo 2025-11-11
sudo chage -l manolo
```



#### **Apartado e)** (0,6pts)

-1.Cambia al usuario `bea` yendo a su home directamente

```bash
su - bea
```



-2.Crea, en el home de `bea` la carpeta `cumplimiento`, ¿cuál es el propetario y grupo principal de la carpeta creada (justificalo con comando)?, ¿qué permisos tiene por defecto la carpeta creada? (justificalo con comando) 

```bash
mkdir cumplimiento
ls -ld cumplimiento
```

Justificar aquí viendo los permisos con el comando anterior



-3.Una vez has creado la carpeta anterior, muévete al directorio raiz del sistema con `bea`. Si cambias de usuario a `manolo`, ¿podrá acceder manolo `/home/bea/cumplimiento`? En caso de que no pueda, deberás editar los permisos octales de los directorios necesarios para que manolo pueda acceder a `home/bea/cumplimiento`.

```bash
cd /
su manolo
sudo chmod o+x /home/bea
```



#### **Apartado f)** (1 pts)

-1.Viendo que la carpeta de `/home/bea/cumpliento` no funcionó como bea quería, `manolo` creará en `/srv` la carpeta `/srv/santander/cumplimiento`, ¿podrá manolo crear esa carpeta o necesitará de `sudo`? Justifica tu respuesta y crea esa carpeta con un solo comando `mkdir`y no con dos.

```bash
ls -ld / #debe usar sudo
sudo mkdir -p /srv/santander/cumplimiento

```



-2.Haciendo uso de la forma octal de los permisos, da a la carpeta `/srv/santander` permisos de lectura escritura y ejecución para todos, además, haz que el usuario y grupo propietario de esta carpeta sea `root`. 

```bash
sudo chmod 777 /srv/santander
sudo chown root:root /srv/santander
```



3-Ahora, con `chmod`, manolo  cambia el grupo propietario de `/srv/santander/cumplimiento` a: `cumplimiento` y  el propietario a `manolo`. Dale los permisos por defecto `776` sin usar sudo.  

```bash
sudo chown manolo:cumplimiento /srv/santander/cumplimiento
chmod 776 /srv/santander/cumplimiento
```

Los permisos los da manolo

-4.Cambia a bea y comprueba (justificando siempre la respuesta) si:

- Puede acceder a la carpeta `/srv/santander/cumplimiento`
- Puede crear la carpeta `/srv/santander/cumplimiento/bea`
- puede hacer `ls` sobre `/srv/santander/cumplimiento`

```bash
cd /srv/santander/cumplimiento #puede
mkdir /srv/santander/cumplimiento/bea
ls /srv/santander/cumplimiento

#puede hacer todo explicar a través de los permisos
```



-5.Prueba todo lo anterior con enric, justificando correctamente si puede acceder a  `/srv/santander/cumplimiento`, crear  sobre `/srv/santander/cumplimiento` (en este caso la carpeta  `/srv/santander/cumplimiento/enric`) o listar en  `/srv/santander/cumplimiento`. ¿Cúal es el permiso que un admnistrados debe añadir a enric para que pueda hacer todo lo anterior? (escribe y no ejecutes el comando)

```bash
cd /srv/santander/cumplimiento
mkdir /srv/santander/cumplimiento/bea
ls /srv/santander/cumplimiento

#no deja hacer nada

#el comando de manolo
chmod 777 /srv/santander
```



## Ejercicio 2 (4pts)

#### Apartado a) (0,5pts)

Cambia al usuario `alvaro` y crea dentro de `/srv/santander` los siguientes archivos:

- bau_sep.txt-> Para crear este archivo, usarás `nano` y escribirás dentro "Tareas de septiembre"

- bau_oct.txt-> Para crear este archivo, usarás `cat` y `>` y posteriormente dentro "Tareas de octubre"

- bau_nov.txt-> Para crear este archivo, usarás `echo` + "Tareas de noviembre" y `>`.

- work_dic.txt-> Usa lo que quieras para crear este archivo y escribirás dentro "Tareas de diciembre"

  ```bash
  alvaro@manolo:/srv/santander$ nano bau_sep.txt
  alvaro@manolo:/srv/santander$ cat > bau_oct.txt
  Tareas de octubre
  alvaro@manolo:/srv/santander$ echo "Tareas de noviembre" > bau_nov.txt
  alvaro@manolo:/srv/santander$ echo "Tareas de diciembre" > work_dic.txt
  
  ```

  

#### Apartado b)  (0,35pts)

1- Mediante el usuario root y con chown, cambia el grupo propietario de los siguientes archivos a `cumplimiento` (deja el propietario igual)

- bau_sep
- bau_nov

```bash
su root
chown :cumplimiento bau_sep.txt bau_nov.txt
```



2- ¿Álvaro podría cambiar el grupo propietario a `cumplimiento` sin necesidad de cambiar a root?

```bash
No, ya que no pertenece ni al grupo cumplimiento ni es usuario con sudo
```





#### Apartado c) (1pts)

1-Da los siguientes permisos a cada uno de los archivos sin sudo, siempre usando la forma octal de los permisos:

- bau_sep.txt: u=rwx g=rw o=rw
- bau_oct.txt: u=rwx g=- o=-
- bau_nov.txt: u=rwx g=- o=r
- work_dic.txt: u=- g=rw o=w

```bash
root@manolo:/srv/santander# chmod 766 bau_sep.txt
root@manolo:/srv/santander# chmod 700 bau_oct.txt
root@manolo:/srv/santander# chmod 704 bau_nov.txt
root@manolo:/srv/santander# chmod 062 work_dic.txt
```



2-Cambia al usuario `manolo`, ¿puede leer el archivo `bau_sep.txt`?, ¿y el archivo `bau_nov.txt`? Justifica tu respuesta

```bash
su manolo
cat bau_sep.txt #Se puede ver ya que manolo pertenece a l grupo y tiene lectura
cat bau_nov.txt #No puede ya que el grupo no tiene permisos de nada

```



3-Cambia al usuario `celia`, ¿puede bea leer `bau_nov.txt`? , ¿puede bea añadir contenido al archivo `work_dic.txt`? ? ¿y ver lo que ha añadido? Justifica tu respuesta.

```bash
su celia
cat bau_nov.txt #Se puede ver ya que otros tienen permisos de lectura
cat >> work_dic.txt #si le de deja pensad por qué
cat work_dic.txt #no le de deja pensad por qué

```



4-Cambia al usuario `alvaro`, ¿puede alvaro leer `work_dic.txt`?

```bash
cat work_dic.txt #No puede, el propietario no tiene permisos
```



#### Apartado d) (0,4pts)

1-Con el usuario `alvaro`, da el comando que liste todos los achivos de `/srv/santander` que empiecen por b y terminen en txt.

```bash
alvaro@manolo:/srv/santander$ ls b*.txt
bau_nov.txt  bau_oct.txt  bau_sep.txt

```



2-Con el usuario `alvaro`, da el comando que liste todos loar achivos de `/srv/santander` que empiecen por b o w y tengan exactamente 6 caracteres antes de txt.

```bash
alvaro@manolo:/srv/santander$ ls [wb]??????.txt
bau_nov.txt  bau_oct.txt  bau_sep.txt
```



#### Apartado e) (1,75pts)

1-Cambia al usuario manolo.

- Comprueba su grupo principal usando id y las opciones necesarios
- Usa el comando `newgrp` para que el grupo principal de manolo durante esta sesión sea `analitica`. Comprueba con id y las opciones necesarias que el grupo principal ha cambiado
- Crea la carpeta `/srv/santander/cumplimiento/analitica`, ¿quién es el propietario de la carpeta y el grupo propietario de la carpeta?  

```bash
su manolo
id -ng #manolo
newgrp analitica
id -ng
mkdir /srv/santander/cumplimiento/analitica
ls -ld /srv/santander/cumplimiento/analitica 
drwxr-xr-x 2 manolo analitica 4096 nov 18 10:02 /srv/santander/cumplimiento/analitica

```

2- sin `sudo` da a esta carpeta  `/srv/santander/cumplimiento/analitica` los permisos `755`, ¿era necesario realmente dárselos o ya estaban por defecto? Justifica tu respuesta.

```bash
chmod 755 analitica
#eran necesario si no eran 775
```



3-Ahora haz que el grupo `analitica` tenga una contraseña y que el administrador único sea `alvaro`. Cambia al usuario `alvaro` y añade a `bea` al grupo analítica. Después, elimínala del grupo (usa la opcion -d en vez de -a)

```bash
sudo gpasswd analitica
sudo gpasswd -A alvaro analitica
su alvaro
gpasswd -a bea analitica
gpasswd -d bea analitica
```



4- Usa el comando  `cat` para mostrar información completa de todos los grupos y fijate al final en`analitica`, con sus usuarios y administradores

```bash
sudo cat /etc/gshadow
```



5-Con `manolo` haz un `newgrp` a `cumplimiento` y, crea el `txt`: `/srv/santander/cumplimiento/analitica/examen.txt` que contenga: `Examen del 18 de noviembre` y dale los permisos de lectura y escritura a usuario y grupo propietario así como nada a otros.

```bash
su manolo
newgrp cumplimiento
manolo@manolo:/srv/santander/cumplimiento$ mkdir manolo@manolo:/srv/santander/cumplimiento/analitica$ nano examen.txt
chmod 660 examen.txt

```



6-Posiciónate en la carpeta `/srv/santander` con y acto seguido, cambia al usuario `enric`. Escribe el comando que debe usar enric para leer el txt, ¿tiene permisos para leerlo? Justifica si puede o no y el porqué.

```bash 
cd /srv/santander
su enric
cat cumplimiento/analitica/examen.txt

#no puede
```



7-Posiciónate en la carpeta `/srv/santander` con y acto seguido, cambia al usuario `bea`. Escribe el comando que debe usar bea para leer el txt, ¿tiene permisos para leerlo? Justifica si puede o no y el porqué. 

```bash
cd /srv/santander
su bea
cat cumplimiento/analitica/examen.txt

#puede
```



8-Si cambio los permisos de `srv/santander/cumplimiento/analitica/examen.txt` a: `777`, ¿podrá enric leer y editar ahora este txt? Justifica tu respuesta.

```bash
su root
chmod 777 /srv/santander/cumplimiento/analitica/examen.txt
cd /srv/santander
su enric
cat cumplimiento/analitica/examen.txt

#sigue sin poder
```



## Ejercicio 3 (2,5pts)

En primer lugar, revisa que en `/etc/skel` no tiene nada más que los archivos ocultos por defecto y no hay nada residual de las prácticas en clase.

#### Apartado a) (0,5pts)

1-Con root y de forma relativa, crea una carpeta en `/etc/skel` llamada `Otros`.

```bash
root@manolo:/srv/santander# cd /etc/skel
root@manolo:/etc/skel# mkdir Otros
```

2- Crea el usuario `raul_roberto` con la opción necesaria para que se cree su home por defecto. Al crearlo, comprueba que la carpeta `Otros` se ha creado en su home.

```bash
root@manolo:/etc/skel# useradd -m -s /bin/bash raul_roberto
root@manolo:/etc/skel# ls -la /home/raul_roberto
total 24
drwxr-x---  3 raul_roberto raul_roberto 4096 nov 18 10:23 .
drwxr-xr-x 35 root         root         4096 nov 18 10:23 ..
-rw-r--r--  1 raul_roberto raul_roberto  220 mar 31  2024 .bash_logout
-rw-r--r--  1 raul_roberto raul_roberto 3863 nov 14 21:11 .bashrc
drwxr-xr-x  2 raul_roberto raul_roberto 4096 nov 18 10:22 Otros
-rw-r--r--  1 raul_roberto raul_roberto  807 mar 31  2024 .profile

```



#### Apartado b) (0,5pts)

1- Con el usuario `manolo`, en `/manolo/home/.bashrc` añade `umask 025`.

```bash
su manolo
nano /home/manolo/.bashrc
```



2- Recarga el `.bashrc`

```bash
source /home/manolo/.bashrc
```



3- Crea en `/home/manolo`:

- La carpeta Examen
- Dentro de la carpeta examen, crearás el txt: `notas.txt`

```bash
manolo@manolo:/etc/skel$ mkdir /home/manolo/Examen
manolo@manolo:/etc/skel$ cd /home/manolo
manolo@manolo:~$ nano Examen/notas.txt
manolo@manolo:~$ ls -ld Examen
drwxr-x-w- 2 manolo manolo 4096 nov 18 10:26 Examen
manolo@manolo:~$ ls -ld Examen/notas.txt
-rw-r---w- 1 manolo manolo 10 nov 18 10:26 Examen/notas.txt

```



Especifica los permisos por defecto anteriores ¿Cuadran con el `umask` definido?

```text
En archivos no cuadran
```



#### Apartado c) (0,5pts)

1-En `/home/manolo/.bashrc` y usando `nano`, define un alias que haga lo siguiente: 

Un alias llamado sudoadd: Este alias debe añadir correctamente a un usuario al grupo sudo.

```bash
nano /home/manolo/.bashrc
alias sudoadd='sudo usermod -aG sudo'
manolo@manolo:~$ source /home/manolo/.bashrc

```



2- Prueba el alias sobre el usuario `celia` y comprueba que efectivamente se ha añadido al grupo `sudo`

```bash
manolo@manolo:~$ sudoadd celia
[sudo] contraseña para manolo: 
manolo@manolo:~$ su celia
Contraseña: 
To run a command as administrator (user "root"), use "sudo <command>".
See "man sudo_root" for details.

#Y ahora, me deja ejecutar con celia comandos como admin
celia@manolo:/home/manolo$ sudo passwd manolo
[sudo] contraseña para celia: 
Nueva contraseña: 

```



#### Apartado d) (0,5pts)

¿Qué debes hacer para que todo usuario nuevo a partir de ahora posea este alias? Especifícalo y ejecuta los comandos necesarios comprobando que el nuevo usuario puede ejecutar el comando sudoadd

```bash
#meter el a usuario en etc/skell
sudo nano /etc/skel/.bashrc
alias sudoadd='sudo usermod -aG sudo'
manolo@manolo:~$ sudo useradd -m -s /bin/bash paco25
manolo@manolo:~$ sudo passwd paco25
manolo@manolo:~$ sudoadd paco25
manolo@manolo:~$ su paco25
manolo@manolo:~$ sudoadd enric

```



#### Apartado e) (0,5pts)

En este apartado crearás el comando `adios` que deberá ser disponible para todos los usuarios del sistema. Cada vez que alguien escriba `adios` deberá aparecer el siguiente mensaje por pantalla:

`Hasta luego!` 

Describe los pasos necesarios para que esto funcione.

Con nano, crear un .sh en alguna carpeta (como /home/manolo/script.sh) que su respectivo shebang #!/bin/bash que contenga:

```bash
echo "Hasta luego!"
```

y después dos opciones:

- Añadir la carpeta donde se creo al path

- mover dicho archivo (como en la práctica 3) a la carpeta /bin

  ```bash
  mv /home/manolo/script.sh /bin/adios
  ```

  