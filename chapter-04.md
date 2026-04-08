# Capítulo 4: Registro de Sistemas para Soporte Red Hat

## Conceptos Clave

### Modelo de Suscripción Red Hat
- **CDN (Content Delivery Network)**: Red de entrega de contenido Red Hat
- **Simple Content Access (SCA)**: Modelo introducido en 2024
  - Suscripción a nivel de organización (no por sistema)
  - Simplifica gestión de suscripciones
  - Elimina necesidad de "adjuntar" suscripciones a cada sistema

### Consola de Gestión
- **Portal Anterior**: https://access.redhat.com (Customer Portal)
- **Portal Actual**: https://console.redhat.com (Hybrid Cloud Console)
  - Experiencia unificada de gestión de sistemas
  - Integración mejorada con Red Hat Insights

### Beneficios de Suscripción Activa
1. **Actualizaciones de software** - Parches de seguridad, correcciones, mejoras
2. **Soporte técnico** - Expertise de Red Hat
3. **Documentación** - Guías oficiales
4. **Red Hat Insights** - Análisis predictivo y resolución proactiva
5. **Customer Portal Labs** - Herramientas adicionales
6. **Knowledge Base** - Base de conocimientos

## Herramientas de Registro

### 1. rhc (Remote Host Configuration)

**Disponible**: RHEL 8.8 y posteriores

#### Características Predeterminadas (habilitadas por defecto)
- **content**: Acceso a repositorios
- **analytics**: Red Hat Insights
- **remote-management**: Gestión remota (servicio yggdrasil)

#### Comandos Básicos

```bash
# Registro completo (todas las características)
sudo rhc connect
# Solicita: Username y Password de Red Hat

# Registro sin Insights
sudo rhc connect --disable-feature=analytics,remote-management

# Registro con Activation Key
sudo rhc connect \
  --activation-key=MI_CLAVE \
  --organization=ID_ORGANIZACION

# Desregistrar sistema
sudo rhc disconnect
```

### 2. subscription-manager

**Disponible**: Todas las versiones RHEL

#### Comandos Básicos

```bash
# Registrar sistema
sudo subscription-manager register
# Solicita: Username y Password

# Registrar con Activation Key
sudo subscription-manager register \
  --activationkey NOMBRE_CLAVE \
  --org ID_ORGANIZACION

# Verificar estado de registro
sudo subscription-manager status

# Desregistrar sistema
sudo subscription-manager unregister
```

#### Configurar System Purpose (etiquetado de inventario)

```bash
# Configurar Role
sudo subscription-manager syspurpose role \
  --set 'Red Hat Enterprise Linux Server'

# Opciones Role:
# - Red Hat Enterprise Linux Server
# - Red Hat Enterprise Linux Workstation
# - Red Hat Enterprise Linux Compute Node

# Configurar Service-Level Agreement (SLA)
sudo subscription-manager syspurpose service-level \
  --set 'Premium'

# Opciones SLA:
# - Premium
# - Standard
# - Self-Support

# Configurar Usage
sudo subscription-manager syspurpose usage \
  --set 'Production'

# Opciones Usage:
# - Production
# - Development/Test
# - Disaster Recovery

# Listar opciones disponibles
sudo subscription-manager syspurpose usage --list
```

### 3. insights-client (para Red Hat Insights)

```bash
# Habilitar Red Hat Insights después de registrar con subscription-manager
sudo insights-client --register

# Verificar conexión
sudo insights-client --status
```

### 4. RHEL Web Console

**Acceso**: Interfaz web gráfica
- Navegar a **Subscriptions**
- Click en **Register**
- Ingresar credenciales o activation key
- Opción de conectar a Red Hat Insights

### 5. Durante la Instalación (Anaconda)

- Durante instalación de RHEL 10
- Sección: **Connect to Red Hat**
- Opciones:
  - Username/Password o Activation Key
  - System Purpose
  - Red Hat Insights

### 6. Kickstart (instalación automatizada)

```bash
# Comando en archivo Kickstart
rhsm --activation-key=MI_CLAVE --organization=MI_ORG
```

## Activation Keys

### ¿Qué son?
- Token de autenticación pre-compartido
- Método más seguro que username/password
- Ideal para automatización

### Creación
1. Acceder a Hybrid Cloud Console
2. **Insights for RHEL** → **Inventory** → **System Configuration** → **Activation Keys**
3. Crear nueva clave
4. Configurar:
   - Versión RHEL (última o específica)
   - System Purpose (opcional)
   - Repositorios adicionales (opcional)

