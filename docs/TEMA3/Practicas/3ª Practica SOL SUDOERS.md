



Aquí os dejo las soluciones de algunos apartados, que os pueden venir genial para el examen

### **<u>Ejercicio 3.2</u>**

Añade a un usuario de tu sistema al grupo sudo y comprueba que podrá usar los siguiente comandos con permisos de root o permisos de otros usuarios (devuelve la salida de los mismo):

```passwd 
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

<u>Solución:</u>

En /etc/sudoers hay una regla que dice que cualquier usuario que esté en el grupo sudo, puede ejecutar cualquier comando con los privilegios de cualquier usuario y cualquier grupo, esto se ve aquí:

![image-20251128205642387](../../img/image-20251128205642387.png)

Por tanto, si manolo está en el grupo sudo, podrá ejecutar el comando sudo con los privilegios que le de la gana. Vamos a meterlo a sudo:

```bash
manolo@manolo:/srv/ejemplo_levi$ su root
Contraseña: 
root@manolo:/srv/ejemplo_levi# groupadd sudo
root@manolo:/srv/ejemplo_levi# usermod -aG sudo manolo
```

Ejecuciones:

```bash
manolo@manolo:/srv/ejemplo_levi$ sudo whoami #ejecuto whoami con privilegios de root, la salida es root.
```

```bash
manolo@manolo:/srv/ejemplo_levi$ sudo -u root whoami #esto es lo mismo que lo anterior
root
```

```bash
manolo@manolo:/srv/ejemplo_levi$ sudo -u manolo whoami
manolo #ejecuto con privilegios de manolo (yo mismo)

```

```bash
manolo@manolo:/srv/ejemplo_levi$ sudo passwd root #ejecuto con privilegios de root
Nueva contraseña: 
CONTRASEÑA INCORRECTA: La contraseña tiene menos de 8 caracteres
Vuelva a escribir la nueva contraseña: 
passwd: contraseña actualizada correctamente
```

```bash
manolo@manolo:/srv/ejemplo_levi$ sudo mkdir /ejemplo_practica_3
manolo@manolo:/srv/ejemplo_levi$ ls -ld /ejemplo_practica_3
drwxr-xr-x 2 root root 4096 nov 28 21:00 /ejemplo_practica_3 #como ejecuto con privlegios de root (al poner sudo) el propietario que la ha 
```

---

### **<u>Ejercicio 3.3</u>**

1-Abrir con `visudo` (seguro) y edita al usuario armin. En una linea haz que armin pueda ejecutar el comando mkdir, chown y chmod con permisos de root, pero solo esos. Haz en otro linea que armin pueda ejecutar ls con permisos de eren, pero solo ese usuario.

<u>**Solucion**</u>

```bash
sudo visudo -f /etc/sudoers.d/eren
armin ALL=(root:root) /usr/bin/mkdir, /usr/bin/chown, /usr/bin/chmod
armin ALL=(eren:armin) /usr/bin/ls
```

---

2-Muevete al usuario eren (metelo en el grupo sudo) y en la raiz "/" crea una carpeta llamada "solo_eren" con permisos 771 y cambiale el propietario a eren. Después crea un fichero en esa carpeta llamado "ejemplo.txt" y añade lo que quieras dentro. Comprueba que eren puede hacer un ls sobre /solo_eren y que `armin` no puede.
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

<u>**Solucion**</u>

```bash
manolo@manolo:~$ nu eren
manolo@manolo:~$ su eren
manolo@manolo:~$ sudo usermod -aG sudo eren
manolo@manolo:~$ sudo passwd eren
manolo@manolo:~$ su eren
eren@manolo:/home/manolo$ 
eren@manolo:/$ sudo mkdir solo_eren
eren@manolo:/$ sudo chown eren:eren solo_eren
eren@manolo:/$ sudo chmod 771 solo_eren
eren@manolo:/solo_eren$ cd solo_eren
eren@manolo:/solo_eren$ nano ejemplo.txt
eren@manolo:/solo_eren$ ls
ejemplo.txt
eren@manolo:/solo_eren$ sudo useradd -m -s /bin/bash armin
useradd: warning: the home directory /home/armin already exists.
useradd: Not copying any file from skel directory into it.
eren@manolo:/solo_eren$ sudo passwd armin
Nueva contraseña: 
CONTRASEÑA INCORRECTA: La contraseña tiene menos de 8 caracteres
Vuelva a escribir la nueva contraseña: 
passwd: contraseña actualizada correctamente
eren@manolo:/solo_eren$ su armin
armin@manolo:/solo_eren$ ls -ld .
drwxrwx--x 2 eren eren 4096 nov 28 21:34 .
armin@manolo:/solo_eren$ sudo -u eren ls /solo_eren #la linea numero dos me ha dejado hacer un ls como eren
[sudo] contraseña para armin: 
ejemplo.txt

