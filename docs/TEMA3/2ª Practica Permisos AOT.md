# Práctica 2 — Administración de permisos (AOT) — Temas imprescindibles

**Duración aprox:** 3 clases (semana 03/11/2025)

Entregable: Un documento de google o pdf con los comandos insertados de los puntos: **<u>7.Parte Práctica </u>** y **<u>8. Parte Práctica</u>**.de esta práctica. No hace falta ejecutar los ejemplos de los puntos 1 al 6.

## Índice

1. Anexo I. El comando `chwon` **(no hay que hacer nada solo teoría)**
2. Anexo II. El comando `chmod` **(no hay que hacer nada solo teoría)**
3. Anexo III. El comando `ls` **(no hay que hacer nada solo teoría)**
4. Anexo IIII. El comando `nano `**(no hay que hacer nada solo teoría)**
5. Anexo V. El comando `cat` **(no hay que hacer nada solo teoría)**
6. Anexo VI. Contraseñas de grupo y algunos comandos `newgrep` **(no hay que hacer nada solo teoría)**

7. Primer Apartado Práctico. Ejercicio de continuación Práctica 1.  (**Entregar ejercicio**)

8. Segundo Apartado Práctico. Contraseñas de grupo y algunos comando `newgrep` ** (**Entregar ejercicio**)**

<u>**->Práctica siguiente (SUDOERS Y UMASK)**</u>

## ANEXO I. PRÁCTICA 1: El comando `chown`. ¿Quién puede usarlo?

Solo root (o un proceso con privilegios de superusuario) puede cambiar el propietario de un archivo.
 El motivo es simple: si cualquiera pudiera cambiar la propiedad de archivos, podría adueñarse de archivos ajenos y romper la seguridad del sistema.  Excepción parcial. El propietario sí puede cambiar el grupo de su archivo, si pertenece al grupo destino.

Ejemplo:

```BASH
# Eren pertenece también al grupo 'paradis'
chown :paradis /home/eren/notas.txt #cambiamos el grupo propietario de este ar
```

## ANEXO II. PRÁCTICA 1: El comando `chmod`. ¿Quién puede usarlo?

El propietario del archivo puede cambiar sus propios permisos. El root puede cambiar los permisos de cualquier archivo. Los demás usuarios no pueden modificar los permisos de archivos ajenos.

## ANEXO III. PRÁCTICA 1: El comando ls y sus opciones. Búsqueda de patrones

En primer lugar vemos las opciones que tiene el comando ls viendo ejemplos:

| Opción | Descripción                                                  | Ejemplo        |
| ------ | ------------------------------------------------------------ | -------------- |
| `-l`   | Muestra lista detallada (tipo, permisos, dueño, grupo, tamaño, fecha). | `ls -l /etc`   |
| `-a`   | Incluye archivos ocultos (`.bashrc`, `.git`, etc.).          | `ls -a`        |
| `-d`   | Muestra info del directorio en sí, no su contenido.          | `ls -ld /home` |
| `-t`   | Ordena por fecha de modificación (más reciente primero).     | `ls -lt`       |
| `-r`   | Invierte el orden (por nombre, fecha, etc.).                 | `ls -lr`       |
| `-S`   | Ordena por tamaño.                                           | `ls -lS`       |
| `-R`   | Muestra contenido recursivo (subcarpetas incluidas).         | `ls -R`        |

- ##### ls -l /etc

Lista con información detallada de cada archivo dentro de /etc:

```
-rw-r--r-- 1 root root  297 oct 27 10:12 hosts
drwxr-xr-x 2 root root 4096 oct 28 09:50 network
```

- ##### ls -a

Incluye los archivos ocultos (los que empiezan por `.`) dentro del directorio actual:

```
.  ..  .bashrc  .profile  Documentos  Imágenes
```

- ##### ls -ld /home

Muestra solo la información del directorio `/home`, no su contenido:

