# Comandos esenciales de Linux con ejemplos

Este documento resume el funcionamiento de algunos de los comandos más importantes del entorno GNU/Linux, con explicaciones claras y ejemplos prácticos.

---

## Apéndice 1º. Patrones globs

Los **globs** son patrones de coincidencia de nombres usados por **Bash** y por los comandos más comunes (los veremos todos a lo largo de la práctica) como:

- `ls`
- `cp`
- `mv`
- `rm`
- `find` (concretamente en su ipción name)

Estos tipos de patrones no te deben sonar como algo nuevo, ya que en la práctica guiada dos los trabjastes con muchos ejemplos del comando `ls`. Los globs usan principalmente 4 símbolos:

| Símbolo            | Significado        |
| ------------------ | ------------------ |
| `*`                | 0 o más caracteres |
| `?`                | 1 carácter         |
| `[ ]`              | conjunto o rango   |
| `!` dentro de `[]` | negación           |

Y estos símbolos se entienden mejor con ejemplos:

---

### Ejemplos globs

Os recomiendo que os miréis los ejemplos de la práctica guiada dos que hicimos con el comando ls, sin embargo, vamos a añadir más ejemplos incluso con comandos que todavía desconocéis los cuales se verán más detalladamente a lo largo de esta práctica.

---

#### Ejemplos con `*`

```
ls *.txt
```

Lista todos los archivos acabados en txt

```bash
ls a*
```

Archivos que empiezan por a:

```bash
ls *a*
```

rchivos que contengan la letra `a` en cualquier parte.

---

#### Ejemplos con `?`

```
ls ?.txt
```

Significa exactamente **un carácter** antes de .txt, el que sea

```bash
ls file??.png
```

Significa exactamente **dos carácteres** antes de .png y después de file, el que sea.

---

#### Ejemplos con `[]`

Sirven para exigir que **un solo carácter** sea uno de los indicados.

```
ls file[123].txt
```

Significa exactamente **un carácter** antes de .png y después de file, que debe ser 1 2 o 3. tambié se pueden poner rangos, como en el siguiente ejemplo:

```bash
ls [a-f]*.?txt
```

Significa exactamente el primer carácter va de la `a` a la `f` (minúsculas) y a partir de ahí, lo que sea hasta terminar en .txt, pero justo después del `.` va un carácter, el que sea.

```bash
ls [aeiouAEIOU]*
```

Significa que los archivos comienzan por cualquier letra dentro del bloque anterior

```bash
ls *[._-]*
```

Significa que en su nombre o extensión, los archivos deben contener un punto, guion o subrayado en medio

---

#### Negación dentro de `[ ]` → `[!x]`

Los bloques [] se pueden negar.

```bash
ls [!a-f]*.txt
```

Significa exactamente el primer carácter no puede ser de la `a` a la `f` (minúsculas) y a partir de ahí, lo que sea hasta terminar en .txt

```bash
ls [!0-9]*.???
```

Significa exactamente el primer carácter no puede ser de la `0` al  `9` y a partir de ahí, lo que sea hasta terminar una extensión con tres caracteres cualesquiera, como pdf o txt...



## Apéndice 2º. Patrones regexp simples

Estos funcionan en comandos como `grep` normal o incluso `awk` pero no con los comandos que mencionamos antes.

---

### El punto `.` “cualquier carácter”

```bash
grep "c.sa" archivo
```

En cada línea del archivo, busca las cadenas donde el segundo carácter puede ser cualquier valor, es decir, el `.` es equivalente a `?` en los patrones globs. Por dar más claridad el ejemplo anterior, si tengo un txt con este contenido:

```makefile
Hola que tal
mi casa es bonita
mi cosa favorita es el youtube
mi cama es grande
mi casa es fea ahora
```

Al ejecutar el comando anterior nos devolverá las líneas que contienen coincidencias, que son:

```makefile
mi casa es bonita
mi cosa favorita es el youtube
mi casa es fea ahora
```

ya que la única letra que puede ser cualquier cosa es el `.` Si no enteindes el comando grep bien, ve al punto siguiente.

---

### El asterisco `*` : “una o más repeticiones del carácter anterior”

```bash
grep "ab+c" archivo
```

En cada línea del archivo, busca las cadenas donde el segundo carácter `b` en este caso puede repetirse tantas veces como se quiera (no 0). Si el archivo tuviera dentro:

