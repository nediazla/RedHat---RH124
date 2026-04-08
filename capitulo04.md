# Capítulo 4 – Registering Systems for Red Hat Support

## Objetivo del capítulo

Registrar un sistema RHEL con una cuenta de Red Hat para obtener acceso a:

- repositorios de software
- actualizaciones y parches de seguridad
- correcciones de errores
- documentación y recursos técnicos
- Red Hat Insights
- servicios de soporte

---

## 1. ¿Por qué registrar un sistema?

Red Hat distribuye contenido de software a través de la **Red Hat Content Delivery Network (CDN)**.

Cuando un sistema está registrado correctamente, puede:

- instalar paquetes desde repositorios oficiales
- recibir actualizaciones de seguridad
- mantenerse alineado con versiones soportadas
- acceder a recursos técnicos de Red Hat

### Puntos importantes

- El acceso depende de una suscripción activa.
- El registro asocia el sistema con una organización o cuenta de Red Hat.
- El sistema obtiene certificados de identidad y autenticación.

---

## 2. Simple Content Access (SCA)

El capítulo explica que Red Hat migró al modelo **Simple Content Access (SCA)**.

### ¿Qué simplifica SCA?

Antes, muchas operaciones giraban alrededor de adjuntar una suscripción a cada sistema individual.
Con SCA, la validación se hace **a nivel de organización**, no por adjuntar manualmente una suscripción por sistema.

### Qué debes recordar

- reduce la complejidad administrativa
- evita el viejo modelo de “attach subscription per host”
- centraliza la gestión en la **Hybrid Cloud Console**

---

## 3. Dónde se gestiona ahora

El material indica que varios servicios orientados al cliente se movieron desde el portal tradicional de Red Hat hacia la:

- **Red Hat Hybrid Cloud Console**

Allí puedes:

- revisar inventario de sistemas
- ver uso de suscripciones
- gestionar activation keys
- integrar Red Hat Insights

---

## 4. Métodos para registrar un sistema

El capítulo presenta varias formas de registrar RHEL:

1. `rhc`
2. `subscription-manager`
3. consola web de RHEL
4. durante la instalación con Anaconda
5. instalaciones automatizadas con Kickstart

---

## 5. Registro con `rhc` (método preferido)

El manual deja claro que **`rhc` es la herramienta preferida** para registrar sistemas modernos.

### Qué hace `rhc`

Por defecto puede habilitar estas funciones:

- `content`
- `analytics`
- `remote-management`

Esto significa que no solo da acceso a repositorios, sino que además puede conectar el sistema con **Red Hat Insights** y capacidades de gestión remota.

### Ejemplo básico

```bash
sudo rhc connect
```

Durante el proceso normalmente se solicita:

- usuario de Red Hat
- contraseña
- privilegios de root en el sistema

### Qué debes entender para el examen

- `rhc` es el método recomendado en RHEL recientes
- registra el sistema y habilita acceso a contenido
- puede activar también Insights
- requiere autenticación con una cuenta válida o activation key

---

## 6. Registro con activation key usando `rhc`

Para automatizar o endurecer seguridad, el capítulo recomienda usar una **activation key** junto con el identificador de organización.

### Ventaja

- no expones credenciales personales en cada registro
- es mejor para despliegues repetitivos
- ayuda en ambientes empresariales

### Ejemplo

```bash
sudo rhc connect --activation-key=<activation_key_name> --organization=<organization_ID>
```

### Idea clave

Para varios hosts, **activation key > usuario/contraseña**.

---

## 7. Desregistrar con `rhc`

Para desconectar el sistema de Red Hat:

```bash
sudo rhc disconnect
```

Esto elimina la conexión con:

- Subscription Management
- Insights
- remote management asociado

---

## 8. Registro con `subscription-manager`

Aunque `rhc` es preferido, el capítulo también enseña `subscription-manager`.

### Cuándo importa

- cuando `rhc` no está disponible en versiones más antiguas
- cuando solo quieres gestionar acceso a contenido
- cuando necesitas operaciones clásicas de suscripción

### Registro básico

```bash
sudo subscription-manager register
```

### Registro con activation key

```bash
sudo subscription-manager register --activationkey <activation_key_name> --org <organization_ID>
```

### Verificar estado

```bash
sudo subscription-manager status
```

Salida esperada:

```text
Overall Status: Registered
```

---

## 9. `subscription-manager` e Insights

El capítulo recalca algo importante:

- `subscription-manager` registra el sistema para acceso a contenido
- **no conecta por sí mismo el sistema a Red Hat Insights**

Para eso necesitas:

```bash
sudo insights-client --register
```

### Diferencia importante

- `rhc` puede registrar + conectar a Insights
- `subscription-manager` registra contenido, pero Insights se habilita aparte

---

## 10. System Purpose

El capítulo introduce **system purpose**, que es básicamente un mecanismo de etiquetado para clasificar hosts.

### Atributos que puedes definir

#### Role

