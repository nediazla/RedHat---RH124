# Capítulo 3: Obteniendo Ayuda desde Documentación Local

## Conceptos Clave

### Manual Pages (man pages)
- **Documentación local del sistema** almacenada en subdirectorios de `/usr/share/man`
- **Provienen del Linux Programmer's Manual** - dividido en secciones temáticas
- **Acceso con comando `man`** - No requiere conexión a internet

### Referencia de Manual Pages
```bash
passwd(1)  # Sección 1, comando passwd
passwd(5)  # Sección 5, formato archivo /etc/passwd
```

**Formato**: `nombre(sección)`

## Secciones del Manual Linux

| Sección | Tipo de Contenido | Descripción |
|---------|-------------------|-------------|
| **1** | User commands | Comandos ejecutables y programas shell |
| **2** | System calls | Rutinas del kernel invocadas desde espacio usuario |
| **3** | Library functions | Funciones provistas por librerías de programas |
| **4** | Special files | Ej: archivos de dispositivo |
| **5** | File formats | Formatos de archivos de configuración y estructuras |
| **6** | Games | Sección histórica para programas de entretenimiento |
| **7** | Conventions | Protocolos, sistemas de archivos, convenciones |
| **8** | System administration | Comandos de mantenimiento y tareas privilegiadas |
| **9** | Linux kernel API | Llamadas internas del kernel |

### Secciones Más Usadas para Administración
1. **Sección 1**: Comandos de usuario
2. **Sección 5**: Formatos de archivo de configuración
3. **Sección 8**: Comandos administrativos

## Comandos para Acceder a Man Pages

### Acceso Básico

```bash
# Abrir man page de un comando
man ssh

# Abrir man page de sección específica
man 1 passwd    # Comando passwd
man 5 passwd    # Archivo /etc/passwd
man 8 tune2fs   # Comando administrativo

# Abrir intro de una sección
man 1 intro     # Intro a comandos de usuario
man 5 intro     # Intro a formatos de archivo
man 8 intro     # Intro a comandos administrativos
```

### Búsqueda de Man Pages

```bash
# man -f = whatis (buscar por título exacto)
man -f passwd
whatis passwd
# Salida:
# passwd (1) - update user's authentication tokens
# passwd (5) - password file

# man -k = apropos (buscar en títulos y descripciones)
man -k passwd
apropos passwd
# Busca "passwd" en títulos y descripciones de todas las pages

# man -K (búsqueda de texto completo - lento)
man -K passwd
# Busca en todo el texto de todas las man pages
```

## Navegación en Man Pages

### Comandos de Navegación

| Comando | Función |
|---------|---------|
| `Espacio` o `PgDn` | Avanzar una pantalla |
| `PgUp` | Retroceder una pantalla |
| `↓` | Avanzar una línea |
| `↑` | Retroceder una línea |
| `d` | Avanzar media pantalla |
| `u` | Retroceder media pantalla |
| `g` | Ir al inicio de la página |
| `G` | Ir al final de la página |
| `/texto` | Buscar "texto" hacia adelante |
| `n` | Siguiente resultado de búsqueda |
| `N` o `Shift+N` | Resultado anterior de búsqueda |
| `q` | Salir de man page |
| `h` | Ayuda de navegación |

### Búsqueda dentro de Man Page

```bash
# Dentro de man page:
/Listen          # Buscar "Listen"
n                # Siguiente ocurrencia
N                # Ocurrencia anterior

# Búsqueda con expresiones regulares
/^Listen         # Líneas que empiezan con "Listen"
/Listen$         # Líneas que terminan con "Listen"
```

## Estructura de una Man Page

### Encabezados Comunes

| Encabezado | Contenido |
|------------|-----------|
| **NAME** | Nombre del tema y breve definición |
| **SYNOPSIS** | Sintaxis del comando |
| **DESCRIPTION** | Descripción detallada del funcionamiento |
| **OPTIONS** | Explicación de opciones de ejecución |
| **EXAMPLES** | Ejemplos de uso |
| **FILES** | Archivos y directorios relacionados |
| **SEE ALSO** | Información relacionada, otras man pages |
| **BUGS** | Bugs conocidos en el software |
| **AUTHOR** | Información sobre contribuidores |

### Interpretación del SYNOPSIS

```bash
# Ejemplo de SYNOPSIS
passwd [options] [LOGIN]

# Convenciones:
# [] = opcional
# MAYÚSCULAS = reemplazar con valor
# ... = puede repetirse
# | = elegir una opción
# {} = obligatorio
```

Ejemplos de sintaxis:
```bash
ssh [-46AaCfGgKkMNnqsTtVvXxYy] [-B bind_interface] user@host
# -46AaCf... = opciones opcionales
# user@host = argumentos requeridos

vi [options] [file ...]
# [file ...] = puede especificar múltiples archivos
```

