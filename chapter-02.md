# Capítulo 2: Acceso a la Línea de Comandos

## Conceptos Clave

### Bash Shell
- **Shell**: Programa que interpreta comandos ingresados por el usuario
- **Bash**: GNU Bourne-Again Shell - shell predeterminado en RHEL
- **Prompt**: Indicador visual que muestra que el shell espera entrada

### Tipos de Prompt

```bash
# Usuario regular
user@host:~$

# Superusuario (root)
root@host:~#
```

- `$` = Usuario regular
- `#` = Superusuario (root)

## Estructura de Comandos

```bash
comando [opciones] [argumentos]
```

### Componentes
1. **Comando**: Nombre del programa a ejecutar
2. **Opciones**: Modifican comportamiento (inician con `-` o `--`)
3. **Argumentos**: Objetivos del comando

### Ejemplos

```bash
# Comando simple
whoami

# Comando con opción
ls -l

# Comando con opción y argumento
ls -l /etc

# Comando con opción larga
ls --all

# Comando con múltiples opciones
ls -la /home

# Ejemplo con usermod
usermod -L user01
# usermod = comando
# -L = opción (lock password)
# user01 = argumento (usuario)
```

## Consolas y Terminales

### Tipos de Consolas
- **Physical Console**: Teclado y pantalla físicamente conectados
- **Virtual Consoles**: Múltiples sesiones de login independientes (tty1-tty6)
- **Serial Console**: Para servidores headless vía puerto serial

### Cambio entre Consolas Virtuales
```bash
Ctrl + Alt + F1  # tty1 (login gráfico)
Ctrl + Alt + F2  # tty2 (terminal texto o sesión gráfica)
Ctrl + Alt + F3  # tty3 (terminal texto)
...
Ctrl + Alt + F6  # tty6 (terminal texto)
```

### Distribución en RHEL 10
- **tty1**: Login screen gráfico
- **tty2-tty6**: Terminales de texto o sesiones gráficas adicionales

## Conexión Remota con SSH

### Sintaxis Básica

```bash
# Login con password
ssh usuario@host

# Login con clave privada
ssh -i archivo_clave.pem usuario@host

# Ejemplo completo
ssh remoteuser@192.168.1.100
```

### Autenticación por Clave Pública

1. **Configurar permisos correctos**
```bash
chmod 600 mykey.pem
```

2. **Conectar con clave**
```bash
ssh -i mykey.pem remoteuser@remotehost
```

### Primera Conexión
- SSH muestra fingerprint de host key
- Responder `yes` para aceptar y guardar
- Se guarda en `~/.ssh/known_hosts`

### Cerrar Sesión SSH
```bash
exit
# o
Ctrl + D
```

## Entorno Gráfico GNOME

### Componentes Principales

1. **Top Bar**: Barra superior siempre visible
2. **Activities Overview**: Organizador de ventanas y aplicaciones
3. **Message Tray**: Notificaciones del sistema
4. **System Menu**: Menú de sistema (esquina superior derecha)

### Acceso a Terminal en GNOME

```bash
# Método 1: Activities
Super (tecla Windows) → escribir "terminal" → Enter

# Método 2: Buscar aplicación
Click en logo Red Hat → buscar "Terminal"
```

### Operaciones Comunes

```bash
# Cambiar password desde terminal
passwd

# Bloquear pantalla
Click system menu → Icono candado
# o desde terminal
Super + L

# Cerrar sesión
Click system menu → Power icon → Log Out

# Apagar sistema
Click system menu → Power icon → Power Off
```

## Comandos Básicos Bash

### Información del Sistema

```bash
# Mostrar usuario actual
whoami

# Mostrar fecha y hora
date

# Fecha con formato personalizado
date +%R          # Hora formato 24h (17:58)
date +%x          # Fecha formato corto (04/29/25)
date +%Y-%m-%d    # Formato YYYY-MM-DD
```

### Gestión de Archivos

```bash
# Identificar tipo de archivo
file /path/archivo
file script.sh

# Ver contenido completo
cat archivo.txt

# Ver contenido paginado
less archivo.txt
more archivo.txt

# Ver primeras líneas
head archivo.txt           # Primeras 10 líneas
head -n 5 archivo.txt      # Primeras 5 líneas

# Ver últimas líneas
tail archivo.txt           # Últimas 10 líneas
tail -n 20 archivo.txt     # Últimas 20 líneas
tail -f /var/log/messages  # Seguir archivo en tiempo real

# Contar líneas, palabras, bytes
wc archivo.txt
wc -l archivo.txt          # Solo líneas
wc -w archivo.txt          # Solo palabras
```

### Separar Comandos en una Línea

```bash
# Separar con punto y coma
comando1 ; comando2 ; comando3

# Ejemplo
date ; whoami ; pwd
```

## Historial de Comandos

### Comandos de Historial

```bash
# Ver historial completo
history

# Re-ejecutar comando por número
!número
!23

# Re-ejecutar comando por patrón
!string
!cat

# Re-ejecutar último comando
!!

# Re-ejecutar último comando que empieza con "ssh"
!ssh
```

### Navegación en Historial

| Tecla/Comando | Función |
|---------------|---------|
| `↑` Up Arrow | Comando anterior en historial |
| `↓` Down Arrow | Siguiente comando en historial |
| `←` Left Arrow | Mover cursor a la izquierda |
| `→` Right Arrow | Mover cursor a la derecha |
| `Esc + .` o `Alt + .` | Insertar último argumento del comando anterior |

## Edición de Línea de Comandos

### Atajos de Teclado Esenciales

