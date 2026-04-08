# Capítulos 11-20: Permisos, Software, Procesos, Servicios, Redes y SSH

## CAPÍTULO 11: Control de Acceso a Archivos (Permisos)

### Permisos Linux
Cada archivo tiene 3 tipos de permisos para 3 categorías de usuarios:

**Permisos (rwx)**:
- `r` (read/4): Leer
- `w` (write/2): Escribir
- `x` (execute/1): Ejecutar

**Categorías**:
- **u** (user): Propietario
- **g** (group): Grupo
- **o** (others): Otros

### Visualizar Permisos

```bash
ls -l archivo
# -rw-r--r--. 1 user group 1234 Mar 1 12:00 archivo.txt
# │││││││││
# │││││││││── Otros (read)
# │││││││─── Grupo (read)
# ││││││──── Propietario (read+write)
# │││││───── Tipo: - (regular), d (dir), l (symlink)

# Explicación detallada:
# - rw- r-- r--
#   │   │   └─ others: r-- (solo lectura)
#   │   └───── group:  r-- (solo lectura)
#   └───────── user:   rw- (lectura+escritura)
```

### chmod - Cambiar Permisos

#### Modo Simbólico
```bash
# Sintaxis: chmod [ugoa][+-=][rwx] archivo

chmod u+x script.sh          # Agregar ejecución al propietario
chmod g-w archivo            # Quitar escritura al grupo
chmod o=r archivo            # Establecer solo lectura para otros
chmod a+x script             # Agregar ejecución a todos (all)
chmod ug+rw,o-rwx archivo    # Múltiples cambios
chmod go-rw archivo          # Quitar read/write a grupo y otros
```

#### Modo Numérico (Octal)
```bash
# Permisos como suma: r=4, w=2, x=1
# rwx = 4+2+1 = 7
# rw- = 4+2   = 6
# r-- = 4     = 4
# r-x = 4+1   = 5

chmod 755 script.sh    # rwxr-xr-x
chmod 644 file.txt     # rw-r--r--
chmod 600 private.txt  # rw-------
chmod 777 file         # rwxrwxrwx (⚠️ evitar en producción)
chmod 700 dir          # rwx------ (solo propietario)
```

### chown - Cambiar Propietario

```bash
# Cambiar propietario
sudo chown usuario archivo

# Cambiar propietario y grupo
sudo chown usuario:grupo archivo

# Solo cambiar grupo (alternativa: chgrp)
sudo chown :grupo archivo

# Recursivo (directorios)
sudo chown -R usuario:grupo /path/directorio
```

### Permisos en Directorios

| Permiso | En Archivo | En Directorio |
|---------|------------|---------------|
| **r** | Leer contenido | Listar archivos (ls) |
| **w** | Modificar contenido | Crear/eliminar archivos |
| **x** | Ejecutar archivo | Entrar al directorio (cd) |

```bash
# Directorio típico para navegación
chmod 755 directorio    # rwxr-xr-x

# Directorio privado
chmod 700 privado       # rwx------
```

### Permisos Especiales

#### Setuid (Set User ID) - 4xxx
```bash
# Archivo se ejecuta con permisos del propietario
chmod u+s /usr/bin/programa
chmod 4755 programa      # rwsr-xr-x

# Ejemplo: /usr/bin/passwd tiene setuid root
```

#### Setgid (Set Group ID) - 2xxx
```bash
# Archivo: se ejecuta con permisos del grupo
# Directorio: archivos creados heredan grupo del directorio
chmod g+s directorio
chmod 2755 directorio    # rwxr-sr-x
```

#### Sticky Bit - 1xxx
```bash
# Solo propietario puede eliminar sus archivos en directorio
# Usado en /tmp
chmod +t /tmp
chmod 1777 directorio    # rwxrwxrwt

# Ejemplo: /tmp tiene sticky bit
```

### umask - Permisos Predeterminados