## Comandos de Búsqueda

### man -k (apropos)

```bash
# Buscar comandos relacionados con red
man -k network

# Buscar relacionado con SELinux
man -k selinux
# Salida incluye:
# getenforce (8) - get the current mode of SELinux
# setenforce (8) - modify the mode SELinux is running in

# Buscar comandos de arranque
man -k boot
# bootctl (1) - Control EFI firmware boot settings
# bootparam (7) - boot time parameters of Linux kernel

# Buscar herramientas ext4
man -k ext4
# tune2fs (8) - adjust tunable file system parameters
# resize2fs (8) - ext2/ext3/ext4 file system resizer
```

### man -f (whatis)

```bash
# Descripción corta de un comando
man -f ls
# ls (1) - list directory contents

man -f w
# w (1) - Show who is logged on and what they are doing

# Mostrar todas las secciones de un tema
man -f passwd
# passwd (1) - update user's authentication tokens
# passwd (5) - password file
```

## Mantenimiento de la Base de Datos Man

```bash
# Actualizar índice de man pages (como root)
sudo mandb

# El servicio man-db-cache-update ejecuta mandb automáticamente
# cuando se instalan paquetes con man pages
```

**Nota**: Las búsquedas con `man -k` y `man -f` dependen del índice generado por `mandb`

## Ejemplos Prácticos

### Ejemplo 1: Explorar Comando Desconocido

```bash
# 1. Buscar comando para cambiar hostname
man -k hostname
# hostname (1) - show or set the system's host name
# hostnamectl (1) - Control the system hostname

# 2. Ver descripción rápida
man -f hostname

# 3. Leer man page completa
man hostname

# 4. Buscar dentro de la página la opción que necesitas
# Presionar /FQDN para buscar "FQDN"
```

### Ejemplo 2: Encontrar Archivo de Configuración

```bash
# Buscar información sobre /etc/passwd
man -k passwd | grep "(5)"
# passwd (5) - password file

# Abrir man page de formato de archivo
man 5 passwd

# Ver estructura del archivo
# Salida de man page mostrará formato:
# username:x:UID:GID:GECOS:home:shell
```

### Ejemplo 3: Trabajar con Comandos Similares

```bash
# Buscar todos los comandos "tune"
man -k tune
# tune2fs (8) - adjust tunable parameters on ext2/ext3/ext4

# Ver man page específico
man 8 tune2fs

# Buscar opciones específicas dentro de la página
# / -L  (buscar opción -L)
```

### Ejemplo 4: Uso de man con vi/vim

```bash
# Ver cómo abrir archivo en línea específica
man vi
# Buscar: /+\[num\]

# Según la man page:
# +[num] - cursor se posicionará en línea "num"

# Ejemplo de uso
vi +25 /etc/ssh/sshd_config    # Abre en línea 25
vi +$ archivo.txt               # Abre al final del archivo
```

### Ejemplo 5: Información sobre Comandos del Sistema

```bash
# Buscar comando para ajustar fecha/hora
man -k time | grep set
# timedatectl (1) - Control the system time and date

# Ver man page
man timedatectl

# Buscar ejemplos dentro de la página
# /EXAMPLES
```

### Ejemplo 6: Troubleshooting con Man Pages

```bash
# Problema: comando "su" no funciona como esperado

# 1. Ver man page
man su

# 2. Buscar diferencias entre opciones
# /- (buscar el guion simple)
# Leer sobre diferencia entre "su" y "su -"

# Resultado:
# su user     = Shell no-login, mantiene entorno actual
# su - user   = Shell login, nuevo entorno limpio
```

## Búsqueda Eficiente

### Patrones de Búsqueda

```bash
# Buscar por categoría funcional
man -k copy              # Comandos para copiar
man -k compress          # Compresión de archivos
man -k encrypt           # Encriptación
man -k firewall          # Firewall
man -k partition         # Particionamiento

# Buscar por tipo de tarea
man -k "user management"
man -k "network config"
man -k "file system"

# Limitar búsqueda por sección
man -k passwd | grep "(8)"    # Solo comandos admin
man -k passwd | grep "(5)"    # Solo formatos archivo
```

## Comandos Relacionados

```bash
# Información sobre comandos instalados
which comando           # Ubicación del ejecutable
type comando            # Tipo de comando (builtin, alias, etc)
whereis comando         # Ubicaciones de binario, man page, fuentes

# Ayuda rápida de comandos
comando --help          # Ayuda corta
comando -h              # Alternativa de ayuda corta
info comando            # Sistema info (más detallado que man)
```

## Alternativas a Man Pages

### --help y -h

```bash
# Ayuda rápida inline
ls --help
grep --help
systemctl --help

# Formato típico:
# Usage, opciones comunes, ejemplos básicos
```

