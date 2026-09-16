# Registro de Terminal: Comandos Copiar y Borrar

A continuación se documenta el flujo de los comandos ejecutados en la terminal de Kubuntu durante la práctica, mostrando la creación de carpetas, archivos, copiado y eliminación de elementos.

---

## 📁 1. Creación de carpetas principales (`mkdir`)

> **Objetivo:** Crear las carpetas `practica1` y `practica2` dentro de `Documentos`.

```bash
victoro@Vicpapas-HP:~$ cd Documentos
victoro@Vicpapas-HP:~/Documentos$ mkdir practica1
victoro@Vicpapas-HP:~/Documentos$ mkdir practica2
victoro@Vicpapas-HP:~/Documentos$ sudo ls
practica1  practica2
victoro@Vicpapas-HP:~/Documentos$
```

---

## 📝 2. Creación del archivo de texto (`gedit`)

> **Objetivo:** Crear un archivo `Readme.txt` dentro de `practica1`.

```bash
victoro@Vicpapas-HP:~/Documentos$ cd practica1
victoro@Vicpapas-HP:~/Documentos/practica1$ sudo gedit Readme.txt
```

Al ejecutar `gedit` aparecieron algunas advertencias relacionadas con `Peas` y `PeasGtk`. A pesar de las advertencias, el archivo se creó correctamente.

```bash
victoro@Vicpapas-HP:~/Documentos/practica1$ ls -l
total 4
-rw-r--r-- 1 root root 33 Sep 16 10:25 Readme.txt
```

---

## 📁 3. Creación de subdirectorios (`mkdir`)

> **Objetivo:** Crear las carpetas `vacia` e `info` dentro de `practica2`.

```bash
victoro@Vicpapas-HP:~/Documentos/practica1$ cd ..
victoro@Vicpapas-HP:~/Documentos$ cd practica2
victoro@Vicpapas-HP:~/Documentos/practica2$ mkdir vacia
victoro@Vicpapas-HP:~/Documentos/practica2$ mkdir info
victoro@Vicpapas-HP:~/Documentos/practica2$ cd info
```

---

## 📝 4. Creación de archivo dentro de `info` (`gedit`)

> **Objetivo:** Crear otro archivo `Readme.txt` dentro de la carpeta `info`.

```bash
victoro@Vicpapas-HP:~/Documentos/practica2/info$ sudo gedit Readme.txt
```

Nuevamente aparecieron advertencias de `gedit`, pero el archivo fue creado correctamente.

```bash
victoro@Vicpapas-HP:~/Documentos/practica2/info$ cd ..
victoro@Vicpapas-HP:~/Documentos/practica2$ cd info
victoro@Vicpapas-HP:~/Documentos/practica2/info$ sudo ls
Readme.txt
```

---

## 📂 5. Copiado recursivo de carpetas (`cp -r`)

> **Objetivo:** Copiar la carpeta `vacia` desde `practica2` hacia `practica1`.

Al principio se intentó copiar utilizando `Vacia` con mayúscula, pero Linux distingue entre mayúsculas y minúsculas, por lo que apareció un error:

```bash
victoro@Vicpapas-HP:~$ cp -r ~/Documentos/practica2/Vacia ~/Documentos/practica1/
cp: no se puede efectuar `stat' sobre '/home/victoro/Documentos/practica2/Vacia': No existe el archivo o el directorio
```

Después se utilizó correctamente el nombre `vacia` en minúsculas:

```bash
victoro@Vicpapas-HP:~$ cp -r ~/Documentos/practica2/vacia ~/Documentos/practica1/
```

El comando se ejecutó correctamente.

También se copió la carpeta `info`:

```bash
victoro@Vicpapas-HP:~$ cp -r ~/Documentos/practica2/info ~/Documentos/practica1/
```

---

## 🔍 6. Comprobación de las carpetas copiadas (`ls`)

> **Objetivo:** Verificar que las carpetas se copiaron correctamente a `practica1`.

```bash
victoro@Vicpapas-HP:~$ ls Documentos
practica1  practica2

victoro@Vicpapas-HP:~$ cd Documentos
victoro@Vicpapas-HP:~/Documentos$ cd practica1
victoro@Vicpapas-HP:~/Documentos/practica1$ sudo ls
Readme.txt  info  vacia

victoro@Vicpapas-HP:~/Documentos/practica1$ cd info
victoro@Vicpapas-HP:~/Documentos/practica1/info$ sudo ls
Readme.txt
```

Esto permitió comprobar que las carpetas `info` y `vacia` fueron copiadas correctamente.

---

## 🗑️ 7. Eliminación del archivo `Readme.txt` (`rm`)

> **Objetivo:** Eliminar el archivo `Readme.txt` que se encontraba dentro de `practica1`.

Primero se intentó eliminar el archivo desde `Documentos`, pero ahí no existía directamente:

```bash
victoro@Vicpapas-HP:~/Documentos$ rm Readme.txt
rm: no se puede borrar 'Readme.txt': No existe el archivo o el directorio
```

Después se ingresó correctamente a `practica1`:

```bash
victoro@Vicpapas-HP:~/Documentos$ cd practica1
victoro@Vicpapas-HP:~/Documentos/practica1$ rm Readme.txt
rm: ¿borrar el regular file 'Readme.txt' protegido contra escritura? (s/n) s
```

El archivo fue eliminado correctamente.

---

## 🗑️ 8. Eliminación de la carpeta `info` (`rm -r`)

> **Objetivo:** Eliminar la carpeta `info` y su contenido de `practica1`.

```bash
victoro@Vicpapas-HP:~/Documentos/practica1$ rm -r info
victoro@Vicpapas-HP:~/Documentos/practica1$
```

La opción `-r` permite eliminar una carpeta junto con su contenido.

---

## 📌 Conclusión

En esta práctica se aprendió a utilizar comandos básicos de Linux para crear, copiar, comprobar y eliminar archivos y carpetas. Se utilizaron principalmente `mkdir`, `cd`, `ls`, `gedit`, `cp -r`, `rm` y `rm -r`. También se comprobó que Linux diferencia entre mayúsculas y minúsculas al escribir los nombres de archivos y directorios.
