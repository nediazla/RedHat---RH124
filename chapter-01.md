# Capítulo 1: Introducción a Red Hat Enterprise Linux

## Conceptos Clave

### Linux y Open Source
- **Linux**: Sistema operativo UNIX-like desarrollado por Linus Torvalds en 1991
- **Kernel**: Núcleo del sistema que gestiona hardware, memoria y scheduling de procesos
- **Open Source**: Software con código fuente que cualquiera puede usar, estudiar, modificar y compartir

### Ventajas de Open Source
- **Control**: Ver y mejorar el código
- **Capacitación**: Aprender de código real
- **Seguridad**: Inspeccionar y corregir código sensible
- **Estabilidad**: El código sobrevive aunque el desarrollador original desaparezca

### Tipos de Licencias Open Source
1. **Copyleft (GPL, LGPL)**: Requieren que las modificaciones también sean open source
2. **Permisivas (MIT, BSD, Apache 2.0)**: Permiten uso incluso en software propietario

### Distribuciones Linux
- **Distribución Linux**: Sistema operativo instalable construido desde kernel Linux + programas de soporte
- Proporciona método fácil para instalar y gestionar sistema Linux completo

## Ecosistema Red Hat Enterprise Linux

### Componentes del Ecosistema

```
Fedora → CentOS Stream → RHEL
  ↓           ↓              ↓
~62K      ~8.1K          ~8.1K
packages  packages       packages
```

1. **Fedora**
   - Proyecto comunitario con innovaciones rápidas
   - Ciclo de vida: 12-18 meses
   - Lanzamientos cada 6 meses
   - Fuente de innovación para RHEL

2. **EPEL (Extra Packages for Enterprise Linux)**
   - Repositorio mantenido por Fedora SIG
   - Paquetes adicionales para RHEL no incluidos en soporte oficial

3. **CentOS Stream**
   - Desarrollo continuo para próxima versión RHEL
   - Ciclo de vida: 5 años
   - Disponible gratuitamente

4. **RHEL (Red Hat Enterprise Linux)**
   - Distribución comercial de grado producción
   - Ciclo de vida: 10 años
   - Soporte y certificaciones de seguridad
   - Modelo basado en suscripciones

### Productos Relacionados RHEL

| Producto | Descripción |
|----------|-------------|
| **RHEL CoreOS** | Host de contenedores para OpenShift |
| **UBI (Universal Base Image)** | Imagen base para contenedores derivada de RHEL |
| **Image Mode RHEL** | Despliegue basado en imágenes usando bootc |

## Obtención de RHEL

### Opciones Gratuitas
- **Fedora**: https://fedoraproject.org
- **CentOS Stream**: https://www.centos.org/centos-stream/
- **RHEL Evaluation**: Prueba limitada con soporte
- **Red Hat Developer Subscription**: Gratuita para desarrolladores individuales
  - Registro en: https://developer.redhat.com
  - Ideal para aprendizaje y desarrollo

### Opciones Comerciales
- **Suscripciones RHEL**: Con soporte comercial completo
- **Cloud Platforms**: AWS, Google Cloud, Azure con Red Hat Cloud Access
- **Container Platforms**: UBI para desarrollo sin suscripción

## Comparativa: Fedora vs CentOS Stream vs RHEL

| Característica | Fedora | CentOS Stream | RHEL |
|----------------|--------|---------------|------|
| **Ciclo de vida** | 12-18 meses | 5 años | 10 años |
| **Certificación de vendors** | No | Usualmente no | Sí |
| **Documentación** | Comunidad | Comunidad | Red Hat |
| **Soporte experto** | No | No | Sí |
| **Equipo de seguridad** | No | No | Sí |
| **Opciones sin costo** | Sí | Sí | Sí |
| **Herramientas gestión** | No | No | Sí |

## Arquitecturas Soportadas
- Intel/AMD x86_64
- ARM64
- IBM Power
- IBM Z

## Puntos Clave para Examen

1. **Open Source no significa "sin soporte comercial"** - Red Hat proporciona soporte comercial para software open source
2. **RHEL es open source pero con suscripción** - No cobra por licencias, cobra por soporte y servicios
3. **CentOS Stream es upstream de RHEL** - No downstream como era CentOS Linux
4. **Fedora es el laboratorio de innovación** - Las características se prueban aquí primero
5. **UBI permite desarrollo de contenedores sin suscripción** - Pero requiere plataforma Red Hat para soporte en producción

## Terminología Importante

- **Upstream**: Proyecto original del que deriva el código
- **Downstream**: Proyecto que se basa en otro proyecto upstream
- **Kernel**: Núcleo del sistema operativo
- **Distribution**: Sistema operativo completo empaquetado
- **Subscription**: Modelo de soporte basado en suscripciones
- **SCA (Simple Content Access)**: Modelo de suscripción que no requiere adjuntar licencias por sistema