### info Pages

```bash
# Sistema de documentación GNU Info
info ls
info grep
info coreutils     # Colección de utilidades básicas

# Navegación en info:
# n = next node
# p = previous node
# u = up node
# q = quit
```

### Documentación en /usr/share/doc

```bash
# Documentación adicional de paquetes
ls /usr/share/doc/bash/
ls /usr/share/doc/httpd/

# Puede incluir READMEs, ejemplos, changelogs
cat /usr/share/doc/httpd/README
```

## Regex en Man Pages

**Importante**: Las búsquedas en man pages soportan expresiones regulares

```bash
# Dentro de man page:
/^OPTIONS$          # Buscar exactamente "OPTIONS" como línea completa
/\-a\>              # Buscar opción -a
/listen.*port       # Buscar "listen" seguido de "port"
```

**Caracteres especiales regex**: `^ $ * . [ ] \ { } + ? |`

## Tips para Examen

1. **man -k busca en títulos Y descripciones** - Más amplio que man -f
2. **man -f = whatis** - Búsqueda exacta de nombre
3. **Siempre especificar sección si hay ambigüedad** - `man 5 passwd` vs `man 1 passwd`
4. **Sección 8 para comandos administrativos** - Comandos que requieren root/sudo
5. **La navegación en man es igual que en less** - Mismo motor de paginación
6. **`/` para buscar dentro de man page** - `n` para siguiente, `N` para anterior
7. **`g` y `G` para inicio y fin** - Como en vi/vim
8. **`q` para salir** - Siempre
9. **SYNOPSIS muestra sintaxis correcta** - `[]` = opcional, `MAYÚSCULAS` = reemplazar
10. **SEE ALSO enlaza documentación relacionada** - Útil para descubrir comandos similares

## Flujo de Trabajo Recomendado

```bash
# 1. ¿No sabes qué comando usar? Busca por función
man -k "file system"
man -k mount

# 2. ¿Conoces el comando pero no la sintaxis? Ve directo a man
man mount

# 3. ¿Necesitas referencia rápida? Usa --help
mount --help

# 4. ¿Necesitas ejemplos? Busca EXAMPLES en man page
man mount
/EXAMPLES

# 5. ¿Dudas sobre archivo de configuración? Busca sección 5
man 5 fstab
man 5 passwd
man 5 hosts
```

## Referencias Importantes

```bash
# Man pages sobre el propio sistema de man
man man          # Cómo usar man
man man-pages    # Convenciones de man pages
man intro        # Introducción general
man regex        # Expresiones regulares (regex(7))
```

## Comandos de Referencia Rápida

```bash
# Búsqueda
man -k keyword      # Buscar por palabra clave (apropos)
man -f command      # Descripción corta (whatis)
man -K keyword      # Búsqueda texto completo (lento)

# Acceso
man command         # Man page de comando
man N topic         # Man page sección N
man -a topic        # Todas las secciones de topic

# Mantenimiento
sudo mandb          # Actualizar índice de man pages

# Navegación (dentro de man page)
q                   # Salir
/texto              # Buscar
n / N               # Siguiente/anterior resultado
g / G               # Inicio/fin de página
h                   # Ayuda de navegación
```

## Ejemplo Completo de Sesión

```bash
# Necesito configurar tiempo del sistema

# 1. Buscar comandos relacionados
$ man -k time | grep set
timedatectl (1) - Control the system time and date
hwclock (8) - time clocks utility

# 2. Ver descripción de candidatos
$ man -f timedatectl
timedatectl (1) - Control the system time and date

# 3. Abrir man page
$ man timedatectl

# 4. Dentro de man page:
/EXAMPLES          # Buscar sección de ejemplos
n                  # Siguiente ejemplo
q                  # Salir cuando tengas la info

# 5. Ejecutar comando aprendido
$ timedatectl status
```

## Documentación Adicional

### Archivos de Configuración Importantes

```bash
man 5 passwd        # /etc/passwd - Base de datos de usuarios
man 5 group         # /etc/group - Grupos del sistema
man 5 shadow        # /etc/shadow - Passwords encriptados
man 5 fstab         # /etc/fstab - Tabla de sistemas de archivos
man 5 hosts         # /etc/hosts - Tabla de hosts estática
man 5 crontab       # Formato de archivos crontab
man 5 sshd_config   # Configuración del servidor SSH
```

### Comandos Administrativos Comunes

```bash
man 8 useradd       # Crear usuarios
man 8 usermod       # Modificar usuarios
man 8 groupadd      # Crear grupos
man 8 mount         # Montar sistemas de archivos
man 8 systemctl     # Control de servicios systemd
man 8 firewalld     # Configuración de firewall
man 8 setenforce    # Configurar modo SELinux
```