- Red Hat Enterprise Linux Server
- Red Hat Enterprise Linux Workstation
- Red Hat Enterprise Linux Compute Node

#### Service-Level Agreement

- Premium
- Standard
- Self-Support

#### Usage

- Production
- Development/Test
- Disaster Recovery

### Ejemplos

```bash
sudo subscription-manager syspurpose role --set 'Red Hat Enterprise Linux Workstation'
sudo subscription-manager syspurpose service-level --set 'Premium'
sudo subscription-manager syspurpose usage --set 'Development/Test'
```

### Listar valores válidos

```bash
sudo subscription-manager syspurpose usage --list
```

### Qué debes recordar

System purpose **no es lo mismo** que registrar el sistema. Es metadato útil para inventario y clasificación.

---

## 11. Desregistrar con `subscription-manager`

```bash
sudo subscription-manager unregister
```

Esto elimina el registro del sistema.

---

## 12. Registro desde la consola web de RHEL

El capítulo también muestra que puedes registrar el sistema desde la **RHEL web console**.

### Idea general

- inicias sesión como usuario con privilegios
- vas a la sección de suscripciones
- puedes registrar con cuenta o activation key
- por defecto suele estar marcada la conexión a Insights

### Cuándo es útil

- administradores que prefieren GUI
- entornos de aprendizaje
- validación visual del estado de suscripción

---

## 13. Registro durante la instalación

En el instalador **Anaconda**, puedes registrar el sistema antes de instalar paquetes.

### Ventajas

- acceso temprano a contenido correcto
- instalación más alineada con la suscripción
- útil en despliegues iniciales

También se menciona que para instalaciones automatizadas con **Kickstart** puedes usar `rhsm`, normalmente con:

- activation key
- organization ID

---

## 14. Certificados de identidad

Un sistema registrado guarda certificados en `/etc/pki`.

### Ubicaciones importantes

#### Productos instalados

```bash
/etc/pki/product-default
```

Indican qué productos Red Hat están instalados.

#### Identidad del sistema

```bash
/etc/pki/consumer
```

Identifican el sistema.

#### Autenticación para repositorios

```bash
/etc/pki/entitlement
```

Autentican el sistema contra repositorios.

### Inspeccionar certificados

```bash
sudo rct cat-cert /etc/pki/product-default/479.pem
```

### Punto clave de examen

Si te preguntan qué directorio contiene los certificados de productos instalados, la respuesta es:

```bash
/etc/pki/product-default
```

---

## 15. Lo más importante para RHCSA

Este capítulo es más conceptual y operativo que de shell puro, pero sigue siendo importante.

### Debes entender muy bien

- para qué sirve registrar un sistema
- diferencia entre `rhc` y `subscription-manager`
- qué es SCA
- qué hace una activation key
- cómo verificar si el sistema está registrado
- qué relación tiene el registro con repositorios y actualizaciones
- qué hace `insights-client --register`

### Ojo para el examen

En muchos escenarios de laboratorio o examen, los repositorios pueden venir preconfigurados y quizá no tengas que registrar manualmente una máquina. Pero sí conviene entender:

- de dónde salen los paquetes oficiales
- por qué a veces un sistema no puede instalar software
- por qué `subscription-manager status` puede ser relevante

---

## 16. Errores comunes

### Error 1
Confundir **registro del sistema** con **instalación de paquetes**.

Registrar no instala software; solo habilita acceso y servicios relacionados.

### Error 2
Pensar que `subscription-manager` activa automáticamente Insights.

No. Para eso se usa:

```bash
sudo insights-client --register
```

### Error 3
Usar credenciales personales cuando en entorno corporativo conviene una activation key.

### Error 4
No distinguir entre:

- identidad del sistema
- acceso a repositorios
- inventario/metadatos de system purpose

---

## 17. Resumen express

- Red Hat usa suscripciones para dar acceso a repositorios y soporte.
- SCA simplifica la gestión al nivel de organización.
- `rhc` es la herramienta preferida en sistemas recientes.
- `subscription-manager` sigue siendo válido y útil.
- `insights-client --register` conecta a Red Hat Insights cuando hace falta.
- Las activation keys son la forma recomendada para automatización.
- Los certificados del sistema registrado viven bajo `/etc/pki`.

---

## 19. Comandos clave del capítulo

```bash
sudo rhc connect
sudo rhc connect --activation-key=<activation_key_name> --organization=<organization_ID>
sudo rhc disconnect
sudo subscription-manager register
sudo subscription-manager register --activationkey <activation_key_name> --org <organization_ID>
sudo subscription-manager status
sudo insights-client --register
sudo subscription-manager syspurpose role --set 'Red Hat Enterprise Linux Workstation'
sudo subscription-manager syspurpose service-level --set 'Premium'
sudo subscription-manager syspurpose usage --set 'Development/Test'
sudo subscription-manager syspurpose usage --list
sudo subscription-manager unregister
sudo rct cat-cert /etc/pki/product-default/479.pem
```