```bash
# Ver umask actual
umask
# 0022

# Cálculo de permisos:
# Archivos: 666 - umask = permisos finales
# Dirs:     777 - umask = permisos finales

# umask 0022:
# Archivos: 666 - 022 = 644 (rw-r--r--)
# Dirs:     777 - 022 = 755 (rwxr-xr-x)

# Cambiar umask (temporal)
umask 0027    # Archivos: 640, Dirs: 750

# Cambiar umask permanente: ~/.bashrc
echo "umask 0027" >> ~/.bashrc
```

---

## CAPÍTULO 12: Instalación y Actualización con RPM/DNF

### RPM (Red Hat Package Manager)
- **Formato de paquetes**: `.rpm`
- **Gestión de dependencias**: DNF/YUM

### Comandos RPM Básicos

```bash
# Listar paquetes instalados
rpm -qa                 # Query all
rpm -qa | grep httpd

# Información de paquete
rpm -qi paquete         # Query info
rpm -ql paquete         # Query list (archivos)
rpm -qc paquete         # Query config files
rpm -qd paquete         # Query documentation

# Qué paquete proporciona un archivo
rpm -qf /etc/passwd

# Información de paquete .rpm sin instalar
rpm -qip paquete.rpm
rpm -qlp paquete.rpm
```

### DNF (Dandified YUM)
Sucesor de YUM, gestiona dependencias automáticamente

#### Comandos Esenciales DNF

```bash
# Buscar paquete
dnf search keyword
dnf search httpd

# Información de paquete
dnf info paquete

# Instalar paquete
sudo dnf install paquete
sudo dnf install httpd
sudo dnf install -y paquete    # Sin confirmación

# Actualizar paquete
sudo dnf update paquete
sudo dnf update                # Actualizar todos

# Eliminar paquete
sudo dnf remove paquete

# Listar instalados
dnf list installed
dnf list installed | grep httpd

# Listar disponibles
dnf list available

# Grupos de paquetes
dnf group list
dnf group info "Development Tools"
sudo dnf group install "Development Tools"

# Historial de transacciones
dnf history
dnf history info 5
sudo dnf history undo 5        # Deshacer transacción

# Limpiar cache
sudo dnf clean all
```

### Repositorios DNF

```bash
# Listar repositorios
dnf repolist
dnf repolist all               # Incluye deshabilitados

# Habilitar repositorio
sudo dnf config-manager --enable repo-id

# Deshabilitar repositorio
sudo dnf config-manager --disable repo-id

# Ver configuración de repos
cat /etc/yum.repos.d/*.repo

# Agregar repositorio EPEL (ejemplo)
sudo dnf install epel-release
```

### Ejemplo Archivo Repositorio

```ini
# /etc/yum.repos.d/ejemplo.repo
[ejemplo-repo]
name=Ejemplo Repository
baseurl=https://repo.ejemplo.com/rhel/10/$basearch/
enabled=1
gpgcheck=1
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-ejemplo
```

---

## CAPÍTULO 13: Flatpak

### Flatpak - Sistema de Paquetes para Aplicaciones
- **Aislamiento**: Aplicaciones en sandbox
- **Independiente de distro**
- **Versiones específicas**: Múltiples versiones de dependencias

### Comandos Flatpak

```bash
# Agregar repositorio Flathub
flatpak remote-add --if-not-exists flathub \
  https://flathub.org/repo/flathub.flatpakrepo

# Buscar aplicación
flatpak search nombre

# Instalar aplicación
flatpak install flathub org.gimp.GIMP

# Listar instaladas
flatpak list

# Ejecutar aplicación
flatpak run org.gimp.GIMP

# Actualizar
flatpak update
flatpak update org.gimp.GIMP

# Desinstalar
flatpak uninstall org.gimp.GIMP

# Limpiar datos no usados
flatpak uninstall --unused
```

---

## CAPÍTULO 14: Media Removible

### Identificar Dispositivos de Bloques