```makefile
ac
adbc
abbbc
adc
acd
acccb
abbbbbcxyz
```

nos devolvería las líneas

```makefile
abbbc
abbbbbcxyz
```

Ojo, en ningún momento estamos pidiendo que la línea del archivo acabe en c, por eso el ejemplo `abbbbbcxyz` sigue también el patrón. Si queremos que la línea acabe en `c` tenemos que estudiar el uso de anclas (en dos puntos dentro de este apartado)

---

### El asterisco `*` : “cero o más repeticiones del carácter anterior”

```bash
grep "ab*c" archivo
```

En cada línea del archivo, busca las cadenas donde el segundo carácter `b` en este caso puede repetirse tantas veces como se quiera (incluso 0). Si el archivo tuviera dentro:

```makefile
ac
adbc
abbbc
adc
acd
acccb
abbbbbcxyz
```

nos devolvería las líneas

```makefile
ac
abbbc
abbbbbcxyz
```

Ojo, en ningún momento estamos pidiendo que la línea del archivo acabe en c, por eso el ejemplo `abbbbbcxyz` sigue también el patrón. Si queremos que la línea acabe en `c`, veremos el uso de anclas en el siguiente punto que nos explicará como hacerlo.

----

### Anclas: `^` y `$`

`^` marca el Inicio de línea y `$` el final de línea

```bash
grep "txt$" archivo
```

las íneas pueden empezar en lo que sea, pero deben acabar sí o sí en txt

```bash
grep "^txt"
```

las íneas deben empezar en txt, pero peuden acabar en lo que sea

```bash
grep "^txt$"
```

La línea del archivo debe ser exactamente txt

```bash
grep "^$" archivo
```

Busca líneas vacías

```bash
grep "^ab*c$" archivo
```

La línea debe empezar en `a`, terminar en `c` y la b se puede repetir tantas veces como se quiera (incluso 0)

---

### Clases de caracteres `[ ]`

Sirven para aceptar **uno entre varios caracteres** y se suelen mezclar con `*` y `+`.

```bash
grep "c[ae]sa" archivo
```

las íneas pueden empezar en lo que sea, pero dentro de ellas se buscan secuencias donde primero va la c, después una a o una e y después sa. Por ejemplo, en este archivo:

```makefile
hola casa mia
hola
cesta
-----cesa de preguntar---
-----ceesa de preguntar---
casa casa casa casa
cesa casa
```

el resultado sería:

```makefile
hola casa mia
hola
cesta
-----cesa de preguntar---
-----ceesa de preguntar---
casa casa casa casa
cesa casa
```

```bash
grep "[0-9]" archivo
grep "[A-Z]" archivo
grep "^[!a-z0-9]" archivo
grep "^[0-9]+.*[ba]$" archivo

```

**El primer dice:** cualquier línea que contenta un número

**El segundo dice:** cualquie ínea que contenga una letra mayúscula al final de la misma

**El tercero dice**: cualquier línea que NO comience por una letra minúscula o un número

**El cuarto dice:** cualquier línea que comience por al menos número (uno o muchos). Después puede ir cualquier carácter ya que hay un `.` y ese carácter puede repetirse tantas veces como se quiera (incluso vacío), ya que le sigue un `*`. Por último hay un `[ba]` lo que significa que la línea debe terminar en b o en a.

---



## 1. `grep`

Busca texto dentro de archivos o dentro de la salida de otros comandos. 

### Sintaxis

```
grep [opciones] 'patrón regexp' archivo
```

### Salida:

`grep` devuelve (muestra por pantalla) las líneas completas del archivo que contienen el patrón buscado.

### Ejemplos

**Buscar una palabra en un archivo (busca error)**

```bash
grep "error" /var/log/syslog
```

**Ignorar mayúsculas/minúsculas** de la palabra o regexp puesta

```bash
grep -i "^[0-9]+.*[ba]$" fichero.txt
```

**Mostrar número de línea** (aparte de la linea)

```bash
grep -n "^.root" /etc/passwd
```

**Buscar recursivamente**. Cuando usas `grep -R`, la salida incluye el nombre del archivo, seguido de la línea que contiene la coincidencia, separadas por dos puntos.

```bash
grep -R "^database[.]+" /home/manolo/proyecto/
grep -R "^database\.+" /home/manolo/proyecto/ #esta línea es equivalente a la anterior, busca porqué
```

¿Qué pasa si solo quieres que devuelva el nombre del archivo? Para ello usa el filtro `-l`