```
drwxr-xr-x 4 root root 4096 oct 28 10:15 /home
```

- ##### ls -lt /var/log

Ordena los archivos por fecha (más recientes primero) dentro de /var/log:

```
-rw-r--r-- 1 root root  25K oct 28 10:10 syslog
-rw-r--r-- 1 root root  18K oct 27 23:40 auth.log
```

- ##### ls -lr /etc

Muestra el listado en orden inverso (por nombre) dentro de /etc:

```
zsh_command_not_found
xdg
xattr.conf
...
```

- ##### ls -lS /var/log

Ordena por tamaño (los archivos grandes primero) dentro de /var/log:

```
-rw-r--r-- 1 root root 10M oct 28 10:10 kern.log
-rw-r--r-- 1 root root  25K oct 28 10:10 syslog
```

- ##### ls -R /home

Muestra todo el contenido recursivamente (subcarpetas incluidas):

### Búsqueda de patrones en ls

Ahora vemos como buscar archivos con ciertos patrones en ls:Ejemplos de uso de ls con patrones

##### ls a*

Muestra todo lo que empieza por “a” en el directorio actual 
Contenido: `archivo.txt`, `app.log`, `audio.mp3`, `nota.txt`  
Resultado:

```
archivo.txt  app.log  audio.mp3
```

##### ls /home/manolo/prueba/A*

Archivos que empiezan por mayúscula “A” en /home/manolo  
Contenido: `Archivo.txt`, `App`, `Agenda.pdf`, `auto.mp3`  
Resultado:

```
Archivo.txt  App  Agenda.pdf
```

##### ls -lt /home/manolo/prueba/a??? 

Nombres que empiezan por “a” y tienen exactamente 4 caracteres además de mostrar información detallada y ordenar por modificacion
Contenido: `auto`, `azul`, `a123`, `amor.txt`, `a`  
Resultado:

```
auto  azul  a123
```

##### ls a*.txt

Archivos que empiezan por “a” y terminan en `.txt`  
Contenido: `archivo.txt`, `apuntes.txt`, `a.txt`, `audio.mp3`  
Resultado:

```
archivo.txt  apuntes.txt  a.txt
```

##### ls a???.txt

Nombres que empiezan por “a” y tienen exactamente 4 caracteres y terminan en txt 
Contenido: `auto`, `azul`, `a123`, `amor.txt`, `a`  
Resultado:

```
amor.txt
```

##### ls *log

Todo lo que termina en “log”  
Contenido: `system.log`, `auth.log`, `catalog`, `logs`  
Resultado:

```
system.log  auth.log
```

##### ls [abc]*

Archivos que empiezan por a, b o c  
Contenido: `auto`, `barco`, `coche`, `dado`, `pez`  
Resultado:

```
auto  barco  coche
```

##### ls -d [abc]*/

Mostrar solo los directorios que empiezan por una letra (a, b o c por ej.)

Contenido: `auto`, `barco`, `coche/`, `dado`, `pez`  
Resultado:

```makefile
coche/
```

##### ls [abd]????.txt`

Archivos que empiezan por a, b o c les siguen 3 letras y terminan en txt
Contenido: `autos.txt`, `barcos.txt`, `dados`, `dados.txt`, `auto`  
Resultado:

```
autos.txt dados.txt
```

##### ls [0-9]*

Archivos o carpetas que empiezan por número  
Ejemplo:  
Contenido: `1-notas.txt`, `2023`, `7zip.tar`, `archivo.txt`  
Resultado:

```
1-notas.txt  2023  7zip.tar
```

##### ls [0-9]??[ab]*

Archivos o carpetas que empiezan por un número seguido de dos letras cualquiera y después las letras a y b.
Contenido: `1-notas.txt`, `2023`, `7zip.tar`, `2-tamizo.txt`, `123bazoca`  
Resultado:

```
123bazoca 2-tamizo.txt
```

##### ls [!ab]*