```bash
# Listar dispositivos de bloque
lsblk
lsblk -f                       # Con filesystem info

# Ver particiones
fdisk -l
parted -l

# Información de sistema de archivos
blkid
df -h                          # Discos montados
```

### Montar Sistemas de Archivos

```bash
# Montar manualmente
sudo mount /dev/sdb1 /mnt/usb
sudo mount -t vfat /dev/sdb1 /mnt/usb    # Especificar tipo

# Ver montados
mount
df -h

# Desmontar
sudo umount /mnt/usb
sudo umount /dev/sdb1

# Montar permanentemente: /etc/fstab
# device    mountpoint    type    options    dump    pass
/dev/sdb1   /mnt/data     ext4    defaults   0       2
UUID=xxx... /mnt/data     ext4    defaults   0       2
```

### Comando find - Buscar Archivos

```bash
# Por nombre
find /path -name "archivo.txt"
find /etc -name "*.conf"

# Por tipo
find /home -type f             # Archivos regulares
find /home -type d             # Directorios

# Por tamaño
find / -size +100M             # Mayor a 100MB
find /var -size -1M            # Menor a 1MB

# Por tiempo
find /tmp -mtime -7            # Modificados últimos 7 días
find /var/log -mtime +30       # Modificados hace más de 30 días

# Por permisos
find /home -perm 777
find /usr/bin -perm /u+s       # Con setuid

# Por propietario
find /home -user juan

# Ejecutar comando en resultados
find /tmp -type f -name "*.log" -exec rm {} \;
find /home -type f -name "*.bak" -delete

# Combinaciones
find /var/log -name "*.log" -size +10M -mtime +30
```

---

## CAPÍTULO 15: Procesos Linux

### Conceptos de Procesos
- **PID**: Process ID (identificador único)
- **PPID**: Parent Process ID
- **Estado**: Running, Sleeping, Stopped, Zombie

### Comandos de Monitoreo

```bash
# Ver procesos
ps                             # Procesos del usuario actual
ps aux                         # Todos los procesos (estilo BSD)
ps -ef                         # Todos los procesos (estilo UNIX)
ps -u usuario                  # Procesos de usuario específico
ps -C comando                  # Procesos de comando específico

# top - Monitor interactivo
top
# Teclas en top:
# q - salir
# k - kill proceso
# r - renice (cambiar prioridad)
# M - ordenar por memoria
# P - ordenar por CPU
# 1 - mostrar todos los CPUs

# htop (si está instalado - más amigable)
htop
```

### Señales de Procesos

| Señal | Número | Descripción |
|-------|--------|-------------|
| **SIGHUP** | 1 | Hang up - recargar configuración |
| **SIGINT** | 2 | Interrupt (Ctrl+C) |
| **SIGKILL** | 9 | Kill forzoso (no puede ser ignorado) |
| **SIGTERM** | 15 | Terminate (default, permite cleanup) |
| **SIGSTOP** | 19 | Stop proceso (no puede ser ignorado) |
| **SIGCONT** | 18 | Continue proceso detenido |

### Gestión de Procesos

```bash
# Enviar señal a proceso
kill PID                       # SIGTERM (15) por defecto
kill -9 PID                    # SIGKILL forzoso
kill -15 PID                   # SIGTERM explícito
kill -HUP PID                  # Recargar configuración
kill -STOP PID                 # Detener (pausar)
kill -CONT PID                 # Continuar

# Killall - por nombre
killall firefox
killall -9 httpd

# pkill - por patrón
pkill -u usuario               # Matar procesos de usuario
pkill -9 firefox

# pgrep - buscar PIDs por patrón
pgrep httpd
pgrep -u root                  # Procesos de root
```

### Job Control (Control de Trabajos)