Con `-l`:

- No muestra el contenido.
- Muestra únicamente los nombres de los archivos que contienen coincidencias.

```bash
grep -Rl "database" /home/manolo/proyecto/
```

**Filtrar las lieneas de la salida de otro comando usando un `|` **: Lo que hace esto es que en vez de pasar un txt al comando, filtramos la salida de otro comando, como un ls.

```
ls -l | grep nginx
```

**Contar cuántas coincidencias hay se hace con `-c`**

```bash
grep -c "error" archivo.txt
```

---



## 2. `find`

Busca archivos y directorios según criterios. El comando **`find` devuelve (muestra en pantalla)** una **lista de rutas completas** (lína por archivo) de todos los archivos o directorios que cumplen los criterios especificados.

### Sintaxis
```bash
find ruta -name patron globs [criterios] [acciones]
```

No veremos la parte de acciones porque para eso combinaremos con pipes.

### Salida

```makefile
/home/manolo/documentos/notas.txt
```

### Ejemplos

`find -name` puede usar exactamente los mismos patrones (globs) que usamos en `ls` y que también podrán usar otros comandos como: `ls`, `cp`, `mv`, `rm`...

**Buscar por nombre**

```bash
find /home -name "documento_?.txt"
```

---

**Buscar ignorando mayúsculas**

En este caso da igual las mayúsculas y lo que haya por delante y por detrás de baclup

```bash
find . -iname "*backup*"
```

---

**Buscar por tamaño**

```bash
find /var -size +100M
```

---

**Filtar por tipo**

```bash
find ruta -type X
```

La opción **`-type` sirve para filtrar por tipo de archivo**.

| Tipo | Significado                           |
| ---- | ------------------------------------- |
| `f`  | archivo normal (file)                 |
| `d`  | directorio                            |
| `l`  | enlace simbólico (link)               |
| `s`  | socket                                |
| `b`  | bloque (dispositivos tipo `/dev/sda`) |

---

**Buscar archivos modificados en carpeta actual hace menos de 2 días**

```bash
find . -mtime -2
```

---

**Eliminar archivos encontrados (la acción por defecto es print)**

```bash
find . -name "*.log" -delete
```

---