Todo lo que NO empieza por “ab”  
Contenido: `archivo.txt`, `barco`, `coche`, `.oculto`, `zorro`  
Resultado:



## ANEXO IV. PRÁCTICA 1: El comando `nano`

Con nano puedes crear un archivo con cualquier nombre y extensión, incluso cosas como `.zip`, `.pdf` o `.jpg`. Nano no tiene problema en guardar lo que escribas con el nombre que tú le pongas.

nano solo trabaja con texto plano, no con datos binarios. Así que aunque tú guardes un archivo llamado `archivo.zip`, lo único que estás haciendo es guardar texto normal con extensión `.zip`, no un archivo comprimido real. Entonces, ¿tiene sentido usar nano para algo que no acaba en `.txt`?
Tiene sentido cuando:

- Editas archivos de configuración o scripts (`.sh`, `.conf`, `.json`, `.yml`, `.css`, etc.).
- Editas archivos sin extensión o que comienzan con punto (`README`, `.bashrc`, `.gitignore`).

En resumen, archivos de texto plano cuya extensión no es necesariamente txt

**Crea un achivo con nano**

```bash
nano mi_archivo.txt
```

Si el archivo no existe, `nano` lo creará automáticamente.  También puedes poner una ruta absoluta

```bash
nano /home/manolo/mi_archivo.txt
```

Escribe algo sobre el editor, por ejemplo:

```bash
Hola mundo desde nano.
```

Guarda con `Ctrl + O`, pulsa Enter, y sal con `Ctrl + X`.

**Editar un archivo existente**

```bash
nano /etc/hosts
```

Abre el archivo directamente en el editor para modificarlo.  nCuando termines, guarda (`Ctrl + O`) y sal (`Ctrl + X`).

**Buscar texto**

Mientras estás en `nano`, pulsa:

```
Ctrl + W
```

Escribe la palabra o frase que deseas buscar y presiona Enter.

**Reemplazar texto**

Mientras estás en `nano`, pulsa:

```
Ctrl + Altgr + \
```

Escribe el texto a buscar :Enter, luego el nuevo texto :Enter.

Si quiero reemplazar todo, pulso la T a la hora de efectuar el cambio.

**Cortar y pegar texto**

1. Sitúa el cursor en una línea y pulsa `Ctrl + K` → se corta la línea.  
2. Mueve el cursor donde quieras pegar y pulsa `Ctrl + U`.

**Salir de nano**

Mientras estás en `nano`, pulsa:

```
Ctrl + X
```

Si has hecho cambios, nano preguntará:

```
Guardar el archivo modificado (Y/N)?
```

- Pulsa `S` para guardar.
- Pulsa `N` para salir sin guardar.
- Pulsa `Ctrl + C` para cancelar.

## ANEXO V. PRÁCTICA 1: El comando `cat`

##### Ver el contenido de un archivo

```bash
cat prueba123.txt
```

##### Crear un archivo nuevo con cat borrando lo que ya hubiese

```bash
cat > prueba123.txt
```

Escribe el contenido pulsa enter y finaliza con:

```
Ctrl + D
```

##### Añadir contenido al final de un archivo existente

```bash
cat >> prueba123.txt
```

Escribe texto y pulsa `Ctrl + D` para guardar.

`>>` también se puede usar con otros comandos, por ejemplo:

```bash
echo "Hola mundo" >> prueba123.txt #Esto añade un salto de linea por defecto
echo -n "Hola" >> prueba123.txt   # No añade salto
```

```bash
ls -l >> prueba123.txt
```

##### **Crea o sobrescribe** el archivo. Si ya existe, borra lo que tenía.

```bash
echo "hola" > prueba123.txt #borra todo lo que tenia
echo "Mundo" >> prueba123.txt
```

##### Unir varios archivos

```bash
cat prueba212.txt dados > prueba123.txt
```

```bash
cat prueba212.txt dados >> prueba123.txt
```

##### Mostrar líneas numeradas

