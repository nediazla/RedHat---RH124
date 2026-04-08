# Capítulo 2 – Accessing the Command Line (Acceso a la línea de comandos)

> **Objetivo del capítulo**
> 
> - Iniciar sesión en sistemas RHEL
> - Usar la terminal
> - Ejecutar comandos básicos
> - Entender usuarios, root y privilegios
> - Salir correctamente del sistema

Este capítulo es **fundamental para RHCSA** porque TODO el examen es práctico y basado en línea de comandos.

---

# 1️. Acceso al sistema

Puedes acceder a RHEL de varias formas:

### ✅ 1. Consola local (física o VM)

- Te aparece el login prompt
- Ingresas usuario y contraseña

### ✅ 2. SSH (remoto) — MUY IMPORTANTE

```
ssh usuario@servidor  
```

Ejemplo:

```
ssh student@servera 
``` 

Si es la primera vez:

- Te preguntará si confías en el host
- Se guarda la clave en `~/.ssh/known_hosts`

📌 En RHCSA usarás mucho SSH para conectarte a máquinas remotas.

---

# 2️. La Terminal y el Shell

Cuando inicias sesión, estás dentro de un **shell**.

El shell por defecto en RHEL es:

```
bash
```  

---

##  ¿Qué es el shell?

Es un intérprete de comandos que:

- Recibe comandos
- Los envía al kernel
- Devuelve resultados

---

## El Prompt

Ejemplo:

bash

Copy

student@workstation:~$  

Significa:

- `student` → usuario actual
- `workstation` → hostname
- `~` → directorio actual (home)
- `$` → usuario normal
- `#` → usuario root

📌 En examen debes saber identificar si eres root o usuario normal.

---

# 3️. Ejecutar comandos

Formato general:

```
comando opciones argumentos  
```

Ejemplo:

```
ls -l /home
```  

- `ls` → comando
- `-l` → opción
- `/home` → argumento

---

# 4️. Cambiar de usuario (su)

Comando:

```
su  
```

Por defecto cambia a root.

```
su -  
```

🔴 MUY IMPORTANTE:

- `su` → shell no-login (hereda entorno actual)
- `su -` → login shell completo (entorno limpio)

📌 En examen, si cambias a root usa `su -`

---

# 5️. Manuales (man)

Este capítulo introduce cómo obtener ayuda.

---

## Abrir página manual

```
man comando  
```

Ejemplo:

```
man passwd  
```

---

## Secciones importantes

|Sección|Contenido|
|---|---|
|1|Comandos de usuario|
|5|Formatos de archivos|
|8|Comandos administrativos|
|2|Llamadas del sistema|

Ejemplo:

```
man 5 passwd  
```

Abre el formato del archivo `/etc/passwd`.

📌 Esto aparece mucho en el examen.

---

## Buscar en manuales

```
man -k palabra
```  

Ejemplo:

```
man -k boot  
```

Busca manuales relacionados con "boot".

---

## Descripción corta

```
man -f comando  
```

Ejemplo:

```
man -f w  
```

---

# 6️. Redirección (Muy importante)

Para enviar salida a archivo:

```
comando >> archivo  
```

Ejemplo:

```
hostname -A >> my_task.txt
```  

`>>` → agrega al final  
`>` → sobrescribe

📌 En examen práctico esto aparece mucho.

---

# 7️. Uso del comando date (Epoch)

Tiempo desde 1 Enero 1970:

```
date +%s  
```
Para fecha específica:

```
date -d "Jan 1 2025" +%s 
``` 

Convertir segundos a fecha:

```
date --date='@1735689600' 
``` 

---

# 8️. SELinux Mode

Para verificar el modo actual:

```
getenforce  
```

Posibles resultados:

- Enforcing
- Permissive
- Disabled

📌 SELinux es crítico en RHCSA.

---

# 9️. Editores – vi

Abrir archivo:

```
vi archivo  
```

Abrir en línea específica:

```
vi +2 archivo  
```

📌 Esto es útil cuando logs indican número de línea.

---

# 10. Salir del sistema

Salir del shell:

```
exit
```  

Cerrar sesión SSH:

```
exit 
``` 

Salir de man:

```
q  
```

---

#  Conceptos CLAVE para RHCSA

✅ Saber usar `man` sin ayuda externa  
✅ Entender diferencias entre secciones del manual  
✅ Usar redirecciones  
✅ Usar `su -` correctamente  
✅ Usar SSH  
✅ Leer errores y buscar ayuda  
✅ Entender login shell vs non-login shell

---

# 🧠 Errores comunes en examen

❌ Usar `su` sin `-`  
❌ No redirigir correctamente salida  
❌ No saber buscar opciones en man  
❌ No revisar sección correcta del manual