**Ejecutar un comando específico por cada archivo:`-exec comando {} \`**

Ejecuta un comando **por cada archivo** encontrado.

Ejemplo:

```bash
find . -name "*.txt" -exec wc -l {} \;
find . -type f -exec chmod 644 {} \;
```

---



## 3. awk

`awk` es un **lenguaje de procesamiento de texto orientado a columnas**, ideal para:

- Filtrar líneas por patrones (regex)
- Extraer columnas
- Realizar cálculos

Es una versión extendida del comando grep, ya que peude hacer lo que él hace pero mucho más potente.

---

### Sintaxis

Su sintaxis general:

```bash
awk '/patron regexp/ + {accion} ' archivo
```

---

### Salida

`awk` devuelve (muestra por pantalla) las líneas completas del archivo que contienen el patrón buscado

---

### Ejemplos

```bash
grep '^[0-9]' fichero.txt
```

 con grep esto serían las líneas que comienzan por un número. Con awk será IGUAL, excepto que debe ir la expresión regular entre "/"

```bash
awk '/^[0-9]/' fichero.txt
```

---

Una acción importantísima de awk es que puede dividir las líneas por columnas, es decir, si en un fichero tengo:

```makefile
Juan    20  Sevilla
Ana     19  Madrid
Luis    22  Valencia
```

Puedo mostrar la primer columna así:

```bash
awk '{print $1}' alumnos.txt
```

que nos devuelve:

```makefile
Juan
Ana
Luis
```

---

O mostrar nombre + ciudad

```bash
awk '{print $1, $3}' alumnos.txt
```

```makefile
Juan Sevilla
Ana Madrid
Luis Valencia
```

---

O mostrar la última columna

```bash
awk '{print $NF}' alumnos.txt
```

```makefile
Sevilla
Madrid
valencia
```

---

Con la opción `-F` podemos elegir otro separador, en vez de espacio, puede ser `:` , así que el archivo:

```makefile
Juan:20:Sevilla
Ana:19:Madrid
Luis:22:Valencia
```

Podría usarse así:

```bash
awk -F: '{print $1, $NF}' alumnos.txt
```

que nos devuelve

```makefile
Juan Sevilla
Ana Madrid
Luis Valencia
```

---

Podemos cobinarlo con grep para filtar unicamente ciertas ciudades, como las que terminan en `a`

```bash
awk '{print $1}' fichero.txt | grep "a$"
```

```makefile
Sevilla
Valencia
```

----

Podemos filtar una columna, si tenemos el archivo notas.txt

```
Juan;10
Ana;5
Luis;2
```

podemos filtar por aquellas notas mayores que 2

```bash
awk -F";" '$0 > 4' notas.txt
```

que nos devolveria todas las lineas que aprueban

```bash
Juan;10
Ana;5
```

Podemos mezclarlo con grep así:

```bash
awk -F";" '$0 > 4' notas.txt | grep "^A"
```

Para aquellos que aprueben y empiecen por A.

---

Podemos concatenar columnas así del txt anterio así:

```bash
awk -F";" '{print "Alumno " $1 " Nota del alumno " $2}' notas.txt
```

que devuele:

```makefile
Alumno Juan Nota del alumno 10
Alumno Ana Nota del alumno 5
Alumno Luis Nota del alumno 2
```

---

Por último, se podrían sumar columnas. Si tenemos el txt notas:

```makefile
Juan;1;5;
Laura;7;3;
Roberto;4;9;
```

Podemos calcular la nota media así:

```bash
awk -F";" '{print $1 ": media = " ($2 + $3) / 2}' notas.txt
```

---



## 4.`wc`

Sirve para contar **líneas**, **palabras**, **caracteres** y **bytes** de uno o varios archivos, o de la entrada estándar. No tiene mucho sentido usar expresiones regulares para este comando.

### Ejemplos

#### Contar líneas

```
wc -l archivo.txt
```

---

#### Salida:

```
42 archivo.txt
```

---

#### Contar palabras

```
wc -w archivo.txt
```

---

#### Contar caracteres

```
wc -m archivo.txt
```

---

#### Contar todo a la vez

```
wc archivo.txt
```

---

#### Salida típica:

```
42  350  2100 archivo.txt 
```

---



## 5. stdin, stdout y stderr. Relación con los comandos.

| Flujo      | Número | Nombre            | Para qué sirve                                          |
| ---------- | ------ | ----------------- | ------------------------------------------------------- |
| **STDIN**  | **0**  | *standard input*  | Entrada del comando o programa (teclado, pipe, fichero) |
| **STDOUT** | **1**  | *standard output* | Salida normal del programa (texto correcto)             |
| **STDERR** | **2**  | *standard error*  | Salida de errores (avisos, permisos, problemas)         |

Estos **no son archivos reales**, pero Linux los maneja **como si lo fueran**, por eso se pueden redirigir. Por ejemplo, cuando ahces:

```bash
cat archivo.txt
```

`STDOUT` muestra el contenido en pantalla.

Pero si el archivo no existe, recibirás: 

```bash
cat: noexiste.txt: No such file or directory
```

Ese mensaje viene por `STDERR`, no por `STDOUT`.

---

#### Redirigiendo la salida estándar (stdout)

Cuando hacemos:

```bash
ls > lista.txt
```

Estamos enviando la salida estandar a un archivo.

---

#### Redirigiendo errores (stderr)

Cuando hacemos:

```bash
ls 2> errores.txt
```

redirigimos sólo los errores que nos pueda dar .txt. otro ejemplo:

```bash
find / -name "*.qcow2" 2>/dev/null
```

---

#### Redirigir stdout y stderr juntos

```bash
comando > todo.txt 2>&1
```

`2>&1` dice: "redirige también stderr a donde esté stdout"

---

#### Redirigir stdout y stderr por separado

```bash
comando > salidas.txt 2> errores.txt
```

---

#### Comandos que aceptan stdin

Un comando **acepta stdin** cuando puede recibir datos **por la entrada estándar**, es decir:

- desde el teclado
- desde un pipe
- desde una redirección (`< archivo`)

| Comando  | ¿Acepta STDIN? | Ejemplo             |
| -------- | -------------- | ------------------- |
| **grep** | ✓              | `echo hola          |
| **awk**  | ✓              | `echo "a b c"       |
| **sed**  | ✓              | `echo hola          |
| **wc**   | ✓              | `echo hola          |
| **sort** | ✓              | `printf "3\n1\n2\n" |
| **cat**  | ✓              |                     |

Veamos ejemplos de uso:

---

