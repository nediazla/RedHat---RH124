# Capítulos 6-10: File System, Archivos, Redirecciones, Usuarios y Grupos

## CAPÍTULO 6: Jerarquía del Sistema de Archivos

### Jerarquía de Archivos Linux
- **Estructura de árbol invertido** - Raíz (/) en la cima
- **Case-sensitive** - `archivo.txt` ≠ `Archivo.txt`
- **Separador**: `/` (barra diagonal)

### Directorios Principales

| Directorio | Propósito | Tipo |
|------------|-----------|------|
| **/** | Raíz del sistema | - |
| **/boot** | Archivos para arranque del sistema | Estático |
| **/dev** | Archivos especiales de dispositivos (hardware) | Dinámico |
| **/etc** | Configuraciones del sistema | Estático/Persistente |
| **/home** | Directorios home de usuarios regulares | Dinámico/Persistente |
| **/root** | Home del superusuario root | Dinámico/Persistente |
| **/run** | Datos de runtime desde último boot | Dinámico/No persistente |
| **/tmp** | Temporales (limpieza automática 10 días) | Dinámico/No persistente |
| **/usr** | Software instalado, bibliotecas compartidas | Estático |
| **/var** | Datos variables: logs, bases de datos, caches | Dinámico/Persistente |

### Subdirectorios de /usr

```bash
/usr/bin        # Comandos de usuario
/usr/sbin       # Comandos de administración del sistema
/usr/local      # Software personalizado localmente
/usr/lib        # Bibliotecas de 32-bit
/usr/lib64      # Bibliotecas de 64-bit
```

### ⚠️ Enlaces Simbólicos en RHEL 7+

```bash
# Estos son enlaces simbólicos a /usr/*
/bin   → /usr/bin
/sbin  → /usr/sbin
/lib   → /usr/lib
/lib64 → /usr/lib64
```

### Paths Absolutos vs Relativos

**Path Absoluto**: Comienza con `/`
```bash
/var/log/messages
/etc/hostname
/home/user/document.txt
```

**Path Relativo**: NO comienza con `/`
```bash
# Desde /var:
log/messages        # = /var/log/messages

# Desde /home/user:
Documents/file.txt  # = /home/user/Documents/file.txt
```

### Comandos de Navegación

```bash
# Ver directorio actual
pwd

# Cambiar directorio
cd /etc              # Path absoluto
cd Documents         # Path relativo
cd                   # Ir a home
cd ~                 # Ir a home
cd -                 # Directorio anterior
cd ..                # Directorio padre
cd ../..             # Dos niveles arriba

# Listar contenido
ls                   # Listar directorio actual
ls -l                # Formato largo (permisos, propietario, etc)
ls -a                # Incluir archivos ocultos (.)
ls -la               # Combinar opciones
ls -R                # Recursivo (subdirectorios)
ls -lh               # Tamaños human-readable
```

### Caracteres Especiales

```bash
.      # Directorio actual
..     # Directorio padre
~      # Home directory del usuario actual
~user  # Home directory de "user"
-      # Directorio previo (con cd)
```

### Archivos Ocultos
- **Comienzan con punto** (`.`)
- **No se muestran con `ls`** (usar `ls -a`)
- **Ejemplos**: `.bashrc`, `.bash_history`, `.config/`

### Ejemplos Prácticos

```bash
# Navegación básica
pwd                          # /home/user
cd /etc
pwd                          # /etc
cd                          # Volver a home
pwd                          # /home/user

# Uso de ..
cd /var/log/httpd
pwd                          # /var/log/httpd
cd ..                        # A /var/log
cd ../..                     # A /var
cd ~                         # A home

# Alternar entre directorios
cd /etc
cd /var/log
cd -                         # Volver a /etc
cd -                         # Volver a /var/log

# Listar con opciones
ls -l /etc                   # Lista larga
ls -la ~                     # Incluye ocultos en home
ls -lh /var                  # Tamaños legibles
```

---

## CAPÍTULO 7: Gestión de Archivos desde Línea de Comandos

### Comandos Básicos de Gestión