```bash
cat -n archivo.txt
```



## ANEXO VI. Contraseñas de grupo y algunos comandos "`newgrep`"

### - `newgrep`

<u>El comando `newgrp` en sistemas Unix/Linux se usa para cambiar el grupo primario de tu sesión actual de shell sin tener que cerrar y volver a iniciar sesión</u>, es decir, el comando `newgrp` solo cambia el grupo primario de manera temporal, es decir, solo durante la sesión actual del shell. Cuando un usuario pertenece a varios grupos, `newgrp` permite “entrar” a uno de esos grupos secundarios para que cuando el usuario cree por ejemplo un archivo o directorio, el grupo propietario del archivo o del directorio creado sea el grupo al cual cambió mediante `newgrp`. Pongamos que el usuario `zeke` pertenece a los grupos `marley` (primario) y `paradis`(grupos extra). Si el usuario `zeke` ejecutase:

```bash
newgrp paradis
```

Ahora, su grupo efectivo pasa a ser `proyecto` y tendría efecto sobre los archivos o directorios creados durante la sesión.

### -`/etc/gshadow` y `gpasswd`

Ya vimos en la práctica anterior que `/etc/group` lista los grupos, pero si un grupo tiene contraseña la entrada de `/etc/group` lleva una `x` en el campo de contraseña y la contraseña cifrada se guarda en `/etc/gshadow` (solo legible por root).

**Estructura `/etc/gshadow`:**

```
<groupname>:<passwd>:<administrators>:<members>
```

- `passwd` es el hash de la contraseña de grupo o `!`/`*` si no tiene.

```bash
  sudo gpasswd paradis
```

  comando para añadir contraseña a grupo

- `administrators` son los administradores del grupo (pueden cambiar la contraseña de grupo). Se añaden de la siguiente forma:

```bash
  sudo groupadd paradis
  sudo usermod -aG paradis eren
  sudo usermod -s /bin/bash eren #recuerda darle a eren bash, ya que tenía bin/false en otras prácticas
  sudo usermod -aG paradis armin
  sudo groups eren
  sudo groups armin
  sudo gpasswd -A eren,armin paradis #el comando para añadirlos como administradores de paradis
```

  y puedes verlo así que ya son administradores
```bash
  sudo getent gshadow paradis #cambia a tu usuario con sudo para esto
```

  De hecho, como son admins, puedes cambiar a eren (por ejemplo) y después, sin usar sudo, cambiar la contraseña o incluso añadir gente al grupo

```bash
  su eren
  gpasswd paradis #cambia la contraseña sin sudo
  gpasswd -a hange paradis #añade a hange a paradis
```

- `members` lista de usuarios secundarios (que no son admins)

Normalmente solo puedes usar `newgrp <grupo>` si ya eres miembro de ese grupo sin ningún tipo de requisito adicional. Pero si el grupo tiene una contraseña establecida en `/etc/gshadow`, entonces cualquier usuario del sistema puede "entrar" temporalmente en el grupo sólo con saber la contraseña, es decir, podemos hacernos la siguiente pregunta:

**¿Cuándo se usan las contraseñas de grupo?**

A modo resumen: normalmente, solo puedes usar `newgrp <grupo>` si ya eres miembro de ese grupo. Pero si el grupo tiene una contraseña establecida en `/etc/gshadow`, entonces cualquier usuario del sistema puede "entrar" temporalmente en el grupo con:

```bash
newgrp <nombre_del_grupo>
```



## 7. Ejercicios de continuación Práctica 1

La Práctica 1 ya realizada cubre la creación y gestión básica de usuarios y grupos, edición de caducidad de contraseñas (`/etc/shadow`, `chage`), creación de directorios y gestión con `chown`/`chmod`. Esta práctica profundiza en temas que suelen generar confusión: contraseñas de grupo, `sudoers`, `umask`, herencia de permisos, editores y bits especiales. Esta práctica es larga y complementará a la práctica anterior. Para entender esta parte de la práctica tenemos que entender cómo se crean los permisos por defecto de un directorio o carpeta. En la práctica anterior vimos el comando `mkdir` para crear carpetas y vimos `chown` para cambiar el usuario owner y grupo owner de esta carpeta. Ahora bien, cuando un usuario normal (como `zeke` ) hace:

```bash
mkdir -p /home/zeke/paradysmola
```

¿Con qué usuario y grupo propietarios se crea esa carpeta por defecto?

- **-Usuario propietario (owner):** `zeke`

- **-Grupo propietario (group):** el **grupo primario** de `zeke` (normalmente también llamado `zeke`, a menos que el administrador lo haya cambiado). Si antes de crear la carpeta se hubiera cambiado el grupo principal de `zeke` a `paradys` de la siguiente forma:

```bash
  usermod -g paradys zeke #ojo el grupo paradys debe haber sido creado
```

  -Entonces el propietario sería zeke y el grupo propietario sería paradys.

- Si la anterior sentencia se hubiera ejecutado con sudo:

```bash
  sudo usermod -g paradys zeke #ojo el grupo paradys debe haber sido creado
```

  Tanto grupo propietario como usuario propietario sería root.

En la práctica anterior vimos que:

| Permiso   | Letra | Valor numérico | Significado                                                  |
| --------- | ----- | -------------- | ------------------------------------------------------------ |
| Lectura   | `r`   | 4              | Permite ver el contenido (o listar archivos en un directorio `ls`). |
| Ejecución | `x`   | 1              | Permite acceder al interior (si es directorio).              |
| Escritura | `w`   | 2              | Permite modificar o borrar información del archivo o Permite *crear, borrar o renombrar archivos dentro del directorio*. Necesito previamente permisos de ejecución en el directorio `x` |

Un punto importante que no vimos en la práctica anterior es los permisos para archivos (como archivos txt) 
Lo difícil es que para poder leer, editar o ejecutar un archivo en Linux, no basta con tener permisos sobre el archivo: también necesitas tener permisos adecuados sobre el directorio que lo contiene. Esto se refleja de la siguiente forma:

| Acción que quieres hacer          | Permisos mínimos en el **archivo** | Permisos mínimos en el **directorio** que lo contiene | Explicación                                                  |
| --------------------------------- | ---------------------------------- | ----------------------------------------------------- | ------------------------------------------------------------ |
| **Leer un archivo**               | `r` (lectura)                      | `x` (ejecución)                                       | `x` te deja “entrar” al directorio para acceder al archivo.  |
| **Editar un archivo**             | `w` (escritura)                    | `x` (ejecución)                                       | `x` te deja “entrar” al directorio para acceder al archivo y `w` te deja editarlo |
| **Ejecutar un archivo**           | `x` (ejecución)                    | `x` (ejecución)                                       | Necesitas poder entrar al directorio y ejecutar el archivo.  |
| **Crear un archivo**              | *(no aplica)*                      | `w` + `x`                                             | `w` permite crear, `x` permite entrar al directorio.         |
| **Borrar o renombrar un archivo** | *(no aplica)*                      | `w` + `x`                                             | Lo controla el directorio, no el archivo.                    |
| **Hacer ls sbre un directorio**   | *(no aplica)*                      | `r`                                                   | Solo permisos de lectura sobre dicho directorio              |

#### <u>**Práctica**</u>

Usuarios a crear:

- `eren`
- `mikasa`
- `armin`
- `zeke`

Grupos principales de los usuarios:

- `eren`: `eren`
- `mikasa`: `mikasa`
- `armin`: `armin`
- `zeke`: `marley`

Grupos secundarios de los usuarios:

- `eren`, `mikasa`, `armin` : grupo secundario `paradis`
- `eren`: también grupo sencundario `marley`

**Paso 1: Crear usuarios y grupos**