#### Comandos que NO aceptan stdin ¿Cómo se usan con pipes?

Los siguientes comandos requieren **argumentos**, NO leen desde stdin.

- `ls`
- `cp`
- `mv`
- `rm`
- `mkdir`
- `rmdir`

Por ejemplo, no puedo hacer esto:

```bash
find /home/manolo -name "*.txt" | ls -l
```

Para solucionarlo, se usa: `xargs`

```bash
find /home/manolo -name "*.txt" | xargs ls -l
```

Con `rm` se haría así:

```bash
find . -name "*.txt" | rm -f #no vale
```

Para solucionarlo, se usa: `xargs`

```bash
find . -name "*.txt" | xargs rm -f #no vale
```

Con `cp` se haría así:

```bash
find . -name "*.txt" | cp /home/manolo/docs/
```

que no tiene ningún sentido. Para solucionarlo, se usa: `xargs`

```bash
find . -name "*.txt" | xargs -I {} cp {} /home/manolo/docs/
```

`-I {}` le dice a xargs: “Cada entrada que reciba por stdin sustitúyela donde aparezca `{}` en el comando final. Luego convierte cada línea en: 

```makefile
cp linea /home/manolo/docs
```

si la linea vale `ejemplo.txt`, entonces:

```bash
cp ejemplo.txt /home/manolo/docs
```



---

## 5. `cp`

Copia archivos y directorios desde un lugar a otro. Para copiar un archivo **origen** hacia un **destino**, el usuario debe cumplir **dos requisitos mínimos**:

---

##### A) Sobre el archivo *origen*

Necesitas:

- permiso de lectura (r) sobre el archivo
   Para poder leer su contenido y copiarlo.
- permiso de ejecución (x) sobre la carpeta donde se encuentra
   Para poder acceder al archivo dentro del directorio.

---

##### B) Sobre la carpeta de *destino*

Necesitas:

- permiso de escritura (w) en la carpeta destino
   Para poder crear el nuevo archivo copiado.
- permiso de ejecución (x) en la carpeta destino
   Para poder entrar al directorio y modificar su contenido.

---

Además, permite el uso de **patrones de expansión de Bash**, al igual que vimos con ls.

### Ejemplos

Veamos ejemplos de todo tipo, como siempre :)

---

#### **Copiar un archivo dentro de una misma carpeta cambiando el nombre de destino**

```
cp notas.txt copia_notas.txt
```

---

#### **Copiar un archivo a otra carpeta cambiando el nombre de destino**

```
cp notas.txt /home/manolo/Escritorio/copia_notas.txt
```

---

#### **Copiar un directorio dentro de una mismo lugar cambiando el nombre de destino**

```makefile
--/home/manolo
	--proyecto (el original)
	--proyecto_copia (la copia)
```

```bash
cp -r proyecto proyecto_backup
```

---

#### **Copiar un directorio a otro lugar cambiando el nombre de destino**

```makefile
--/home/manolo/proyecto (carpeta original)
--home/manolo/Descargas/proyecto_backup (carpeta de copia)
```

```bash
cp -r /home/manolo/proyecto /home/manolo/Descargas/proyecto_backup
```

---

#### **Copiar un archivo a una carpeta sin renombrar**

```
cp -v archivo1 /home/manolo/Descargas/
```

---

#### **No sobrescribir en caso de que se pueda**

```
cp -n archivo.txt /destino/
```

---

#### **Copiar todos los archivos de una carpeta a otra**

```bash
cp /ruta/origen/* /ruta/destino/ #todos los archivox
cp /ruta/origen/*.txt /ruta/destino/ #solo los txt
```

---

#### **Copiar con expresiones globs**

```bash
cp [a-f]* /home/manolo/rango/ #todos los que empiece por a-f y lo de después da igual

cp !(*.txt) /home/manolo/destino/ #todos menos los txt

cp archivo??.md /home/manolo/docs/ #todos los que son archivo--.md,  
archivo1_.md, archivo1-.md...

cp archivo[_][1-9].md /home/manolo/docs/ #otro formato mas especifico piénsalo

cp *([0-9]).txt /home/manolo/docs/ #0 o mas numeros despues de 

cp *[02468].txt /home/manolo/destino/ #todos los txt terminen en un número par

cp [a-zA-Z]*[0-9] /home/manolo/docs/ #todos los que empiecen por una letra y terminene en un número

cp [0-9w]?a[A-B]*([0-9]).txt /home/manolo/config/
```