### Uso

```bash
# Con rhc
sudo rhc connect \
  --activation-key=prod-key-2025 \
  --organization=1234567

# Con subscription-manager
sudo subscription-manager register \
  --activationkey prod-key-2025 \
  --org 1234567
```

## Certificados de Identidad

### Ubicaciones de Certificados

```bash
# Certificados de productos instalados
/etc/pki/product-default/

# Certificado de identidad del sistema
/etc/pki/consumer/

# Certificados de autorización (entitlement)
/etc/pki/entitlement/
```

### Inspeccionar Certificados

```bash
# Comando rct (Red Hat Certificate Tool)
sudo rct cat-cert /etc/pki/product-default/479.pem

# Salida muestra:
# - Product ID
# - Product Name
# - Version
# - Architecture
# - Start/End Date
```

### Tipos de Certificados

| Certificado | Ubicación | Propósito |
|-------------|-----------|-----------|
| **Product** | `/etc/pki/product-default/` | Identifica productos RHEL instalados |
| **Consumer** | `/etc/pki/consumer/` | Identifica el sistema |
| **Entitlement** | `/etc/pki/entitlement/` | Autenticación a repositorios |

## Ejemplos Prácticos

### Ejemplo 1: Registro Básico con rhc

```bash
# 1. Verificar si rhc está instalado
which rhc

# 2. Si no está instalado (en versiones anteriores a RHEL 10)
sudo dnf install rhc

# 3. Registrar sistema
sudo rhc connect

# Salida:
# Connecting host.example.com to Red Hat.
# Username: mi_usuario@redhat.com
# Password: ********
# [✔] Connected to Red Hat Subscription Management
# [✔] Content. Red Hat repository file generated
# [✔] Analytics. Connected to Red Hat Insights
# [✔] Remote Management. Activated yggdrasil service

# 4. Verificar en consola web
# https://console.redhat.com/insights/inventory
```

### Ejemplo 2: Registro con Activation Key

```bash
# 1. Crear activation key en Hybrid Cloud Console
# (hacer vía web interface)

# 2. Obtener Organization ID
# (disponible en consola web)

# 3. Registrar sistema
sudo rhc connect \
  --activation-key=prod-webservers-2025 \
  --organization=7654321

# 4. Verificar estado
sudo subscription-manager status
```

### Ejemplo 3: Registro Solo para Repositorios (sin Insights)

```bash
# Usando rhc sin analytics
sudo rhc connect --disable-feature=analytics,remote-management

# O usar subscription-manager directamente
sudo subscription-manager register

# Luego habilitar Insights más tarde si es necesario
sudo insights-client --register
```

### Ejemplo 4: Configurar System Purpose Post-Registro

```bash
# Ver valores actuales
sudo subscription-manager syspurpose show

# Configurar como servidor de producción
sudo subscription-manager syspurpose role \
  --set 'Red Hat Enterprise Linux Server'

sudo subscription-manager syspurpose service-level \
  --set 'Premium'

sudo subscription-manager syspurpose usage \
  --set 'Production'

# Verificar cambios
sudo subscription-manager syspurpose show
```

### Ejemplo 5: Verificar Estado de Registro

```bash
# Con subscription-manager
sudo subscription-manager status
# Salida:
# System Status: Registered

# Ver información del sistema
sudo subscription-manager identity
# Salida incluye:
# - system UUID
# - name
# - org ID

# Verificar Insights
sudo insights-client --status

# Verificar repositorios habilitados
sudo subscription-manager repos --list-enabled
```

### Ejemplo 6: Desregistrar y Re-registrar

```bash
# Desregistrar con rhc
sudo rhc disconnect

# O con subscription-manager
sudo subscription-manager unregister

# Re-registrar con método diferente
sudo subscription-manager register \
  --activationkey nueva-key \
  --org 7654321
```

## Comparación de Herramientas

| Característica | rhc | subscription-manager | Web Console |
|----------------|-----|----------------------|-------------|
| **Registra sistema** | ✓ | ✓ | ✓ |
| **Habilita repositorios** | ✓ | ✓ | ✓ |
| **Insights automático** | ✓ | ✗ (manual) | ✓ (opcional) |
| **Gestión remota** | ✓ | ✗ | ✗ |
| **Disponibilidad** | RHEL 8.8+ | Todas versiones | RHEL 7+ |
| **Interfaz** | CLI | CLI | Web GUI |