1. Como root, crea los grupos principales (`eren`, `mikasa`, `armin`, `marley`, `paradis`).
2. Luego crea los usuarios con sus respectivos grupos principales y asignaciones secundarias. Puedes crear el usuario y cambiar el grupo principal después (usermod -g)

**Paso 2: Crear la carpeta `/aot`**

El objetivo es crear un directorio llamado `/aot` en la raíz del sistema.

1. Antes de crearla, pregúntate: ¿Quién puede crear una carpeta en el directorio raíz `/`?
2. Compruébalo con el comando adecuado de (`ls `mirar arriba) para ver sus permisos y propietario.
3. A partir de lo que observes, determina qué comando debes usar para crear la carpeta `/aot` (pista: probablemente necesites usar `sudo`).

**Paso 3: Cambiar propietario, permisos y comprobar accesos**

1. Con sudo, cambia el propietario y grupo de `/aot` a `zeke:marley`.

2. Cambia los permisos a user=todos, group=todos, resto=ejecución y write. Hazlo con la forma octal de los permisos.

3. Comprueba los resultados con con ls directamente sobre el directorio aot:

4. Luego cambia al usuario `mikasa` con:

```bash
   su - mikasa
```

   y realiza estas cuestiones, haciendo capturas de lo que obtienes y justificando dichas capturas:

   - Intenta entrar a `/aot`
   
   - Intenta listar el contenido de `/aot`
   
   - Intenta crear una carpeta dentro de `/aot`
   
   - Con `cat`y `>`, crear un archivo llamado `"la_prueba.txt"` dentro de `aot`que dentro tenga ` "esto es una prueba"`
   
   - con `chown` y sin usar sudo, cambia el grupo propietario de `"la_prueba.txt"` a `paradis.`
   
   - Con `mikasa`, intenta borrar la carpeta `/aot` y observa el mensaje de error ¿Por qué no puedes borrarla?
   
   - Cambia a `armin` . Usa `>>` y `echo` para añadir `"buen trabajo mikasa!"` al final de `"la_prueba.txt"`. Justifica mirando los permisos de `la_prueba.txt` y del directorio padre `aot` porque puede o no puede hacer esto.
   
     para editar un archivo, necesito dos cosas:
   
     - write sobre el archivo "la_prueba.txt"->Lo tengo
     - execute sobre la carpeta del archivo "aot"-> Lo tengo

   - Con `armin` crea dentro de la carpeta `aot` el archivo `"titantes.txt"` con el texto `"menudos titanes"`. Con `sudo` y `chmod` cambia los permisos de este archivo para que otros no puedan nada más que puedan leerlo

**Paso 4: Cambiar a eren**

1. Con su cambia al usuario eren
2. Crea la carpeta `archivos_of_eren` y justifica correctamente por qué eren puede crear esta carpeta dentro de `/aot`
3. ¿Cuál es el usuario principal y grupo principal de esta carpeta creada por defecto? ¿y los permisos por defecto?
4. Dale a esta carpeta los permisos `772` sin utilizar la forma octal de los permisos (mira la práctica anterior)
5. Crea la carpeta `archivos_of_eren_v2` dentro de `aot`y dale el grupo `paradis`.
6. Dale a esta carpeta los permisos `772`
7. Con eren, en la carpeta `aot` realiza un comando para listar todos aquellos archivos terminados en `.txt`
8. Con eren, en la carpeta `aot` realiza un comando para listar exclusivamente ficheros que en realidad, sean directorios.

**Paso 5: La love letter de Mikasa** 

1. Cambia con `su`a Mikasa. 

2. Mikasa quiere escribir una carta de amor a `eren` en esta carpeta. Para ello, intentará crear un archivo en esta carpeta (`archivos_of_eren`) denominado:`love_letter_for_micasa. txt`: ¿podrá? justifica tu respuesta. En caso de no poder, cambia los permisos octales de 772 a lo que corresponda (y sin usar la forma octal de los permisos), es decir, no cambies ni el propietario principal ni grupo principal.