El último es un poco más complejo.`[^.]` significa: Cualquier carácter EXCEPTO el punto. Lo último es:

- **Primera letra:** un número 0–9 o la letra w

- **Segunda letra:** cualquier cosa

- **Tercera letra:** a

- **Cuarta letra:** A o B

- **Después:** tantos letras como quieras

- **Termina en:** `txt`

---

#### **Copiar todos los archivos + subcarpetas de una carpeta a otra**

```bash
cp -r /home/manolo/origen/* /home/manolo/destino/
```

Se usa la opción -r

---

#### Ejemplos para aceptar stdin

```bash
find . -name "*.txt" | xargs -I {} cp {} /home/manolo/docs/
```

---



## 6. `mv`

Sirve para mover o renombrar archivos y directorios. Respecto a los permisos, igual que cp.

Además, permite el uso de **patrones de expansión de Bash**, al igual que vimos con ls y cp.

---

### Ejemplos

#### **Mover y renombrar el archivo**

```bash
mv informe.txt informe_final.txt
```

---

#### **Mover archivo a otra carpeta**

```bash
mv foto.png /home/manolo/imagenes/
```

---

#### **Mover varios archivos todos los archivos acabados en .log a otra carpeta**

```bash
mv *.log /var/log/old/
```

---

#### Ejemplos para aceptar stdin

```bash
find . -name "*.log" \ xargs -I {} mv "{}" /var/log/old/
```

---





## 7. `rm`

Elimina archivos y directorios.

Además, permite el uso de **patrones de expansión de Bash**, al igual que vimos con ls, cp, mv y rm

---

### Ejemplos

#### **Eliminar archivo**

```bash
rm documento.txt
```

---

#### **Eliminar todos los archivos terminados en tmp**

```bash
rm *.tmp
```

---

#### **Eliminar directorio completo**

```bash
rm -rf carpeta/
```

---

#### Ejemplos para aceptar stdin

```bash
find . -name "*.log" \
| xargs -I {} rm "{}"
```

---



## 8. `rmdir`

Elimina únicamente directorios vacíos.

### Ejemplos
```bash
rmdir carpeta_vacia
rmdir dir1 dir2 dir3
rmdir -p ruta/una/dos/tres 
```

-Elimina `ruta/una/dos/tres`

-Luego intenta eliminar `ruta/una/dos`

-Luego `ruta/una`

-Y por último `ruta`

**¡Siempre y cuando estén vacíos!**

---



## 9. Comprimir y descomprimir

### A) `tar`

**No comprime** por sí mismo.

**Empaqueta** en un archivo único (`.tar`) manteniendo:

- permisos UNIX completos
- propietarios
- estructura de directorios
- symlinks

Para comprimir se combina con:

- gzip → `.tar.gz`
- xz → `.tar.xz`
- bzip2 → `.tar.bz2` (no lo veremos)

---

#### **Comprimir**

```
tar -czvf backup.tar.gz carpeta/
tar -cJvf backup.tar.xz carpeta/
tar -cvf - carpeta/ | gzip -9 > backup.tar.gz
```

En el último comando, el guion `-` en lugar del nombre de archivo significa: "Saca la salida de TAR por *stdout* (pantalla) en lugar de crear un archivo .tar". `tar` empaqueta la carpeta pero NO crea un archivo `backup.tar` sino que envía el contenido del tar por la tubería (stdout)

---

#### **Descomprimir**

```bash
tar -xzvf backup.tar.gz
tar -xJvf backup.tar.xz
```

---

Vamos a comprender un poco más las opciones usadas en los comandos anteriores:

| Opción | Nombre  | ¿Qué hace?                                                   |
| ------ | ------- | ------------------------------------------------------------ |
| `c`    | create  | Crear un nuevo archivo tar                                   |
| `z`    | gzip    | Comprimir usando gzip o Indica que está en gzip              |
| `v`    | verbose | Mostrar progreso en pantalla                                 |
| `f`    | file    | Permitir especificar el nombre del archivo resultante        |
| x      | extract | Extrae (descomprime + desempaqueta) el contenido del archivo. |
| `j`    | xz      | Comprime usando **XZ** (mucho más eficiente que gzip)        |

