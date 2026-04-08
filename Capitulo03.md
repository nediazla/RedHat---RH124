## ### Objetivo del capítulo

Resolver problemas usando la ayuda local del sistema, especialmente:

- `man`
- búsqueda de manuales
- navegación dentro de páginas manual
- diferencia entre secciones
- localizar opciones de comandos sin Internet

---

## ### 1. ¿Qué son las man pages?

Las **manual pages** son la documentación local instalada en Linux para comandos, archivos, funciones y temas del sistema.

Se consultan con:

```
man comando  
```

Ejemplo:

```
man ssh 
``` 

Eso abre la documentación del comando `ssh`.

### Idea clave para RHCSA

Si no recuerdas una opción exacta, muchas veces puedes encontrarla con `man` en lugar de adivinar.

---

## ### 2. Dónde están las páginas manual

El manual indica que las páginas se almacenan bajo:

```
/usr/share/man  
```

No necesitas memorizar la ruta para el examen, pero sí entender que la documentación está **localmente disponible**.

---

## ### 3. Secciones del manual

Un mismo nombre puede existir en varias secciones, así que debes saber diferenciarlas.

### Secciones más importantes

|Sección|Contenido|
|---|---|
|`1`|Comandos de usuario|
|`2`|System calls|
|`3`|Funciones de librería|
|`4`|Archivos especiales|
|`5`|Formatos de archivos y configuración|
|`7`|Convenciones, estándares, miscelánea|
|`8`|Comandos administrativos|

### Las más importantes para RHCSA

- `1` → comandos de usuario
- `5` → formato de archivos
- `8` → comandos administrativos
- `2` → a veces útil en troubleshooting

### Ejemplo clásico

```
man passwd  
```

abre normalmente `passwd(1)` → comando para cambiar contraseña

```
man 5 passwd  
```

abre `passwd(5)` → formato del archivo `/etc/passwd`

### Punto de examen

Debes entender que:

- `passwd(1)` ≠ `passwd(5)`

Mismo nombre, distinto contenido.

---

## ### 4. Estructura típica de una man page

Muchas páginas usan encabezados comunes:

- `NAME`
- `SYNOPSIS`
- `DESCRIPTION`
- `OPTIONS`
- `EXAMPLES`
- `FILES`
- `SEE ALSO`

### Qué significa cada una

- `NAME`: nombre y definición corta
- `SYNOPSIS`: sintaxis del comando
- `DESCRIPTION`: explicación general
- `OPTIONS`: opciones disponibles
- `EXAMPLES`: ejemplos
- `FILES`: archivos relacionados
- `SEE ALSO`: páginas relacionadas

### Lo más útil en RHCSA

Cuando abras una man page, normalmente irás directo a:

- `SYNOPSIS` → para entender sintaxis
- `OPTIONS` → para encontrar el flag que necesitas
- `EXAMPLES` → si existen

---

## ### 5. Navegar dentro de `man`

El manual recalca que saber navegar rápido es una habilidad administrativa clave.

### Teclas importantes

|Tecla|Acción|
|---|---|
|`Space`|bajar una pantalla|
|`PageDown`|bajar una pantalla|
|`PageUp`|subir una pantalla|
|`DownArrow`|bajar una línea|
|`UpArrow`|subir una línea|
|`d`|bajar media pantalla|
|`u`|subir media pantalla|
|`/texto`|buscar hacia adelante|
|`n`|siguiente coincidencia|
|`N`|coincidencia anterior|
|`g`|ir al inicio|
|`G`|ir al final|
|`q`|salir|

### Súper importante para examen

Buscar dentro de una man page:

```
/OPTIONS  
```

o por ejemplo:

```
/interactive  
```

Eso te ahorra mucho tiempo.

---

## ### 6. Buscar manuales por nombre o palabra clave

Este es uno de los puntos más útiles del capítulo.

### `man -f`

Busca por **título exacto** y da una descripción corta. Es equivalente a `whatis`.

```
man -f w  
```

Salida esperada:

```
w (1) - Show who is logged on and what they are doing.  
```

Otro ejemplo:

```
man -f passwd  
```

### `man -k`

Busca por **palabra clave** en títulos y descripciones. Es equivalente a `apropos`.

```
man -k passwd  
```

o

```
man -k boot  
```

Esto te sirve cuando no recuerdas el nombre exacto del comando pero sí el tema.

### Diferencia importante

- `man -f` → búsqueda exacta por nombre
- `man -k` → búsqueda amplia por palabra clave

### Ejemplos del manual

Buscar algo relacionado con ZIP:

```
man -k zip
```  

Buscar parámetros de arranque del kernel:

```
man -k boot  
```

Buscar herramienta para ajustar parámetros de ext4:

```
man -k ext4  
```

Y aparece:

```
tune2fs (8) 
``` 

---

## ### 7. Búsqueda de texto completo: `man -K`

El manual también menciona:

```
man -K palabra  
```

Esto hace búsqueda en el **texto completo** de todas las man pages, no solo en título/descripción.

### Advertencia

- usa más recursos
- tarda más
- no suele ser la primera opción

### Para RHCSA

Úsalo solo si `man -k` no te dio lo que buscabas.