```bash
# Ejecutar en background
comando &
sleep 100 &

# Ver jobs
jobs
jobs -l                        # Con PIDs

# Traer a foreground
fg %1                          # Job número 1
fg                             # Job más reciente

# Enviar a background
bg %1
bg                             # Job más reciente

# Suspender job en foreground
Ctrl+Z

# Ejemplo de flujo:
sleep 200
Ctrl+Z                         # Suspender
bg                             # Continuar en background
jobs                           # Ver estado
fg                             # Traer a foreground
```

### Nice y Renice - Prioridad

```bash
# Valores nice: -20 (alta prioridad) a +19 (baja prioridad)
# Default: 0
# Solo root puede usar valores negativos

# Iniciar con prioridad
nice -n 10 comando             # Baja prioridad
sudo nice -n -10 comando       # Alta prioridad

# Cambiar prioridad de proceso existente
renice -n 5 -p PID
sudo renice -n -5 -p PID
```

---

## CAPÍTULO 16: Servicios y Daemons (systemd)

### systemd - Gestor de Servicios
- **Units**: Servicios, sockets, targets, devices, etc
- **Targets**: Equivalente a runlevels

### systemctl - Control de Servicios

```bash
# Estado de servicio
systemctl status httpd
systemctl is-active httpd
systemctl is-enabled httpd

# Iniciar/Detener servicio
sudo systemctl start httpd
sudo systemctl stop httpd
sudo systemctl restart httpd
sudo systemctl reload httpd    # Recargar config sin detener

# Habilitar/Deshabilitar al boot
sudo systemctl enable httpd
sudo systemctl disable httpd
sudo systemctl enable --now httpd    # Enable + start

# Listar servicios
systemctl list-units --type=service
systemctl list-units --type=service --state=running
systemctl list-units --type=service --state=failed

# Ver dependencias
systemctl list-dependencies httpd

# Enmascarar servicio (prevenir que inicie)
sudo systemctl mask httpd
sudo systemctl unmask httpd
```

### Unit Files

```bash
# Ubicaciones
/usr/lib/systemd/system/       # Unit files del sistema
/etc/systemd/system/           # Unit files locales (override)

# Ver contenido de unit
systemctl cat httpd.service

# Editar (crea override)
sudo systemctl edit httpd.service

# Recargar configuración systemd
sudo systemctl daemon-reload
```

### Targets (Runlevels)

```bash
# Ver target actual
systemctl get-default

# Cambiar target default
sudo systemctl set-default multi-user.target
sudo systemctl set-default graphical.target

# Cambiar target actual (sin reboot)
sudo systemctl isolate multi-user.target

# Targets comunes:
# poweroff.target      - runlevel 0
# rescue.target        - runlevel 1
# multi-user.target    - runlevel 3 (CLI)
# graphical.target     - runlevel 5 (GUI)
# reboot.target        - runlevel 6
```

---

## CAPÍTULO 17-18: Networking

### Conceptos de Red
- **IPv4**: 192.168.1.100
- **IPv6**: 2001:db8::1
- **Subnet mask**: 255.255.255.0 o /24
- **Gateway**: Puerta de enlace predeterminada
- **DNS**: Sistema de nombres de dominio

### Ver Configuración de Red

```bash
# Interfaces de red
ip addr show
ip a                           # Forma corta
ip link show                   # Solo enlaces

# Rutas
ip route show
ip r                           # Forma corta

# DNS
cat /etc/resolv.conf

# Hostname
hostname
hostnamectl

# Pruebas de conectividad
ping ejemplo.com
ping -c 4 192.168.1.1          # 4 paquetes

# Trazar ruta
traceroute ejemplo.com
tracepath ejemplo.com

# Puertos abiertos
ss -tuln                       # Sockets TCP/UDP
ss -tunlp                      # Con procesos
netstat -tuln                  # Alternativa (deprecated)
```

### NetworkManager (nmcli)