armin@manolo:/solo_eren$ sudo mkdir /solo_eren/armin_tambien_puede #la linea numero 1 me ha dejado hacer un sudo mkdir
armin@manolo:/solo_eren$ sudo chown armin:armin . #me ha dejado por la primera linea
```

---

3.Abrir con `visudo` (seguro) y edita al usuario armin, en este caso, armin podrá ejecutar todos los comandos con permisos del grupo exploradores (Si no tienes este grupo debes crearlo y solo meter a eren). En este caso, eren creará en /solo_eren la carpeta /exploradores y le dará el grupo propietario exploradores y le asignará permisos 771.  Sobre esta carpeta eren creará el txt llamado (asuntos_secretos.txt).

<u>**Solucion**</u>

```bash
su root
visudo -f /etc/sudoers.d/armin
#escribo lo siguiente:
armin ALL=(ALL:exploradores) ALL
#-----------------------------------
root@manolo:/solo_eren# groupadd exploradores
root@manolo:/solo_eren# sudo usermod -aG exploradores eren
root@manolo:/solo_eren# su eren
#----------------------------------
eren@manolo:/solo_eren$ sudo chown eren:eren .
eren@manolo:/solo_eren$ newgrp exploradores
eren@manolo:/solo_eren$ id -ng #me sale exploradores
eren@manolo:/solo_eren$ mkdir exploradores
eren@manolo:/solo_eren$ mkdir exploradores
eren@manolo:/solo_eren$ ls
armin_tambien_puede  ejemplo.txt  exploradores
eren@manolo:/solo_eren$ chmod 771 exploradores
eren@manolo:/solo_eren$ cd exploradores
eren@manolo:/solo_eren/exploradores$ nano asuntos_secretos.txt

```

---

4.Con armin, intenta borrar este .txt sin sudo ¿influyen los permisos del txt o los permisos de la carpeta que lo contiene? ¿Puedes? Si no puedes, ejecuta el comando apropiado con sudo para borrar este txt.

<u>**Solucion**</u>

```bash
armin@manolo:/solo_eren/exploradores$ rm asuntos_secretos.txt
rm: ¿borrar el fichero regular 'asuntos_secretos.txt'  protegido contra escritura? (s/n) s
rm: no se puede borrar 'asuntos_secretos.txt': Permiso denegado
armin@manolo:/solo_eren/exploradores$ ls -ld .
drwxrwx--x 2 eren exploradores 4096 nov 28 21:47 . #armin no eren ni esta en exploradores entonces armin no puede bajo ningun concepto crear ni borrar nada

armin@manolo:/solo_eren/exploradores$ sudo -g exploradores rm asuntos_secretos.txt

```

---

5.Armin tiene una pelea con eren y trata sobre lo siguiente. Armin dice que si tuviese privilegios para ejecutar con el grupo sudo  cualquier comando (es decir, esta regla)

```bash
armin ALL=(armin:sudo) ALL
```

Podría ejecutar cualquier comando así:

```bash
armin@asir:~$ sudo -g sudo rm -r /solo_eren

```

Sin embargo eren le dice que si puede, pero que el comando lo está escribiendo mal y que todavía no entiende como usar sudo correctamente.

¿Qué opinas? ¿Quien tiene razón?

<u>**Solucion**</u>

Que en parte, armin tiene razón, puede ejecutar cualquier comando con esa regla, pero no está haciendo bien la ejecución. Si el quiere ejecutar con los privilegios del grupo sudo, el comando sería el siguiente:

```bash
sudo -g sudo sudo rm -r /solo_eren
```