3. Una vez conseguido lo anterior, escribe dentro del archivo: `siempre estaré enamorada de eren`.

4. En caso de que no pueda hacer esto mikasa, ejecuta con sudo para crear la carta de amor. 

5. Crea este archivo con  `nano` y cambia los permisos con `sudo chown` y `sudo chmod` para que: el propietario sea `eren` el grupo principal sea `mikasa` y los permisos sean `770`.

6. Con lo anterior, parece que `mikasa` podría leer su propio archivo. ¿Es así? Justifica tú respuesta.

   **Paso 6: La lista de la compra**

1. root crea otro archivo, llamado "lista_de_equipamiento.txt". Este archivo estrá en `/aot/archivos_of_eren` con usuario principal: `root` y grupo pincipal: `root`. Este archivo tendrá los siguientes permisos 777. En el escribirá `más gaaaaaaaas`
2. root crea otro archivo, llamado "lista_de_equipamiento.txt". Este archivo estará en `/aot/archivos_of_eren_v2` con usuario principal: `root` y grupo pincipal: `root`. Este archivo tendrá los siguientes permisos 777.  En el escribirá `más gaaaaaaaas`

**Paso 7: Armin el cotilla de las letras de amor y las listas de equipamiento** 

1. Cambia con `su` a Armin. 

2. Intenta leer el archivo con el comando `cat` :`"lista_de_equipamiento.txt"` de la ruta; `/aot/archivos_of_eren`  y el `"lista_de_equipamiento.txt"` de `/aot/archivos_of_eren_v2"`.
   Ahora, con nano, intenta cambiar el contenido de ambos archivos de igual forma.

3. ¿Puede armin acceder a leer la carta de amor mi mikasa? Compruébalo.

   

## 8. Contraseñas de grupo y algunos comando `newgrep`

Vamos a poner en práctica lo visto en el Anexo VI

#### **<u>Práctica</u>**.

1. Con el comando correspondiente de ls, mira quien tiene permisos para leer sobre `/etc/gshadow` (mira directamente sobre este archivo), ¿quién tendría permisos para editar el archivo?
3. Añade el grupo y crea contraseña de grupo (ejemplo `escuadrilla_A`) con `gpasswd`:

```bash
sudo gpasswd escuadrilla_A   # te pedirá establecer la contraseña del grupo
```

3. Prueba `newgrp` con  el usuario eren. Una vez logueado correctamente, visualiza el grupo principal de eren.

```bash
# desde el usuario eren (no miembro aún):
newgrp escuadrilla_A
# te pedirá la contraseña del grupo si existe
```

4. Después, crea una nueva carpeta en `home/eren` llamada `familia` y visualiza con ls el propietario y grupo por defecto de la carpeta. Utilizando la forma octal, dale a la carpeta los permisos: rwx-----x
5. Con sudo, añade a `reiner` al grupo escuadrilla_A y mételo como administrador del grupo. Cambia con `su` a reiner y sin usar `sudo` cambia la contraseña de `escuadrilla_A`. Haz que reiner añada al grupo a hange sin usar `sudo`.

6. Vuelve a cambiar a eren y visualiza su grupo principal. Si es necesario, vuelve a hacer `newgrep` para que `escuadrilla_A` vuelva a ser su grupo pirncipal. Haz que eren cree un archivo de texto en esta nueva carpeta llamado `el_sotano_de_papa.txt`. Y visualiza con ls el propietario y grupo por defecto de la carpeta.  Utilizando la forma octal, dale a la carpeta los permisos: rwx---r-x ¿Podrá cualquier otro usuario leer este archivo? Compruébalo con un usuario del grupo propietario del archivo y con un usuario que no sea ni owner ni pertenezca al grupo del archivo

7. Elimina la contraseña del grupo (como root):

```bash
sudo gpasswd -r escuadrilla
```