```bash
# Ver conexiones
nmcli connection show
nmcli con show

# Ver dispositivos
nmcli device status

# Información de conexión
nmcli con show "nombre-conexion"

# Crear conexión
sudo nmcli con add con-name eth0-static \
  type ethernet ifname eth0 \
  ip4 192.168.1.100/24 gw4 192.168.1.1

# Modificar conexión
sudo nmcli con mod eth0-static ipv4.dns "8.8.8.8 8.8.4.4"
sudo nmcli con mod eth0-static ipv4.method manual

# Activar/Desactivar conexión
sudo nmcli con up eth0-static
sudo nmcli con down eth0-static

# Recargar configuración
sudo nmcli con reload

# DHCP
sudo nmcli con mod eth0-static ipv4.method auto
```

### Archivos de Configuración

```bash
# NetworkManager connections
/etc/NetworkManager/system-connections/

# Archivo de ejemplo
# /etc/NetworkManager/system-connections/eth0-static.nmconnection
[connection]
id=eth0-static
type=ethernet
interface-name=eth0

[ipv4]
method=manual
address1=192.168.1.100/24,192.168.1.1
dns=8.8.8.8;8.8.4.4;
```

### Hostname

```bash
# Ver hostname
hostname
hostnamectl

# Cambiar hostname
sudo hostnamectl set-hostname servidor.ejemplo.com

# Editar /etc/hosts
sudo vi /etc/hosts
# 192.168.1.100  servidor.ejemplo.com  servidor
```

---

## CAPÍTULO 19: SSH

### Conexión SSH Básica

```bash
# Conectar
ssh usuario@host
ssh usuario@192.168.1.100
ssh -p 2222 usuario@host       # Puerto no estándar

# Primera conexión (host key)
# Responder "yes" para guardar fingerprint

# Copiar archivos con scp
scp archivo.txt usuario@host:/ruta/destino/
scp usuario@host:/ruta/archivo.txt .
scp -r directorio usuario@host:/destino/    # Recursivo

# Alternativa: sftp
sftp usuario@host
```

### Autenticación por Clave Pública

#### Generar Par de Claves

```bash
# Generar clave SSH
ssh-keygen
# Default: ~/.ssh/id_rsa (privada) y ~/.ssh/id_rsa.pub (pública)

# Con parámetros específicos
ssh-keygen -t rsa -b 4096 -C "comentario"
ssh-keygen -t ed25519 -f ~/.ssh/mi_clave

# Proteger clave privada
chmod 600 ~/.ssh/id_rsa
```

#### Copiar Clave Pública al Servidor

```bash
# Método 1: ssh-copy-id (recomendado)
ssh-copy-id usuario@host

# Método 2: Manual
cat ~/.ssh/id_rsa.pub | ssh usuario@host \
  "mkdir -p ~/.ssh && cat >> ~/.ssh/authorized_keys"

# En servidor: verificar permisos
chmod 700 ~/.ssh
chmod 600 ~/.ssh/authorized_keys
```

#### Usar Clave Específica

```bash
ssh -i ~/.ssh/mi_clave usuario@host

# Configurar en ~/.ssh/config
Host servidor
    HostName 192.168.1.100
    User admin
    IdentityFile ~/.ssh/mi_clave
    Port 22

# Luego simplemente:
ssh servidor
```

### Configuración SSH Server

```bash
# Archivo: /etc/ssh/sshd_config

# Opciones importantes:
Port 22                        # Puerto SSH
PermitRootLogin no             # Deshabilitar login root
PasswordAuthentication yes     # Permitir password (no = solo keys)
PubkeyAuthentication yes       # Permitir autenticación por clave
AllowUsers user1 user2         # Solo estos usuarios

# Reiniciar servicio después de cambios
sudo systemctl restart sshd
```

### SSH Agent (Gestión de Claves)

```bash
# Iniciar ssh-agent
eval $(ssh-agent)

# Agregar clave
ssh-add ~/.ssh/id_rsa
ssh-add                        # Clave por defecto

# Listar claves cargadas
ssh-add -l

# Eliminar claves
ssh-add -D

# Agent forwarding (para saltar entre servidores)
ssh -A usuario@host
```