```bash
# Crear directorio
mkdir directorio
mkdir -p /path/completo/anidado    # Crear padres si no existen

# Copiar
cp origen destino                   # Copiar archivo
cp -r dir1 dir2                    # Copiar directorio recursivamente
cp -a origen destino               # Preservar atributos
cp -i origen destino               # Interactivo (confirmar sobrescritura)

# Mover/Renombrar
mv origen destino                   # Mover o renombrar
mv -i origen destino               # Interactivo

# Eliminar
rm archivo                          # Eliminar archivo
rm -r directorio                   # Eliminar directorio recursivamente
rm -i archivo                      # Interactivo
rm -f archivo                      # Forzar (sin confirmación)
rm -rf directorio                  # ⚠️ PELIGROSO - Elimina todo recursivamente

# Crear archivo vacío
touch archivo.txt

# Eliminar directorio vacío
rmdir directorio
```

### Links (Enlaces)

#### Hard Links
```bash
ln archivo_original hardlink_nombre

# Características:
# - Mismo inode que original
# - Cuenta de referencias incrementa
# - Si se borra original, datos persisten
# - NO funcionan entre file systems
# - NO funcionan con directorios
```

#### Symbolic Links (Soft Links)
```bash
ln -s archivo_original symlink_nombre

# Características:
# - Inode diferente
# - Apunta al path del original
# - Si se borra original, link queda "roto"
# - Funcionan entre file systems
# - Funcionan con directorios
```

### Wildcards (Expansiones)

| Wildcard | Coincide con | Ejemplo |
|----------|--------------|---------|
| `*` | Cero o más caracteres | `*.txt`, `file*` |
| `?` | Exactamente un carácter | `file?.txt` |
| `[abc]` | Cualquier carácter del conjunto | `file[123].txt` |
| `[!abc]` | Cualquier carácter NO en el conjunto | `file[!0-9].txt` |
| `[a-z]` | Rango de caracteres | `[A-Z]*` |

```bash
# Ejemplos prácticos
ls *.txt                    # Todos los .txt
ls file?.txt                # file1.txt, fileA.txt, NO file12.txt
ls file[1-3].txt           # file1.txt, file2.txt, file3.txt
ls [A-Z]*                  # Archivos que empiezan con mayúscula
rm test[!0-9].txt          # Elimina testA.txt, NO test1.txt
```

---

## CAPÍTULO 8: Editando Archivos de Texto

### Vim - Editor de Texto

#### Modos de Vim

1. **Modo Comando** (default al abrir)
2. **Modo Inserción** (editar texto)
3. **Modo Visual** (seleccionar texto)

#### Comandos Básicos

```bash
# Abrir archivo
vim archivo.txt
vi archivo.txt

# Abrir en línea específica
vim +25 archivo.txt        # Abre en línea 25
vim +$ archivo.txt         # Abre al final
```

#### Modo Comando → Inserción

| Tecla | Acción |
|-------|--------|
| `i` | Insert antes del cursor |
| `a` | Append después del cursor |
| `I` | Insert al inicio de línea |
| `A` | Append al final de línea |
| `o` | Nueva línea abajo |
| `O` | Nueva línea arriba |
| `Esc` | Volver a modo comando |

#### Movimiento en Modo Comando

| Tecla | Movimiento |
|-------|------------|
| `h` | Izquierda |
| `j` | Abajo |
| `k` | Arriba |
| `l` | Derecha |
| `w` | Palabra siguiente |
| `b` | Palabra anterior |
| `0` | Inicio de línea |
| `$` | Fin de línea |
| `gg` | Inicio del archivo |
| `G` | Fin del archivo |
| `:n` | Ir a línea n |

#### Edición en Modo Comando

```bash
x       # Eliminar carácter bajo cursor
dd      # Eliminar línea completa
dw      # Eliminar palabra
d$      # Eliminar hasta fin de línea
yy      # Copiar (yank) línea
p       # Pegar después del cursor
P       # Pegar antes del cursor
u       # Deshacer (undo)
Ctrl+r  # Rehacer (redo)
.       # Repetir último comando
```

#### Guardar y Salir

```bash
:w              # Guardar
:w archivo      # Guardar como
:q              # Salir (si no hay cambios)
:q!             # Salir sin guardar
:wq             # Guardar y salir
:x              # Guardar y salir (solo si hay cambios)
ZZ              # Guardar y salir (desde modo comando)
```

#### Búsqueda y Reemplazo

