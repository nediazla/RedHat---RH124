# Resúmenes RH124 - Red Hat System Administration I

## Descripción
Resúmenes completos en español del curso **RH124 - Red Hat System Administration I** (RHEL 10.0), organizados por capítulos con conceptos clave, comandos y ejemplos prácticos.

## Estructura del Curso

### 📁 Resúmenes Disponibles

#### [Capítulo 1: Introducción a RHEL](chapter-01.md)
- Conceptos de Open Source y Linux
- Distribuciones Linux
- Ecosistema RHEL (Fedora, CentOS Stream, RHEL)
- Obtención de RHEL

#### [Capítulo 2: Acceso a la Línea de Comandos](chapter-02.md)
- Bash Shell
- Navegación en terminal
- Comandos básicos (ls, cd, pwd, cat, less, head, tail)
- SSH y conexión remota
- Historial de comandos y atajos

#### [Capítulo 3: Obteniendo Ayuda desde Documentación Local](chapter-03.md)
- Man pages (manual pages)
- Secciones del manual (1-9)
- Búsqueda de documentación (man -k, man -f, apropos, whatis)
- Navegación en man pages

#### [Capítulo 4: Registro de Sistemas para Soporte Red Hat](chapter-04.md)
- Modelo de suscripción Red Hat
- Simple Content Access (SCA)
- Herramientas: rhc, subscription-manager, insights-client
- Activation Keys
- Certificados de identidad

#### [Capítulo 5: Asistencia con IA - RHEL Lightspeed](chapter-05.md)
- Command-line assistant (`c chat`)
- Uso de IA para troubleshooting
- Comandos y ejemplos prácticos
- Limitaciones y advertencias

#### [Capítulos 6-10: File System, Archivos, Redirecciones, Usuarios](chapter-06-to-10.md)
**Cap 6: Jerarquía del Sistema de Archivos**
- Estructura de directorios Linux (/, /etc, /home, /var, /usr)
- Paths absolutos vs relativos
- Navegación (pwd, cd, ls)

**Cap 7: Gestión de Archivos**
- Comandos: mkdir, cp, mv, rm, touch
- Hard links y symbolic links
- Wildcards (*, ?, [])

**Cap 8: Editando Archivos de Texto**
- Editor Vim/Vi
- Modos de operación
- Comandos básicos de edición
- Guardar y salir

**Cap 9: Redirección de Entrada/Salida**
- Descriptores de archivo (stdin, stdout, stderr)
- Redirección (>, >>, 2>, &>, <)
- Pipes (|)
- Comando tee

**Cap 10: Gestión de Usuarios y Grupos**
- useradd, usermod, userdel
- groupadd, groupmod, groupdel
- passwd, chage
- su y sudo

#### [Capítulos 11-20: Permisos, Software, Procesos, Servicios, Redes](chapter-11-to-20.md)
**Cap 11: Control de Acceso (Permisos)**
- Permisos rwx (lectura, escritura, ejecución)
- chmod (simbólico y numérico)
- chown, chgrp
- Permisos especiales (setuid, setgid, sticky bit)
- umask

**Cap 12: Instalación y Actualización con RPM/DNF**
- Gestión de paquetes RPM
- DNF: install, update, remove, search
- Repositorios y configuración
- Grupos de paquetes

**Cap 13: Flatpak**
- Instalación de aplicaciones con Flatpak
- Comandos: install, list, update, run
- Repositorio Flathub

**Cap 14: Media Removible**
- Identificación de dispositivos (lsblk, blkid)
- Montar y desmontar file systems
- /etc/fstab
- Comando find

**Cap 15: Procesos Linux**
- Monitoreo: ps, top, htop
- Señales: kill, killall, pkill
- Job control: jobs, fg, bg
- Prioridad: nice, renice

**Cap 16: Servicios y Daemons (systemd)**
- systemctl: start, stop, restart, status
- enable, disable
- Targets y unit files
- systemctl list-units

**Cap 17-18: Networking**
- Configuración de red
- Comandos: ip, nmcli, ping, ss
- NetworkManager
- Hostname y DNS

**Cap 19: SSH**
- Conexión remota
- Autenticación por clave pública
- ssh-keygen, ssh-copy-id
- Configuración de servidor SSH

**Cap 20: Comprehensive Review**
- Laboratorios integradores
- Revisión de objetivos

## Comandos de Referencia Rápida

### Navegación y Archivos
```bash
pwd, cd, ls, mkdir, cp, mv, rm, touch, find, ln
```

### Ayuda y Documentación
```bash
man, man -k, apropos, whatis, --help
```