---

## ### 8. Ojo con expresiones regulares

El manual avisa que algunas búsquedas usan patrones tipo regex.

Ejemplo problemático:

```
man -k make $$$ 
``` 

Puede producir resultados inesperados porque caracteres como:

- `$`
- `*`
- `^`

tienen significado especial.

### Qué debes sacar de aquí

Si una búsqueda rara no funciona, prueba con una palabra simple.

---

## ### 9. Ejemplos prácticos importantes del capítulo

El capítulo trae varios ejercicios muy buenos. Estos son los más relevantes.

### Abrir el manual del comando `man`

```
man man  
```

### Abrir el manual del comando `passwd`

```
man passwd  
```
### Abrir el formato del archivo `passwd`

```
man 5 passwd  
```

### Revisar `su`

```
man su  
```

Y entender:

- si no pones usuario, asume `root`
- `su -` abre un login shell

Esto enlaza con el capítulo 2.

---

## ### 10. Ejemplo importante: `vi +2 archivo`

En el ejercicio, el manual de `vi` muestra que puedes abrir un archivo con el cursor en una línea específica:

```
vi +2 manual  
```

Eso abre el archivo `manual` con el cursor en la línea 2.

### Por qué importa

Esto puede ayudarte cuando un log o un error te dice un número de línea y quieres ir directo ahí.

---

## ### 11. Lab del capítulo: habilidades prácticas que quieren que domines

El laboratorio del capítulo está diseñado para que encuentres información usando `man`.

### Tareas clave del lab

#### 1. Encontrar la opción de `hostname` para mostrar todos los FQDN

Buscar en el manual:

```
man hostname  
```

O primero encontrarlo con:

```
man -k hostname  
```

La opción correcta es:

```
hostname -A  
```

Y redirigir salida:

```
hostname -A >> my_task.txt  
```

#### 2. Obtener epoch time de una fecha

Buscar en `man date`:

```
man date 
``` 

La solución encontrada en el manual:

```
date -d "Jan 1 2025" +%s >> my_task.txt 
``` 

Esto usa:

- `-d` para especificar fecha
- `+%s` para mostrar segundos desde Epoch

#### 3. Encontrar el comando que muestra el modo actual de SELinux

Buscar:

```
man -k selinux  
```

Resultado útil:

```
getenforce (8)  
```

Ejecutar:

```
getenforce >> my_task.txt  
```

#### 4. Encontrar cómo imprimir una man page en PostScript

Buscar en:

```
man man  
```

La línea que muestra el manual es:

```
man -t bash | lpr -Pps  
```

Y en el ejercicio la agregan al archivo con:

```
echo "man -t bash | lpr -Pps" >> my_task.txt  
```

### Qué están evaluando realmente

No solo memorizar comandos, sino saber:

- buscar opciones
- interpretar manuales
- redirigir salida
- encontrar comandos correctos sin ayuda externa

---

## ### 12. Lo más importante para RHCSA

Aunque este capítulo parece “teórico”, en realidad es una habilidad práctica clave.

### Debes ser capaz de:

- abrir una man page
- distinguir secciones
- buscar una opción dentro de la página
- usar `man -k` para descubrir comandos
- usar `man -f` para descripción corta
- encontrar el comando correcto aunque no lo recuerdes de memoria

### En RHCSA esto ayuda muchísimo cuando:

- no recuerdas la sintaxis exacta
- olvidas un flag
- no sabes qué comando usar
- necesitas confirmar formato de archivo o comportamiento

---

## ### Errores comunes

### ❌ Error 1: abrir la sección incorrecta

```
man passwd  
```

cuando en realidad necesitas:

```
man 5 passwd  
```

### ❌ Error 2: usar `man -f` cuando no sabes el nombre del comando

Si no sabes el nombre exacto, usa:

```
man -k palabra  
```

### ❌ Error 3: no buscar dentro de la página

Abrir `man` y leer todo desde arriba es lento. Mejor:

```
/keyword  
```

### ❌ Error 4: salir sin aprovechar `SYNOPSIS` y `OPTIONS`

Esas dos secciones suelen darte casi todo lo que necesitas.

---

## ### Comandos clave del capítulo

```
man ssh  
man 5 passwd  
man su  
man vi  
man -f w  
man -k zip  
man -k boot  
man -k ext4  
man -k selinux  
hostname -A  
date -d "Jan 1 2025" +%s  
getenforce  
vi +2 manual  
```

---

## ### Resumen final

En este capítulo aprendiste que:

- `man` es la fuente local principal de ayuda en Linux
- una man page puede existir en distintas secciones
- las secciones más útiles para RHCSA son `1`, `5` y `8`
- puedes navegar y buscar dentro de las páginas manual
- `man -f` muestra descripción corta
- `man -k` ayuda a descubrir comandos por palabra clave
- `man -K` busca en texto completo
- saber usar la ayuda local es una habilidad crítica para administración real y para RHCSA

---

## ### Lo que debes memorizar sí o sí

- `man comando`
- `man 5 archivo`
- `man -f comando`
- `man -k palabra`
- `/texto`, `n`, `N`, `q`
- diferencia entre `passwd(1)` y `passwd(5)`