Si no pones `-z` ``J`, NO funciona con un `.tar.gz`.

| Formato     | Velocidad  | Compresión             | Uso típico                                    |
| ----------- | ---------- | ---------------------- | --------------------------------------------- |
| gzip (`-z`) | Más rápido | Menor                  | Logs, backups rápidos                         |
| xz (`-J`)   | Más lento  | Mucha mayor compresión | Paquetes de distros, almacenamiento eficiente |

---

### B) `zip` / `unzip`

Empaqueta y comprime a la vez. Cada archivo dentro del ZIP se comprime por separado. Conserva algunos permisos, pero no todos los permisos UNIX avanzados (propietarios, permisos especiales, ACLs…).

`zip` NO usa gzip, ni xz, ni bzip2.
 `zip` utiliza su propio método de compresión, el más común es: Deflate

---

#### **Comprimir**

```bash
zip -r proyecto.zip proyecto/
zip -9 proyecto.zip carpeta/ #lenta
zip -9 proyecto.zip carpeta/ #rapida
```

`-r`: Incluye todos los archivos y subcarpetas que haya dentro de la carpeta.

---

#### **Descomprimir**

```bash
unzip proyecto.zip
```

| Características | ZIP                      | TAR + gzip                                  |
| --------------- | ------------------------ | ------------------------------------------- |
| Algoritmo       | Deflate                  | Gzip usa Deflate pero con formato diferente |
| Permisos UNIX   | Parciales                | Completo                                    |
| Compatibilidad  | Muy alta (Windows/macOS) | Menos universal                             |

---

#### Ejemplos para aceptar stdin

```bash
find / -type f -name "*.zip" \
| xargs -I {} unzip "{}" -d /home/manolo/descomprimidos/

```

`-d` significa:

> Extrae el contenido del .zip dentro de este directorio.

---

### C) `gzip` / `gunzip`

#### **Casos de uso**

Comprimir archivos sueltos.   Por defecto, `gzip` borra el archivo original después de comprimirlo. Para evitar que lo borre, usa:  Usa `-c` (stdout) y redirección:

```bash
gzip -c archivo.log > archivo.log.gz
gzip -1 archivo.log        # compresión rápida
gzip -9 archivo.log        # máxima compresión
```

---

#### **Comprimir:**

```bash
gzip archivo.log
```

Lo transforma en:  gzip archivo.log.gz

---

#### **Descomprimir**

```
gunzip archivo.log.gz
```

 o devolviendo la salida a otroa para renombar incluso:

```bash
gunzip -c archivo.log.gz > archivo.log
```

---

#### Ejemplos para aceptar stdin

Acepta entrada estandar, por ejemplo, acepta lo que le devuelva un cat:

```bash
cat archivo.log | gzip > archivo.log.gz
```

---







## 9.df (disk free)

### **Sintasis**

`df` significa **disk free**. Sirve para **ver el espacio disponible y ocupado** en cada sistema de archivos montado en Linux.`df` **no mide archivos o carpetas**. Mide **sistemas de archivos completos** (ext4, xfs, tmpfs, etc.) que están montados en puntos como `/`, `/home`, `/boot`, etc. Es decir: `df` te dice **cuánto espacio queda en la partición**, no cuánto ocupa una carpeta concreta.

```bash
df [opciones]
df -h
```

---

### Ejemplos

```bash
df -h
```

`-h` = *human readable*, muestra valores en KB, MB, GB en lugar de bloques. Salida:

`-H`= Usa múltiplos **SI** en lugar de **IEC**:

​	a)`-h`: 1G = 1024 MB

​        b)`-H`: 1G = 1000 MB

`T`= Muestra el tipo de sistema de archivos (ext4, xfs, tmpfs, squashfs…).

```makefile
df -h
Filesystem      Size  Used Avail Use% Mounted on
/dev/sda2        98G   65G   29G  70% /
tmpfs           1.6G  2.0M  1.6G   1% /run
/dev/sda1       512M  120M  392M  24% /boot
```

---

### Cómo interpretar la salida de df

- **Filesystem** → dispositivo o sistema de archivos
- **Size** → tamaño total del sistema de archivos
- **Used** → espacio usado
- **Avail** → espacio libre
- **Use%** → porcentaje usado
- **Mounted on** → carpeta donde está montado dicha partición

---



## 10.du (disk usage)

### **Sintasis**

`du` significa **disk usage**. A diferencia de `df`, `du` **sí mide cuántos bytes ocupa una <u>carpeta</u> o archivo específico**. `du` recorre el sistema de archivos **archivo por archivo**, sumando tamaños.

```bash
du [opciones] ruta
```

---

### Ejemplos

```bash
du -sh home/manolo
```

`-s` → *summary* (solo total y no de cada una de las subcarpetas de /home/manolo

`-h` → *human readable*, transforma el tamaño en números legibles

Si no queremos que mire cada subcarpeta, se le puede poner un: --max-depth. Por ejemplo, si estamos en:

`home/manolo` y me interesa que sólo mire las carpetas hijas como:

- `/home/manolo/Descargas`

- `/home/manolo/Videos`

- `/home/manolo/Imagenes`

Tenemos que poner: --max-depth=1.

```bash
 sudo du -h --max-depth=1 /var | sort -h