```bash
/texto          # Buscar "texto" hacia adelante
?texto          # Buscar hacia atrás
n               # Siguiente resultado
N               # Resultado anterior

# Buscar y reemplazar
:s/old/new/         # Reemplazar primera ocurrencia en línea
:s/old/new/g        # Reemplazar todas en línea
:%s/old/new/g       # Reemplazar en todo el archivo
:%s/old/new/gc      # Con confirmación
```

---

## CAPÍTULO 9: Redirección de Entrada/Salida

### Descriptores de Archivo Estándar

| Número | Canal | Descripción | Redireccionamiento |
|--------|-------|-------------|--------------------|
| 0 | stdin | Entrada estándar | `< archivo` |
| 1 | stdout | Salida estándar | `> archivo` o `1>` |
| 2 | stderr | Error estándar | `2> archivo` |

### Redirección de Salida

```bash
# Sobrescribir archivo (stdout)
comando > archivo

# Agregar al archivo (append)
comando >> archivo

# Redirigir stderr
comando 2> archivo_errores

# Redirigir stdout y stderr juntos
comando &> archivo
comando > archivo 2>&1

# Descartar salida (/dev/null)
comando > /dev/null
comando 2> /dev/null
comando &> /dev/null
```

### Redirección de Entrada

```bash
# Leer desde archivo
comando < archivo
cat < archivo.txt
```

### Pipes (Tuberías)

```bash
# Conectar stdout de comando1 a stdin de comando2
comando1 | comando2

# Ejemplos
ls -l | less
ps aux | grep httpd
cat /etc/passwd | wc -l
dmesg | grep -i error | less
```

### Tee - Copiar a Archivo y Pantalla

```bash
# Ver Y guardar salida
comando | tee archivo
ls -la | tee listado.txt

# Append mode
comando | tee -a archivo
```

### Ejemplos Prácticos

```bash
# Guardar listado de procesos
ps aux > procesos.txt

# Agregar fecha al log
date >> sistema.log

# Guardar solo errores
find / -name "*.conf" 2> errores.txt

# Buscar en logs y guardar resultados
grep ERROR /var/log/messages > errores_encontrados.txt

# Pipeline complejo
cat archivo.txt | grep patron | sort | uniq | wc -l

# Ver y guardar al mismo tiempo
ls -laR /etc | tee estructura_etc.txt | less

# Descartar errores, mostrar solo salida válida
find / -name "config" 2> /dev/null
```

---

## CAPÍTULO 10: Gestión de Usuarios y Grupos Locales

### Conceptos Usuarios/Grupos

- **UID**: User ID (identificador numérico)
  - 0 = root
  - 1-999 = usuarios del sistema
  - 1000+ = usuarios regulares
- **GID**: Group ID
- **Primary Group**: Grupo principal del usuario
- **Supplementary Groups**: Grupos adicionales

### Archivos Importantes

```bash
/etc/passwd     # Información de usuarios (UID, home, shell)
/etc/shadow     # Passwords encriptados
/etc/group      # Información de grupos
/etc/gshadow    # Passwords de grupos (raramente usado)
```

### Formato /etc/passwd

```
username:x:UID:GID:GECOS:home:shell
root:x:0:0:root:/root:/bin/bash
user1:x:1001:1001:User One:/home/user1:/bin/bash
```

### Gestión de Usuarios

```bash
# Ver usuario actual
whoami
id                 # Muestra UID, GID, grupos

# Crear usuario
sudo useradd username
sudo useradd -m username           # Crear home directory
sudo useradd -u 1500 username      # UID específico
sudo useradd -g grupo username     # Grupo primario específico
sudo useradd -G grp1,grp2 username # Grupos suplementarios
sudo useradd -s /bin/bash username # Shell específico
sudo useradd -c "Nombre Completo" username # Comentario (GECOS)

# Modificar usuario
sudo usermod -l nuevo_nombre viejo_nombre  # Cambiar nombre
sudo usermod -u 1500 username              # Cambiar UID
sudo usermod -g grupo username             # Cambiar grupo primario
sudo usermod -aG grupo username            # AGREGAR a grupo (append)
sudo usermod -G grp1,grp2 username         # REEMPLAZAR grupos suplementarios
sudo usermod -s /bin/bash username         # Cambiar shell
sudo usermod -L username                   # Bloquear cuenta (lock)
sudo usermod -U username                   # Desbloquear cuenta (unlock)

# Eliminar usuario
sudo userdel username              # Eliminar (mantiene home)
sudo userdel -r username           # Eliminar con home directory
```

