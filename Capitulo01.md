# ### Capítulo 1 – Introduction to Red Hat Enterprise Linux

## Objetivo del capítulo

Definir y explicar:

- Open Source
- Linux
- Distribuciones Linux
- Red Hat Enterprise Linux (RHEL)
- Ecosistema RHEL (Fedora → CentOS Stream → RHEL)
- Opciones para obtener RHEL

---

# 1️. ¿Por qué aprender Linux?

Linux es una tecnología crítica en IT.

### 📌 Dónde se usa Linux

- Servidores web
- Infraestructura cloud (AWS, Azure, GCP)
- Contenedores (Docker, OpenShift)
- Supercomputadoras
- IoT
- Sistemas financieros
- Data centers empresariales

### Enfoque certificación

En RHCSA:

- Trabajarás principalmente en **servidores sin entorno gráfico**
- Todo se hace desde la **línea de comandos**
- Automatización y administración remota son clave

---

# 2️. ¿Qué hace grande a Linux?

## 1. Es Open Source

Puedes:

- Ver el código
- Modificarlo
- Redistribuirlo

Esto promueve:

- Innovación rápida
- Seguridad (puedes auditar código)
- Estabilidad a largo plazo

---

## 2. CLI poderosa (Command Line Interface)

Linux fue diseñado para ser administrado desde línea de comandos.

Esto permite:

- Automatización
- Scripting
- Administración remota
- Escalabilidad

💡 En RHCSA TODO gira alrededor de CLI.

---

## 3. Modularidad

Puedes:

- Instalar solo lo necesario
- Construir desde un servidor mínimo
- Convertirlo en appliance especializado

Ejemplo:  
Un servidor de producción puede no tener entorno gráfico instalado.

---

# 3️. ¿Qué es Open Source?

## Definición

Software cuyo código fuente puede:

- Usarse
- Estudiarse
- Modificarse
- Redistribuirse

---

## Open Source vs Propietario

|Open Source|Propietario|
|---|---|
|Código visible|Código cerrado|
|Puedes modificar|No puedes modificar|
|Comunidad contribuye|Solo el proveedor|

---

## Beneficios (Importante para examen)

✔ Control  
✔ Transparencia  
✔ Seguridad  
✔ Estabilidad  
✔ Aprendizaje real

---

# 4️. Tipos de Licencias Open Source

## Copyleft (ej: GPL)

Si modificas el código y lo redistribuyes:  
→ Debes mantenerlo open source.

Ejemplo:

- GNU GPL
- LGPL

---

## Permisivas (ej: MIT, BSD, Apache)

Puedes:

- Reutilizar código
- Integrarlo incluso en software propietario

---

## Punto clave examen

Open source ≠ gratuito  
Red Hat vende soporte, no licencia del código.

---

# 5️. ¿Qué es una Distribución Linux?

Linux no es solo el kernel.

## Kernel

El kernel:

- Administra CPU
- Administra memoria
- Administra dispositivos
- Administra procesos

Pero necesitas más componentes:

- GNU utilities
- Bibliotecas
- Sistema gráfico
- Gestor de paquetes

Una **distribución** empaqueta todo eso.

---

## Definición formal

Una distribución Linux es un sistema operativo instalable basado en:

- Linux kernel
- Programas de espacio de usuario
- Herramientas de administración
- Sistema de paquetes

---

## Ejemplos de distribuciones

- Fedora
- CentOS Stream
- RHEL
- Ubuntu
- Debian

---

# 6️. ¿Quién es Red Hat?

Red Hat:

- Es el mayor proveedor empresarial de software open source
- Proporciona soporte comercial
- Contribuye activamente a proyectos upstream

No "re-empaqueta" simplemente.  
Contribuye activamente al desarrollo.

---

# 7️. Ecosistema RHEL (MUY IMPORTANTE)

Este es uno de los conceptos más importantes del capítulo.

## Flujo de desarrollo:

```
Fedora → CentOS Stream → RHEL  
```

---

## Fedora

- Innovación rápida
- Releases cada ~6 meses
- Soporte corto (12-18 meses)
- Comunidad
- No empresarial

Es el laboratorio.

---

## CentOS Stream

- Desarrollo continuo
- Upstream directo de RHEL
- Base para futuras versiones menores
- Ciclo ~5 años

Es la "zona de integración".

---

## RHEL

- Producto comercial
- Soporte 10 años
- Certificaciones de hardware/software
- Equipo de seguridad dedicado
- Estabilidad empresarial

Es producción.

---

## Comparación clave (examen conceptual)

|Característica|Fedora|CentOS Stream|RHEL|
|---|---|---|---|
|Ciclo de vida|12-18 meses|5 años|10 años|
|Soporte oficial|No|No|Sí|
|Certificaciones|No|No|Sí|
|Seguridad empresarial|No|No|Sí|

---

# 8️. Red Hat Enterprise Linux (RHEL)

RHEL es:

- Distribución empresarial
- Basada en CentOS Stream
- Con soporte por suscripción

Red Hat cobra por:

- Soporte
- Actualizaciones
- Parches de seguridad
- Acceso a portal de conocimiento

---

# 9️. UBI (Universal Base Image)

UBI es:

- Imagen base redistribuible
- Derivada de RHEL
- Usada en contenedores

Muy relevante para entornos cloud.

---

# 10. RHEL CoreOS

Sistema:

- Basado en RHEL
- Orientado a contenedores
- Usado en OpenShift

No es sistema general.

---

# 1️1. Image Mode en RHEL 10

Nueva forma de desplegar RHEL:

- Basado en imágenes
- Usando contenedores (bootc)
- Reduce drift de configuración
- Ideal para edge computing

---

# 1️2. ¿Cómo obtener RHEL?

Opciones:

✅ Developer Subscription (GRATIS para aprendizaje)  
✅ Evaluación  
✅ Cloud providers (AWS, Azure, GCP)  
✅ Suscripción empresarial