```

En el siguiente punto veremos el comando sort.

---

### Aclarando max-depth

--max-depth=N. indica hasta qué nivel de subdirectorios quieres que du examine y muestre. Es decir:

    Nivel 0 → solo el directorio indicado
    
    Nivel 1 → el directorio + sus hijos directos
    
    Nivel 2 → el directorio + sus hijos + los hijos de sus hijos
    
    Nivel 3 → y así sucesivamente

---

### Recordatorio ls vs du

Puedes pensar que lo anterior con max-depth=1 también se puede hacer con un ls, es decir:

```bash
ls -lSh carpeta
```

donde este ls ordena todos los archivos de una carpeta por tamaño de mayor a menor (la h es para que el tamaño sea leible y no en bytes). Sin embargo, ls tiene un gran problema y es que no muestra el tamaño de las carpetas bien, solo muestra el tamaño de los binarios como (txt's, ISO's, qcow's...). Esto se ve bien así:

Con ls, si queremos mirar el tamaño de la carpeta (home/manolo) me saldría algo así:

![image-20251208141905611](/home/manolo/.config/Typora/typora-user-images/image-20251208141905611.png)

Sin embargo, con du me saldría bien:

![image-20251208142015685](/home/manolo/.config/Typora/typora-user-images/image-20251208142015685.png)

Por tanto, recomiendo usar ls para mirar el tamaño de los ficheros!=carpetas de un directorio, tal que asÍ:

```bash
ls -lShp carpeta | grep ""
```

en donde, con p, hemos hecho que ls añada "/" al final de un directorio:

```makefile
Documentos/
foto.jpg
scripts/
notas.txt
```

### Ejemplos para aceptar stdin

No acepta entrada estandar, sólo argumentos. Un ejemplo con find

```bash
find . -maxdepth 1 -type d\
| xargs du -sh \
```



---



## 11.sort

### **Sintasis**

Como es evidente, sirve para ordenar líneas. **Sort interpreta la primera \*cadena numérica\* que encuentra en cada línea** y la usa como clave para ordenar. En su opción más común,`sort -h` ordena líneas de conteniendo con tamaños *human-readable* como:

- 4.0K
- 200M
- 1.3G

---

### Ejemplos

```makefile
150M /var/cache
4.0K /var/tmp
900M /var/log
200M /var/cache/apt
1.3G /var
```

Salida:

```makefile
4.0K /var/tmp
150M /var/cache
200M /var/cache/apt
900M /var/log
1.3G /var
```

Vamos a ver como ordenar también por tamaño con sort. 

```bash
sort -n lista.txt
#ordena de menor a mayor numéricamente las lineas con números
```

con `-r` tenemos el orden inverso:

```bash
sort -nr lista.txt
```

con `-k` ordenamos por una columna específica:

```bash
sort -k2 lista.txt
```

con `-u` quitamos duplicados después de ordenar

```bash
sort -u archivo.txt
```

---

### Ejemplos para aceptar stdin

```bash
du -h --max-depth=1 /home/manolo | sort -hru
```









# **EJERCICIO 1 — Encontrar carpetas de /home/manolo cuyo tamaño supere 200 MB**

*Objetivo:* combinar `du`, `sort`, `find`, filtros y pipes.

### **Enunciado**

Usando **únicamente comandos de la práctica**, localiza todas las carpetas dentro de `/home/manolo` cuyo tamaño sea **superior a 200 MB**.
 Debes mostrar solo el **nombre de la carpeta y su tamaño** ordenados de mayor a menor.

### **Solución esperada (una posible)**

```bash
du -h --max-depth=1 /home/manolo \
    | grep -E "[0-9]+M|[0-9]+G" \
    | sort -hr \
    | awk '$1+0 > 200 {print}'

```