### Gestión de Grupos

```bash
# Ver grupos del usuario
groups
groups username
id username

# Crear grupo
sudo groupadd nombre_grupo
sudo groupadd -g 2000 nombre_grupo    # GID específico

# Modificar grupo
sudo groupmod -n nuevo_nombre viejo   # Cambiar nombre
sudo groupmod -g 2500 nombre_grupo    # Cambiar GID

# Eliminar grupo
sudo groupdel nombre_grupo

# Agregar usuario a grupo
sudo usermod -aG grupo username
sudo gpasswd -a username grupo        # Alternativa

# Eliminar usuario de grupo
sudo gpasswd -d username grupo
```

### Gestión de Passwords

```bash
# Cambiar password propio
passwd

# Cambiar password de otro usuario (como root)
sudo passwd username

# Configurar expiración
sudo passwd -e username               # Forzar cambio en próximo login
sudo passwd -l username               # Bloquear cuenta
sudo passwd -u username               # Desbloquear cuenta
sudo passwd -S username               # Ver estado de password

# Comando chage (gestión avanzada)
sudo chage -l username                # Ver política de password
sudo chage -M 90 username             # Max 90 días antes de expirar
sudo chage -m 7 username              # Mín 7 días entre cambios
sudo chage -W 14 username             # Advertir 14 días antes
sudo chage -E 2025-12-31 username     # Cuenta expira en fecha
```

### Acceso Superusuario

```bash
# su (switch user)
su                   # Cambia a root (shell no-login)
su -                 # Cambia a root (shell login completo)
su - username        # Cambia a otro usuario (shell login)
su username          # Cambia sin cambiar entorno

# sudo (ejecutar comando como root)
sudo comando         # Ejecutar comando como root
sudo -i              # Shell root interactivo (login shell)
sudo -s              # Shell root (non-login shell)
sudo -u user comando # Ejecutar como otro usuario

# Ver privilegios sudo
sudo -l              # Listar comandos permitidos
```

### Archivo /etc/sudoers

```bash
# Editar (SIEMPRE usar visudo)
sudo visudo

# Formato básico:
user    ALL=(ALL)   ALL
%grupo  ALL=(ALL)   ALL           # Grupo (%) puede ejecutar todo
user    ALL=(ALL)   NOPASSWD: ALL # Sin solicitar password

# Ejemplos prácticos:
john    ALL=(ALL)   /usr/bin/systemctl restart httpd
%admin  ALL=(ALL)   ALL
```

### Ejemplos Prácticos Completos

```bash
# Crear usuario completo para desarrollo
sudo useradd -m -c "Developer John Doe" -G wheel,developers -s /bin/bash jdoe
sudo passwd jdoe

# Ver información completa del usuario
id jdoe
groups jdoe
grep jdoe /etc/passwd

# Agregar usuario existente a múltiples grupos
sudo usermod -aG docker,www-data,sudo usuario

# Bloquear temporalmente un usuario
sudo passwd -l usuario
sudo usermod -L usuario        # Alternativa

# Crear grupo de proyecto y agregar usuarios
sudo groupadd proyecto-web
sudo usermod -aG proyecto-web alice
sudo usermod -aG proyecto-web bob

# Forzar cambio de password en próximo login
sudo chage -d 0 username

# Ejecutar comando como otro usuario
sudo -u apache cat /var/log/httpd/error_log
```

---

## RESUMEN DE COMANDOS CLAVE

### File System
```bash
pwd, cd, ls, mkdir, cp, mv, rm, touch
ln, ln -s, rmdir
```

### Wildcards
```bash
*, ?, [abc], [!abc], [a-z]
```

### Vim
```bash
i, a, o, Esc, :w, :q, :wq, dd, yy, p, u, /buscar, :%s/old/new/g
```

### Redirección
```bash
>, >>, 2>, &>, <, |, tee, /dev/null
```

### Usuarios/Grupos
```bash
useradd, usermod, userdel, groupadd, groupmod, groupdel
passwd, chage, id, groups, whoami, su, sudo, visudo
```