| Atajo | Función |
|-------|---------|
| `Ctrl + A` | Ir al inicio de la línea |
| `Ctrl + E` | Ir al final de la línea |
| `Ctrl + U` | Borrar desde cursor hasta inicio |
| `Ctrl + K` | Borrar desde cursor hasta final |
| `Ctrl + →` | Saltar al final de siguiente palabra |
| `Ctrl + ←` | Saltar al inicio de palabra anterior |
| `Ctrl + R` | Buscar en historial |
| `Ctrl + C` | Cancelar comando actual |
| `Ctrl + D` | Cerrar sesión (equivalente a exit) |
| `Ctrl + L` | Limpiar pantalla (equivalente a clear) |
| `Tab` | Autocompletar comandos/archivos |

### Autocompletado con Tab

```bash
# Autocompletar comando
da[Tab]          # Completa a 'date' si es único

# Autocompletar ruta
cd /et[Tab]      # Completa a /etc/

# Doble Tab muestra opciones
ls --a[Tab][Tab] # Muestra todas las opciones que empiezan con --a
```

## Ejemplos Prácticos

### Ejemplo 1: Explorar Sistema

```bash
# Ver información actual
whoami
date
hostname

# Ver tipo de archivos en directorio
file /bin/bash
file /etc/passwd
file /home/user/imagen.jpg

# Explorar logs del sistema
tail -20 /var/log/messages
tail -f /var/log/messages  # Ver en tiempo real
```

### Ejemplo 2: Análisis de Archivos

```bash
# Ver primeras líneas de configuración
head -15 /etc/ssh/sshd_config

# Ver últimas líneas de log
tail -30 /var/log/secure

# Contar líneas en archivo
wc -l /etc/passwd

# Ver archivo completo paginado
less /etc/services
```

### Ejemplo 3: Uso Eficiente del Historial

```bash
# Ejecutar serie de comandos
ls -l /etc
cat /etc/hosts
wc -l /etc/passwd

# Ver historial
history

# Re-ejecutar comando 150
!150

# Re-ejecutar último ls
!ls

# Usar último argumento del comando anterior
cat Esc+.  # Inserta /etc/passwd si fue último argumento
```

### Ejemplo 4: Conexión Remota

```bash
# Conectar a servidor remoto
ssh admin@servidor.ejemplo.com

# Conectar con clave privada
chmod 600 ~/.ssh/mi_clave.pem
ssh -i ~/.ssh/mi_clave.pem usuario@192.168.1.50

# En servidor remoto
whoami
hostname
date
# Trabajo en servidor...
exit  # Volver a máquina local
```

## Navegación en less/more

| Tecla | Función |
|-------|---------|
| `Espacio` | Avanzar una pantalla |
| `PageDown` | Avanzar una pantalla |
| `PageUp` | Retroceder una pantalla |
| `↓` | Avanzar una línea |
| `↑` | Retroceder una línea |
| `d` | Avanzar media pantalla |
| `u` | Retroceder media pantalla |
| `/texto` | Buscar "texto" hacia adelante |
| `n` | Siguiente resultado de búsqueda |
| `N` | Resultado anterior de búsqueda |
| `g` | Ir al inicio del archivo |
| `G` | Ir al final del archivo |
| `q` | Salir |

## Puntos Clave para Examen

1. **Bash es el shell predeterminado en RHEL** - Otros shells: zsh, csh, ksh
2. **`#` indica superusuario, `$` usuario regular** - Importante para seguridad
3. **SSH encripta conexiones** - Seguro para administración remota
4. **Consolas virtuales tty1-tty6** - Cambiar con Ctrl+Alt+F1-F6
5. **Tab completion ahorra tiempo** - Usar liberalmente para eficiencia
6. **Historial de comandos es persistente** - Se guarda en ~/.bash_history
7. **Ctrl+R para búsqueda interactiva** - Muy útil para comandos largos
8. **Permisos 600 para claves SSH** - Requerido para seguridad

## Configuración Importante

### Archivo de Historial
```bash
~/.bash_history  # Historial de comandos del usuario
```

### Variables de Entorno Relacionadas
```bash
HISTSIZE        # Número de comandos en memoria
HISTFILESIZE    # Número de comandos en archivo
PS1             # Formato del prompt
```

## Comandos de Referencia Rápida

```bash
# Sistema
whoami          # Usuario actual
hostname        # Nombre del host
date            # Fecha y hora
uptime          # Tiempo encendido
who             # Usuarios conectados

# Archivos
file            # Tipo de archivo
cat             # Mostrar contenido
less/more       # Ver paginado
head            # Primeras líneas
tail            # Últimas líneas
wc              # Contar líneas/palabras/bytes

# Historial
history         # Ver historial
!n              # Ejecutar comando número n
!!              # Repetir último comando
!string         # Ejecutar último comando que empieza con string

# Sesión
passwd          # Cambiar password
exit            # Cerrar sesión
clear           # Limpiar pantalla (Ctrl+L)
```

## Consejos para Productividad

1. **Usar Tab extensivamente** - Evita typos y ahorra tiempo
2. **Dominar Ctrl+R** - Buscar en historial interactivamente
3. **Usar Esc+.** - Reutilizar argumentos de comandos previos
4. **Aprender atajos Ctrl** - Ctrl+A, Ctrl+E, Ctrl+U, Ctrl+K
5. **Usar history + !número** - Re-ejecutar comandos complejos
6. **less mejor que cat** - Para archivos grandes
7. **tail -f para logs** - Monitorear archivos en tiempo real
