# Nessus-Step-by-Step-Gu-a-para-Principiantes
Este repositorio constituye una guía técnica detallada y práctica sobre el despliegue y uso de Tenable Nessus, la herramienta líder en la industria para la gestión de vulnerabilidades. El proyecto ha sido diseñado como un recurso de aprendizaje para profesionales que se inician en la seguridad ofensiva y la administración de sistemas seguros.

<div align="center">

# 🛡️ Análisis de Vulnerabilidades con Nessus Expert
### Guía completa de gestión de vulnerabilidades para entornos Linux · Módulo 4

---

![Tool](https://img.shields.io/badge/Tool-Nessus%20Expert-00B4E0?style=for-the-badge&logo=tenable&logoColor=white)
![Platform](https://img.shields.io/badge/Target-Metasploitable%202-critical?style=for-the-badge)
![OS](https://img.shields.io/badge/Attacker-Kali%20Linux-557C94?style=for-the-badge&logo=kalilinux&logoColor=white)
![Purpose](https://img.shields.io/badge/Purpose-Academic%20%2F%20M%C3%A1ster-informational?style=for-the-badge)
![Standard](https://img.shields.io/badge/Standard-CVSS%20v3.1-orange?style=for-the-badge)

</div>

---

> [!WARNING]
> Este repositorio tiene **fines exclusivamente académicos**. Todo el análisis se realizó sobre máquinas virtuales en un entorno de laboratorio controlado y aislado. Escanear sistemas sin autorización explícita es ilegal y constitutivo de delito.

---

## 📋 Tabla de Contenidos

- [Visión del Proyecto](#-visión-del-proyecto)
- [¿Por qué este proyecto?](#-por-qué-este-proyecto)
- [¿Qué es Nessus Expert?](#-qué-es-nessus-expert)
- [Diferencias entre versiones de Nessus](#-diferencias-entre-versiones-de-nessus)
- [Glosario de términos clave](#-glosario-de-términos-clave)
- [Instalación de Nessus Expert](#-instalación-de-nessus-expert)
- [Configuración del Entorno de Laboratorio](#-configuración-del-entorno-de-laboratorio)
- [Tipos de Escaneo explicados](#-tipos-de-escaneo-explicados)
- [Políticas de Escaneo aplicadas](#-políticas-de-escaneo-aplicadas)
- [Cómo leer e interpretar un reporte](#-cómo-leer-e-interpretar-un-reporte)
- [Hallazgos del Laboratorio](#-hallazgos-del-laboratorio)
- [Plan de Remediación](#-plan-de-remediación)
- [Estructura del Repositorio](#-estructura-del-repositorio)

---

## 🎓 Visión del Proyecto

> *"Como estudiante apasionado por la ciberseguridad, entiendo que la teoría es solo el primer paso. El análisis de vulnerabilidades es una de las habilidades más críticas en el mundo real. He creado este repositorio no solo para documentar mi progreso, sino también para que sirva de guía a otros compañeros que, como yo, están empezando a utilizar Nessus para securizar sus entornos. Mi meta es democratizar el conocimiento técnico y facilitar la curva de aprendizaje."*

En el panorama actual de la ciberseguridad, la superficie de ataque de las organizaciones crece de forma exponencial. La gestión de vulnerabilidades no es una tarea puntual, sino un **proceso cíclico y crítico**. Este proyecto se centra en las fases de **Descubrimiento de Información** y **Evaluación de Vulnerabilidades**, utilizando Nessus Expert como herramienta principal.

---

## ❓ ¿Por qué este Proyecto?

El motivo principal de este repositorio es demostrar capacidad técnica para:

- **Identificar proactivamente** fallos de seguridad antes de que sean explotados por agentes maliciosos.
- **Priorizar riesgos** basándose en métricas estándar como CVSS v3.1.
- **Cumplir con normativas**: marcos como NIST, ISO 27001 o ENS exigen escaneos de vulnerabilidades periódicos.
- **Entender la arquitectura**: aplicar seguridad sobre infraestructuras Linux y granjas de servidores.
- **Demostrar el ciclo completo**: desde el escaneo hasta la remediación verificada.

---

## 🔎 ¿Qué es Nessus Expert?

**Nessus**, desarrollado por **Tenable**, es el escáner de vulnerabilidades más utilizado en la industria de la ciberseguridad. Su funcionamiento se basa en una base de datos de más de **175.000 plugins** — pequeños programas especializados — que comprueban si un sistema presenta una debilidad conocida.

Nessus Expert es la versión más completa de la herramienta e incluye capacidades que las versiones inferiores no tienen: escaneo de infraestructura cloud, análisis de código de infraestructura como código (IaC) y exportación avanzada de reportes.

### ¿Qué nos permite hacer?

| Capacidad | Descripción |
|---|---|
| **Escaneo de redes** | Detecta qué dispositivos están activos y qué puertos tienen abiertos |
| **Auditoría de parches** | Comprueba si el sistema operativo y los servicios están actualizados |
| **Detección de malware** | Identifica señales de botnets, rootkits o procesos sospechosos |
| **Cumplimiento (Compliance)** | Verifica si la configuración sigue estándares como CIS Benchmarks |
| **Escaneo cloud** | Audita recursos en AWS, Azure y GCP *(exclusivo Expert)* |
| **Análisis IaC** | Detecta configuraciones inseguras en Terraform, CloudFormation... *(exclusivo Expert)* |

---

## 🆚 Diferencias entre versiones de Nessus

Antes de empezar a usar Nessus, es importante saber qué versión tienes disponible y qué puedes hacer con ella:

| Característica | Essentials | Professional | Expert |
|---|:---:|:---:|:---:|
| Precio | Gratuita | De pago | De pago (superior) |
| IPs escaneables | 16 IPs | Ilimitadas | Ilimitadas |
| Plugins disponibles | Todos | Todos | Todos |
| Exportación de reportes | Básica | Avanzada | Avanzada |
| Escaneo cloud (AWS/Azure/GCP) | ❌ | ❌ | ✅ |
| Análisis IaC (Terraform...) | ❌ | ❌ | ✅ |
| Soporte técnico prioritario | ❌ | ✅ | ✅ |
| Ideal para | Estudiantes / Lab | Pentesters profesionales | Equipos de seguridad empresarial |

> [!NOTE]
> Para este proyecto se utiliza **Nessus Expert**, lo que permite escanear un número ilimitado de IPs dentro del laboratorio y acceder a todas las capacidades de exportación y análisis avanzado.

---

## 📖 Glosario de Términos Clave

Antes de comenzar, es fundamental entender el vocabulario que Nessus utiliza. Este glosario está pensado para quien empieza desde cero.

---

### CVE — Common Vulnerabilities and Exposures

Es el **identificador único de una vulnerabilidad conocida** a nivel mundial. Tiene el formato `CVE-AÑO-NÚMERO`. Por ejemplo, `CVE-2021-41773` es la vulnerabilidad crítica de Apache que permite ejecución remota de comandos. Cuando Nessus reporta un hallazgo, siempre lo asocia a su CVE correspondiente si existe, lo que permite buscar información detallada en bases de datos como el NVD (National Vulnerability Database).

---

### CVSS — Common Vulnerability Scoring System

Es el **sistema de puntuación estándar internacional** que mide la gravedad de una vulnerabilidad en una escala del 0 al 10. No es un número inventado por Nessus — es un estándar calculado por FIRST.org que tiene en cuenta:

- **Vector de ataque:** ¿hay que estar físicamente o se puede explotar por red?
- **Complejidad:** ¿es trivial o requiere condiciones especiales?
- **Privilegios necesarios:** ¿necesita credenciales o es sin autenticación?
- **Impacto:** ¿afecta a confidencialidad, integridad o disponibilidad?

| Puntuación CVSS | Severidad | Color en Nessus |
|:---:|---|:---:|
| 0.0 | Informativo | 🔵 Azul |
| 0.1 – 3.9 | Bajo | 🟢 Verde |
| 4.0 – 6.9 | Medio | 🟡 Amarillo |
| 7.0 – 8.9 | Alto | 🟠 Naranja |
| 9.0 – 10.0 | Crítico | 🔴 Rojo |

---

### Plugin

Un plugin es un **pequeño programa dentro de Nessus** que realiza una comprobación específica. Hay plugins para detectar versiones antiguas de Apache, para verificar si SSH permite login como root, para comprobar si MySQL acepta contraseñas en texto plano, etc. Nessus tiene más de 175.000 plugins y los actualiza diariamente. Cada plugin tiene un ID numérico único — por ejemplo, Plugin ID `10881` corresponde a la detección del protocolo SSH versión 1.

---

### VPR — Vulnerability Priority Rating

Es la **puntuación propia de Tenable** que complementa al CVSS. Mientras que el CVSS mide el impacto técnico teórico, el VPR tiene en cuenta también si hay exploits activos circulando en la red en este momento y qué tan probable es que una vulnerabilidad sea explotada en la práctica. Va del 0 al 10. Un CVSSv3 de 7.5 puede tener un VPR de 4.0 si el exploit nunca se usa — o un VPR de 9.8 si hay una campaña activa. **El VPR es más útil para priorizar** que el CVSS solo.

---

### Credentialed vs. Uncredentialed Scan

- **Uncredentialed (sin credenciales):** Nessus escanea desde fuera, como un atacante externo. Solo ve lo que está expuesto en la red. Es menos exhaustivo pero refleja la visión real de un atacante.
- **Credentialed (con credenciales):** Nessus se autentica en el sistema via SSH (Linux) o WMI (Windows) y lo analiza desde dentro — versiones de paquetes instalados, archivos de configuración, políticas, etc. Es mucho más completo y preciso.

---

### False Positive

Un **falso positivo** ocurre cuando Nessus reporta una vulnerabilidad que en realidad no existe en ese sistema. Suele ocurrir cuando el plugin detecta una versión por el banner del servicio pero el administrador ya aplicó el parche sin actualizar el número de versión — práctica habitual en distribuciones Debian/Ubuntu. Saber identificarlos es una habilidad clave del analista.

---

### Asset

En Nessus, un **asset** es cualquier dispositivo o sistema escaneado: un servidor, una VM, un router, un contenedor. Nessus agrupa los hallazgos por asset, lo que permite ver de un vistazo qué máquinas son las más vulnerables.

---

## ⚙️ Instalación de Nessus Expert

### Requisitos previos

| Componente | Mínimo recomendado |
|---|---|
| Sistema operativo | Kali Linux / Ubuntu 20.04+ / Debian 11+ |
| RAM | 4 GB (8 GB recomendado) |
| Almacenamiento | 30 GB libres |
| CPU | 4 núcleos |
| Navegador | Chrome / Firefox (para la interfaz web) |
| Conectividad | Acceso a internet para activación y actualizaciones |

---

### Paso 1 — Obtener la licencia

1. Ir a [tenable.com/products/nessus](https://www.tenable.com/products/nessus) y seleccionar **Nessus Expert**.
2. Registrarse y completar el proceso de compra o solicitud de prueba gratuita.
3. Anotar el **Activation Code** que llegará por email — se necesitará en el paso 4.

---

### Paso 2 — Descargar el paquete

Desde la [página de descargas de Tenable](https://www.tenable.com/downloads/nessus), seleccionar el paquete para el sistema operativo. Para Kali Linux / Debian:

```bash
# Descargar el paquete .deb (sustituir X.X.X por la versión actual)
wget https://www.tenable.com/downloads/api/v1/public/pages/nessus/downloads/XXXXX/get \
     -O Nessus-X.X.X-debian10_amd64.deb
```

> [!TIP]
> Es más cómodo descargarlo directamente desde el navegador en la página de Tenable, que siempre muestra la versión más reciente.

---

### Paso 3 — Instalar el paquete

```bash
# Instalar el paquete descargado
sudo dpkg -i Nessus-X.X.X-debian10_amd64.deb

# Iniciar el servicio de Nessus
sudo systemctl start nessusd

# Habilitar el inicio automático con el sistema
sudo systemctl enable nessusd

# Verificar que el servicio está activo
sudo systemctl status nessusd
```

---

### Paso 4 — Activar desde el navegador

1. Abrir: `https://localhost:8834`
2. Aceptar el aviso de certificado autofirmado.
3. Seleccionar **Nessus Expert** en la pantalla de bienvenida.
4. Introducir el **Activation Code** recibido por email.
5. Crear el usuario administrador.
6. Esperar a que Nessus descargue todos los plugins — puede tardar entre **10 y 30 minutos**.

> [!IMPORTANT]
> Durante la descarga de plugins, **no cerrar el navegador ni reiniciar el servicio**. Si se interrumpe, ejecutar `sudo systemctl restart nessusd` y acceder de nuevo a la interfaz para retomarlo.

---

### Paso 5 — Verificar la instalación

```
Menú superior → tu usuario → About
→ Debe mostrar: "Nessus Expert" con la fecha de expiración de la licencia
```

> [!TIP]
> 📸 **Captura recomendada:** pantalla de instalación completada y pantalla *About* confirmando la versión Expert.

---

## 🧪 Configuración del Entorno de Laboratorio

Todo el entorno corre sobre **VirtualBox** o **VMware**, con ambas máquinas en la misma red privada para que puedan comunicarse sin exponer tráfico al exterior.

### Arquitectura de red

```
┌──────────────────────────────────────────────────────────┐
│               Red Interna / NAT Network                  │
│                    192.168.1.0/24                        │
│                                                          │
│  ┌──────────────────┐         ┌─────────────────────┐   │
│  │   Kali Linux     │         │   Metasploitable 2  │   │
│  │   (Analista)     │◄───────►│   (Target)          │   │
│  │   192.168.1.10   │         │   192.168.1.50      │   │
│  │                  │         │                     │   │
│  │  Nessus Expert   │         │  Apache  2.2.8      │   │
│  │  :8834           │         │  MySQL   5.0        │   │
│  └──────────────────┘         │  vsftpd  2.3.4      │   │
│                               │  OpenSSH 4.7        │   │
│                               └─────────────────────┘   │
└──────────────────────────────────────────────────────────┘
```

### Máquina Atacante — Analista

| Parámetro | Valor |
|---|---|
| **Sistema Operativo** | Kali Linux 2023.x |
| **Herramienta** | Nessus Expert v10.x |
| **Adaptador de red** | NAT Network / Red Interna |
| **IP** | `192.168.1.10` |
| **Interfaz Nessus** | `https://localhost:8834` |

### Máquina Objetivo — Target

| Parámetro | Valor |
|---|---|
| **Sistema Operativo** | Metasploitable 2 (Ubuntu 8.04) |
| **Servicios activos** | Apache 2.2.8, MySQL 5.0, vsftpd 2.3.4, OpenSSH 4.7 |
| **Adaptador de red** | NAT Network / Red Interna |
| **IP** | `192.168.1.50` |

> [!NOTE]
> **¿Por qué Metasploitable 2?** Es una máquina virtual diseñada específicamente para ser vulnerable, usada en entornos de aprendizaje. Viene con servicios desactualizados y configuraciones inseguras que permiten practicar sin riesgo legal ni ético. Descargable en: [sourceforge.net/projects/metasploitable](https://sourceforge.net/projects/metasploitable/)

### Verificación de conectividad

```bash
# Desde Kali Linux — comprobar visibilidad del target
ping -c 4 192.168.1.50
# Resultado esperado: 0% packet loss

# Verificar puertos abiertos antes de Nessus (referencia previa)
nmap -sV 192.168.1.50
```

> [!TIP]
> 📸 **Captura recomendada:** resultado del `ping` y del `nmap` confirmando conectividad antes de lanzar Nessus.

---

## 🗂 Tipos de Escaneo explicados

Nessus Expert ofrece múltiples tipos de escaneo. Elegir el correcto es tan importante como configurarlo bien.

---

### 🔵 Basic Network Scan
El escaneo más equilibrado y el punto de partida recomendado. Realiza descubrimiento de hosts, identifica servicios y puertos abiertos, y comprueba vulnerabilidades conocidas en los servicios detectados. Es el equivalente a un chequeo médico general — amplio pero no invasivo.

**Cuándo usarlo:** primer análisis de un sistema desconocido o cuando se quiere un panorama general rápido.

---

### 🔴 Advanced Scan
Versión totalmente configurable del Basic Network Scan. Permite ajustar manualmente cada parámetro: qué plugins activar, qué puertos escanear, si usar credenciales, límites de rendimiento, etc. Da control total pero requiere más conocimiento para configurarlo correctamente.

**Cuándo usarlo:** cuando se necesita un escaneo muy específico adaptado a un entorno concreto.

---

### 🟢 Credentialed Patch Audit
Escaneo autenticado que se conecta al sistema via SSH (Linux) o WMI (Windows) y comprueba qué paquetes están instalados contra una base de datos de parches. Detecta software desactualizado aunque el servicio no esté expuesto en red.

**Cuándo usarlo:** cuando se tienen credenciales del sistema y se quiere el inventario más completo posible de vulnerabilidades, no solo las visibles desde fuera.

---

### 🟡 Web Application Tests
Escanea aplicaciones web en busca de vulnerabilidades OWASP: SQL Injection, XSS, CSRF, directorios expuestos, etc. Envía payloads de prueba contra los formularios y endpoints.

**Cuándo usarlo:** cuando el objetivo tiene una aplicación web y se quiere analizar la capa de aplicación.

> [!WARNING]
> Este tipo de escaneo puede provocar comportamientos inesperados en la aplicación (formularios enviados, registros creados). Usarlo **solo en entornos controlados**.

---

### 🟠 Malware Scan
Busca señales de compromiso: procesos sospechosos, archivos con firmas de malware, conexiones a dominios de C2 (Command & Control), modificaciones en archivos del sistema. Requiere credenciales para ser efectivo.

**Cuándo usarlo:** cuando se sospecha que un sistema ya ha sido comprometido.

---

### 🔵 Compliance / Policy Compliance
Compara la configuración del sistema contra un estándar de seguridad (CIS Benchmarks, DISA STIG, PCI-DSS...) y reporta qué controles se cumplen y cuáles no. No busca vulnerabilidades técnicas sino errores de configuración respecto a buenas prácticas.

**Cuándo usarlo:** en auditorías de cumplimiento normativo o para verificar el hardening de un servidor.

---

### Tabla resumen

| Tipo de escaneo | Credenciales | Agresividad | Mejor para |
|---|:---:|:---:|---|
| Basic Network Scan | Opcional | Baja | Primer análisis general |
| Advanced Scan | Opcional | Configurable | Análisis personalizado |
| Credentialed Patch Audit | ✅ | Media | Inventario completo de parches |
| Web Application Tests | No | Alta | Aplicaciones web |
| Malware Scan | ✅ | Media | Detección de compromiso |
| Compliance | ✅ | Baja | Auditorías normativas |

---

## 🔧 Políticas de Escaneo aplicadas

En este proyecto se configuró una política personalizada basada en **Basic Network Scan** con las siguientes modificaciones específicas:

### Configuración general

- **Scan Type:** Long — escaneo de los **65.535 puertos TCP** para no dejar ningún servicio sin detectar.
- **Detección de servicios:** activada la búsqueda de **banners** para identificar versiones exactas de Apache, MySQL y vsftpd.
- **Service Detection:** habilitado en todos los puertos, no solo en los bien conocidos (well-known ports).

### Enumeración de usuarios SSH

Se habilitó la **enumeración de usuarios SSH** en la pestaña *Settings → Assessment*. Esto permite a Nessus verificar si el servidor SSH revela si un nombre de usuario es válido o no mediante diferencias en los tiempos de respuesta — un vector previo crítico en ataques de fuerza bruta.

### Consideraciones de rendimiento

Para no saturar la CPU de Metasploitable, que tiene recursos muy limitados, se redujo el número de **comprobaciones simultáneas a 5** en *Settings → Performance*. Sin este ajuste, Nessus puede enviar tantas peticiones simultáneas que el objetivo deje de responder y el escaneo no se complete.

> [!TIP]
> 📸 **Captura recomendada:** pantalla de configuración de la política en Nessus mostrando los ajustes de puertos, detección de servicios y rendimiento.

---

## 📊 Cómo leer e interpretar un reporte

Saber lanzar un escaneo es solo la mitad del trabajo. La parte más importante para un analista es saber leer lo que Nessus devuelve.

### El código de colores

| Color | Severidad | CVSS | Qué significa en la práctica |
|:---:|---|:---:|---|
| 🔴 | **Crítico** | 9.0 – 10.0 | Existe un exploit público. Un atacante puede tomar control sin credenciales. **Acción inmediata.** |
| 🟠 | **Alto** | 7.0 – 8.9 | El atacante puede obtener privilegios elevados o acceder a datos sensibles. **Acción urgente.** |
| 🟡 | **Medio** | 4.0 – 6.9 | Fallos de configuración o versiones antiguas que facilitan ataques más complejos. **Planificar remediación.** |
| 🟢 | **Bajo** | 0.1 – 3.9 | Información que ayuda al atacante en reconocimiento. Bajo impacto directo. |
| 🔵 | **Info** | 0.0 | Datos informativos: puertos abiertos, certificados, configuración detectada. No es una vulnerabilidad. |

---

### Anatomía de un hallazgo

Cada hallazgo en Nessus tiene la siguiente estructura. Entender cada campo es clave para priorizar correctamente:

```
┌──────────────────────────────────────────────────────────────┐
│  Plugin Name:  Apache HTTP Server < 2.4.50 Multiple Vulns    │
│  Plugin ID:    153584                                        │
│  Severity:     🔴 CRITICAL                                   │
│  CVE:          CVE-2021-41773, CVE-2021-42013                │
│  CVSS v3:      9.8                                           │
│  VPR Score:    9.3                                           │
├──────────────────────────────────────────────────────────────┤
│  Synopsis:     El servidor Apache remoto está afectado       │
│                por múltiples vulnerabilidades críticas.       │
│                                                              │
│  Description:  La versión instalada es anterior a 2.4.50     │
│                y está afectada por Path Traversal y RCE      │
│                con mod_cgi activo.                           │
│                                                              │
│  Solution:     Actualizar Apache a versión 2.4.50 o          │
│                superior.                                      │
│                                                              │
│  See Also:     https://httpd.apache.org/security/            │
│                https://nvd.nist.gov/vuln/detail/CVE-2021...  │
├──────────────────────────────────────────────────────────────┤
│  Port:         80/tcp                                        │
│  Protocol:     TCP                                           │
│  Service:      www                                           │
└──────────────────────────────────────────────────────────────┘
```

**Cómo leer este hallazgo:**
1. **Plugin Name + CVE** — identifica exactamente qué vulnerabilidad es y permite buscarla en el NVD.
2. **CVSS 9.8 + VPR 9.3** — ambas puntuaciones altas confirman que es real, activamente explotada y de impacto crítico.
3. **Description** — explica el problema: path traversal + RCE si `mod_cgi` está activo.
4. **Solution** — dice exactamente qué hay que hacer para arreglarlo.
5. **Port 80/tcp** — el fallo está en el servicio web en el puerto 80.

---

### Priorización: ¿por dónde empezar?

Con un objetivo como Metasploitable, Nessus puede reportar decenas de hallazgos. El orden correcto es:

```
1. 🔴 Críticos con VPR alto y exploit público conocido  → Remediar de inmediato
2. 🔴 Críticos sin exploit público                      → Remediar esta semana
3. 🟠 Altos en servicios expuestos externamente         → Remediar este mes
4. 🟡 Medios                                            → Backlog de seguridad
5. 🟢 Bajos + 🔵 Info                                  → Documentar, sin urgencia
```

> [!TIP]
> 📸 **Captura recomendada:** vista del dashboard de Nessus con el resumen de hallazgos por severidad y el gráfico de distribución.

---

## 🔬 Hallazgos del Laboratorio

**Escenario:** Auditoría de servidor web con arquitectura LAMP sobre Metasploitable 2.

### Resumen del escaneo

| Severidad | Cantidad detectada |
|:---:|:---:|
| 🔴 Crítico | *(completar con captura)* |
| 🟠 Alto | *(completar con captura)* |
| 🟡 Medio | *(completar con captura)* |
| 🟢 Bajo | *(completar con captura)* |
| 🔵 Info | *(completar con captura)* |

> [!TIP]
> 📸 **Captura recomendada:** pantalla de resumen del escaneo con el gráfico de distribución por severidad.

---

### Hallazgos principales

| Plugin Name | Severidad | CVE | ¿Qué significa? | Acción recomendada |
|---|:---:|---|---|---|
| **Apache HTTP Server RCE** | 🔴 Crítico | CVE-2021-41773 | Un atacante puede ejecutar comandos en el servidor sin credenciales mediante Path Traversal con mod_cgi activo. | `sudo apt-get install --only-upgrade apache2` |
| **vsftpd 2.3.4 Backdoor** | 🔴 Crítico | CVE-2011-2523 | Esta versión tiene una backdoor deliberada que abre una shell en el puerto 6200 al enviar `:)` como usuario FTP. | Desinstalar vsftpd 2.3.4 e instalar una versión limpia. |
| **MySQL EOL (End of Life)** | 🟠 Alto | — | MySQL 5.0 llegó a fin de vida en 2012 y no recibe parches. Cualquier vulnerabilidad descubierta hoy no será corregida. | Migrar a MySQL 8.x o MariaDB 10.x. |
| **MySQL Cleartext Auth** | 🟠 Alto | — | Las credenciales viajan por la red sin cifrar. Un atacante en la misma red puede capturarlas con Wireshark. | Activar SSL/TLS: `require_secure_transport=ON`. |
| **OpenSSH < 7.0** | 🟡 Medio | CVE-2016-0777 | Versión vulnerable a filtraciones de claves privadas por el cliente SSH. | `sudo apt-get install --only-upgrade openssh-server` |
| **SSH Root Login Enabled** | 🟡 Medio | — | Permite intentos de login directo como `root`, facilitando ataques de fuerza bruta al usuario más privilegiado. | `PermitRootLogin no` en `/etc/ssh/sshd_config` |

> [!TIP]
> 📸 **Capturas recomendadas:** detalle de cada hallazgo crítico en Nessus mostrando descripción, CVE, CVSS y solución.

---

## 🩹 Plan de Remediación

Acciones tomadas para corregir los hallazgos y verificación mediante **re-scan** posterior.

---

### Acción 1 — Actualización de Apache

```bash
sudo apt-get update
sudo apt-get install --only-upgrade apache2

# Verificar que la versión ha cambiado
apache2 -v
```

**Verificación:** re-scan → el plugin `Apache HTTP Server RCE` desaparece de los resultados.

---

### Acción 2 — Desactivar login root en SSH

```bash
# Editar la configuración
sudo nano /etc/ssh/sshd_config

# Modificar:
# PermitRootLogin yes  →  PermitRootLogin no

# Aplicar cambios
sudo systemctl restart sshd
```

**Verificación:** re-scan → `SSH Root Login Enabled` desaparece.

---

### Acción 3 — Cerrar puertos innecesarios con iptables

```bash
# Bloquear acceso externo al puerto de MySQL
sudo iptables -A INPUT -p tcp --dport 3306 -j DROP

# Verificar la regla
sudo iptables -L INPUT -n -v

# Hacer la regla persistente
sudo iptables-save > /etc/iptables/rules.v4
```

**Verificación:** re-scan → el puerto 3306 ya no aparece expuesto en los hallazgos.

---

### Acción 4 — Control de tráfico con tc (QDISC HTB)

```bash
# Limitar el ancho de banda para mitigar riesgo de DoS
sudo tc qdisc add dev eth0 root handle 1: htb default 30
sudo tc class add dev eth0 parent 1: classid 1:1 htb rate 100mbit
sudo tc class add dev eth0 parent 1:1 classid 1:30 htb rate 10mbit ceil 100mbit

# Verificar que el qdisc está activo
sudo tc qdisc show dev eth0
```

---

### Resultado del re-scan

Tras aplicar todas las acciones de remediación, se realizó un nuevo escaneo completo. Los hallazgos críticos relacionados con Apache, la backdoor de vsftpd corregida, SSH y la exposición de MySQL desaparecieron del reporte, confirmando la efectividad de las medidas aplicadas.

> [!TIP]
> 📸 **Captura recomendada:** comparativa lado a lado del primer escaneo (con hallazgos críticos) y el re-scan (resueltos).

---

## 📁 Estructura del Repositorio

```text
.
├── 📂 assets/
│   ├── 📂 screenshots/               # Capturas del proceso completo
│   │   ├── 01_nessus_install.png
│   │   ├── 02_lab_connectivity.png
│   │   ├── 03_scan_config.png
│   │   ├── 04_scan_results_overview.png
│   │   ├── 05_finding_apache_rce.png
│   │   ├── 06_finding_vsftpd_backdoor.png
│   │   ├── 07_finding_ssh_root.png
│   │   └── 08_rescan_after_remediation.png
│   └── 📂 diagrams/                  # Diagramas de arquitectura de red
│       └── lab_network.png
│
├── 📂 docs/
│   ├── installation-guide.md         # Guía detallada de instalación de Nessus Expert
│   ├── scan-policies.md              # Políticas de escaneo configuradas
│   └── scan-types-reference.md       # Referencia de tipos de escaneo
│
├── 📂 reports/
│   ├── nessus-report-initial.pdf     # Reporte del primer escaneo
│   ├── nessus-report-rescan.pdf      # Reporte del re-scan post-remediación
│   └── remediation-plan.md          # Plan de remediación detallado
│
├── 📂 lab-environment/
│   ├── network-config.md             # Configuración de red de las VMs
│   └── metasploitable-setup.md       # Guía de configuración de Metasploitable 2
│
└── 📄 README.md                      # Este archivo
```

---

<div align="center">

---

*Desarrollado con fines académicos · Módulo 4 — Seguridad en Arquitecturas Linux*
*Herramienta: [Nessus Expert — Tenable](https://www.tenable.com/products/nessus)*

</div>