## Repositorios

### Repositorios Predeterminados
- **Habilitados automáticamente** después del registro
- Basados en productos instalados
- Proporcionan acceso inmediato a contenido

### Gestión de Repositorios

```bash
# Listar todos los repositorios
sudo subscription-manager repos --list

# Listar repositorios habilitados
sudo subscription-manager repos --list-enabled

# Habilitar repositorio específico
sudo subscription-manager repos \
  --enable=rhel-9-for-x86_64-baseos-rpms

# Deshabilitar repositorio
sudo subscription-manager repos \
  --disable=rhel-9-for-x86_64-supplementary-rpms
```

## Troubleshooting

### Problemas Comunes

```bash
# Error: "This system is not registered"
# Solución: Registrar el sistema
sudo rhc connect

# Error: "Unable to verify server's identity"
# Solución: Verificar conectividad a internet
ping subscription.rhsm.redhat.com

# Error: Activation key no válida
# Solución: Verificar nombre exacto de clave y org ID

# Error: Repositorios no disponibles
# Solución: Verificar estado de suscripción
sudo subscription-manager status
sudo subscription-manager refresh

# Limpiar cache y regenerar
sudo subscription-manager clean
sudo subscription-manager refresh
```

## Red Hat Insights

### ¿Qué es?
- Servicio de análisis predictivo
- Identifica problemas proactivamente
- Recomendaciones de seguridad y rendimiento
- Detección de configuraciones incorrectas

### Registro

```bash
# Con rhc (automático)
sudo rhc connect

# Con insights-client (después de subscription-manager)
sudo insights-client --register

# Verificar estado
sudo insights-client --status

# Forzar actualización de datos
sudo insights-client

# Desregistrar de Insights
sudo insights-client --unregister
```

## Comandos de Referencia Rápida

```bash
# === RHC ===
sudo rhc connect                          # Registro completo
sudo rhc connect --activation-key=KEY --organization=ORG
sudo rhc disconnect                       # Desregistro

# === SUBSCRIPTION-MANAGER ===
sudo subscription-manager register        # Registro
sudo subscription-manager status          # Estado
sudo subscription-manager identity        # Info del sistema
sudo subscription-manager unregister      # Desregistro
sudo subscription-manager repos --list    # Listar repos
sudo subscription-manager syspurpose show # Ver system purpose
sudo subscription-manager clean           # Limpiar cache
sudo subscription-manager refresh         # Actualizar

# === INSIGHTS ===
sudo insights-client --register           # Habilitar Insights
sudo insights-client --status             # Estado Insights
sudo insights-client                      # Actualizar datos
sudo insights-client --unregister         # Deshabilitar

# === RCT (Certificate Tool) ===
sudo rct cat-cert /etc/pki/product-default/479.pem  # Ver cert
```

## Puntos Clave para Examen

1. **rhc es la herramienta preferida** - RHEL 8.8+, incluye Insights automáticamente
2. **subscription-manager sigue siendo válido** - No está deprecado en RHEL 10
3. **SCA elimina attach de suscripciones** - Por sistema, ahora a nivel organización
4. **Activation keys son más seguras** - Mejor que username/password para automatización
5. **System Purpose es opcional** - Ayuda a etiquetar y filtrar inventario
6. **Tres tipos de certificados** - Product, Consumer, Entitlement
7. **Hybrid Cloud Console es nuevo portal** - https://console.redhat.com
8. **insights-client es independiente** - Se puede usar con subscription-manager
9. **Registro durante instalación** - Posible en Anaconda y Kickstart
10. **Repositorios por defecto se habilitan automáticamente** - Basados en producto instalado

## Flujo de Trabajo Típico

```bash
# Escenario 1: Sistema nuevo con rhc
sudo rhc connect
# → Automáticamente configura: repos, Insights, gestión remota

# Escenario 2: Sistema nuevo con subscription-manager + Insights
sudo subscription-manager register
sudo insights-client --register
# → Configuración manual en dos pasos

# Escenario 3: Automatización con activation key
sudo rhc connect \
  --activation-key=webservers-prod \
  --organization=1234567
# → No requiere interacción de usuario

# Escenario 4: Solo acceso a repositorios
sudo rhc connect --disable-feature=analytics,remote-management
# → Solo repositorios, sin Insights

# Verificación general después de cualquier registro
sudo subscription-manager status
sudo subscription-manager repos --list-enabled
```