### Usuarios y Permisos
```bash
useradd, usermod, userdel, passwd, su, sudo
chmod, chown, chgrp, umask, id, groups
```

### Software
```bash
rpm -qa/-qi/-ql/-qf
dnf install/update/remove/search/info/list
flatpak install/list/update/run
```

### Procesos y Servicios
```bash
ps, top, kill, killall, nice, renice
systemctl start/stop/restart/status/enable/disable
```

### Red y Conectividad
```bash
ip addr/route, nmcli, ping, ss, ssh, scp
hostnamectl, ssh-keygen, ssh-copy-id
```

### Sistema de Archivos
```bash
mount, umount, lsblk, df, du, blkid
```

### Redirección y Pipes
```bash
>, >>, 2>, &>, <, |, tee
```

## Preparación para Certificación

### Temas Clave para RHCSA (EX200)
1. ✅ Navegación del sistema de archivos
2. ✅ Gestión de archivos y directorios
3. ✅ Permisos y ownership
4. ✅ Usuarios y grupos locales
5. ✅ Instalación de software (RPM/DNF)
6. ✅ Gestión de procesos
7. ✅ Control de servicios (systemd)
8. ✅ Configuración básica de red
9. ✅ SSH y acceso remoto
10. ✅ Redirección I/O y pipes

### Práctica Recomendada
- **Laboratorios**: Completar todos los labs del curso
- **Comando `lab`**: `lab start ejercicio` y `lab finish ejercicio`
- **Máquinas**: workstation, servera, serverb, serverc
- **Repetición**: Practicar comandos hasta dominarlos sin consultar
- **man pages**: Familiarizarse con documentación oficial

## Recursos Adicionales

### Documentación Oficial Red Hat
- [RHEL 10 Documentation](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/10/)
- [RHCSA Exam Objectives](https://www.redhat.com/en/services/training/ex200-red-hat-certified-system-administrator-rhcsa-exam)
- [Red Hat Customer Portal](https://access.redhat.com)
- [Hybrid Cloud Console](https://console.redhat.com)

### Cursos Relacionados
- **RH134**: Red Hat System Administration II
- **RH294**: Red Hat System Administration III (Automation with Ansible)

## Notas Importantes

### Convenciones del Manual
- 📖 **Referencias**: Documentación externa relevante
- 📝 **Nota**: Tips y atajos útiles
- ⚠️ **Importante**: Información fácil de pasar por alto
- 🚨 **Advertencia**: Ignorar puede causar problemas

### Sobre los Resúmenes
- **Idioma**: Español (con nombres técnicos en inglés preservados)
- **Formato**: Markdown para fácil lectura
- **Comandos**: Ejemplos prácticos incluidos
- **Enfoque**: Preparación para certificación RHCSA

## Autor y Licencia
Resúmenes creados para estudio de la certificación RH124.  
Basado en el manual oficial: **Red Hat System Administration I (RH124) - RHEL 10.0 Edition 4**

---

**📚 Última actualización**: Abril 2026  
**📌 Versión RHEL**: 10.0  
**🎯 Objetivo**: Certificación RHCSA (EX200)

---

## Tips para el Examen RHCSA

### ✅ Antes del Examen
- [ ] Dominar navegación de filesystem sin ayuda
- [ ] Memorizar permisos numéricos (755, 644, etc)
- [ ] Practicar vim hasta que sea segunda naturaleza
- [ ] Conocer systemctl de memoria
- [ ] Entender bien umask y permisos por defecto
- [ ] Practicar find con múltiples criterios
- [ ] Dominar redirecciones y pipes
- [ ] Saber usar man pages eficientemente

### ✅ Durante el Examen
- [ ] Leer todas las tareas antes de empezar
- [ ] Verificar cada tarea después de completarla
- [ ] Usar tab-completion extensivamente
- [ ] No desperdiciar tiempo en una tarea difícil
- [ ] Documentación está permitida (man pages)
- [ ] Reiniciar sistema cuando sea necesario para probar cambios persistentes

### ⚠️ Errores Comunes a Evitar
- ❌ No verificar que cambios son persistentes después de reboot
- ❌ Olvidar hacer `systemctl daemon-reload` después de editar units
- ❌ No usar `systemctl enable` para servicios que deben iniciar al boot
- ❌ Olvidar permisos correctos en archivos críticos (SELinux, SSH)
- ❌ No probar comandos antes de ejecutarlos en producción
- ❌ Perder tiempo buscando en Google (no está disponible en examen)

---

**¡Buena suerte con tu certificación RHCSA!** 🚀
