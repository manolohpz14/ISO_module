# 4ª Practica: Migración de un disco de 120 GB (Más grande) a otro de 100 GB (Más chico) usando GParted y el comando dd

Objetivo: Aprender a migrar un sistema instalado en un disco de mayor tamaño (160 GB) hacia otro más pequeño (120 GB), utilizando redimensionado y copia de particiones con GParted. La práctica se realizará de forma segura en un entorno virtual, pero se diseñará para que el alumno comprenda cómo aplicar el mismo procedimiento en un ordenador real con SSDs físicos.

Video a seguir: https://drive.google.com/file/d/1WCdgbt8Ft16aUmUvdIfQaFHdDkuHqeV8/view?usp=sharing (volumen subido)

### 1 En el laboratorio virtual (simulación)

Se usarán dos discos virtuales en Virt-Manager:
- Uno de **160 GB** con un sistema ya instalado (origen). utilizad el disco de la practica anterior
- Uno de **120 GB** vacío (destino). Creadlo desde virt-maanger, debe ser un disco vacío.

Se creará una **máquina intermedia** que arranca con un **Live CD de Ubuntu (es decir, ISO de Ubuntu)** y que monta ambos discos a la vez, para redimensionar y clonar las particiones.

---

### 2 En la vida real (aplicación práctica)

En lugar de discos virtuales, se trabaja con **dos SSDs físicos** conectados al mismo equipo:

- El SSD de **160 GB** con el sistema a migrar.
- El SSD de **120 GB** vacío como destino.

Para no interferir con el sistema en uso, se arranca el ordenador desde un **Live USB/CD con GParted** o desde una instalación de Linux en un **tercer disco distinto** con LINUX instalado. Desde ahí se ejecutan las mismas operaciones que en la simulación: redimensionar, copiar particiones y reinstalar GRUB en el nuevo disco.

---

### 3 Pasos principales de la práctica

**1.**Crear en Virt-Manager una nueva VM que arranque con la ISO de Ubuntu y monte los dos discos virtuales, el vacío y el de la práctica anterior.

---

**2**.Iniciar la VM con Ubuntu Live y redimensiona las particiones del disco de 120 GB hasta que quepan en 100 GB.

---

**3.**Copiar las particiones al disco de 100 GB usando el comando dd.

---

**4.**Es posible que al terminar de copiar, no puedas ver el disco copiado en Gparted **(a mi no me pasa en el primer video pero es una posibilidad que os puede ocurrir, es decir).** Los siguientes pasos en la práctica se pueden seguir desde las capturas de pantalla o desde el siguiente video. Es tán fácil que desde las capturas os debería salir solo: https://drive.google.com/file/d/1cXXj6BIagIzb9j_hg1KGPxTrxv4UCEh5/view?usp=sharing

Cuando vais a abrir el disco copiado desde g-parted, os sale esto:

![image-20251130130112127](../../img/image-20251014160751656.png)

La idea es que GPT no ha podido copiar el "backup" que guardan las particiones GPT al final del disco (pensad por qué sucede esto). Para arreglarlo, deberéis ejecutar la siguiente secuencia de comandos:

```bash
sudo gdisk /dev/nvme0
```

 (aquí hareis referencia al disco en donde estais copiando la información)

![image-20251130130112127](../../img/image-20251014161056830.png)

Desupués pulsais la opción `e`  que consiste en: *"relocate backup strctures at the end of the disk"*. ¿Por qué debéis hacer esto? Contesta de forma que se entienda y sea explicativo.

![image-20251130130112127](../../img/image-20251014161339232.png)

Por último, una vez habéis reubicado el backup al final del disco, le dais a la opción `w`  y después ` Y (de Yes) `que escribirá los cambios establecidos mediante los pasos anteriores.

![image-20251130130112127](../../img/image-20251014161512856.png)

Si todo ha salido correctamente, podréis ver el disco en G-Parted.

---

**5.**Por ultimo, se pide verificar que el sistema inicia correctamente y que el espacio se ha adaptado al nuevo tamaño. Verifica con capturas como queda el disco final.

