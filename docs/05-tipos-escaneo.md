 🗂 Tipos de Escaneo, Configuración y Ejecución

← [Volver al índice](../README.md)

---

Este documento cubre todo lo necesario para trabajar con los escaneos de Nessus: qué tipos existen, cómo se configura cada uno, cómo se navega por la interfaz y cómo se ejecuta un escaneo completo de principio a fin — tal y como se realizó en este proyecto sobre **Windows 11**.

---

## Tabla de Contenidos

- [Interfaz de Nessus — Pestañas y navegación](#interfaz-de-nessus--pestañas-y-navegación)
- [Tipos de escaneo disponibles](#tipos-de-escaneo-disponibles)
- [Configuración detallada de un escaneo](#configuración-detallada-de-un-escaneo)
- [Cómo realizar un escaneo paso a paso](#cómo-realizar-un-escaneo-paso-a-paso)
- [Análisis de Malware](#análisis-de-malware)

---

## Interfaz de Nessus — Pestañas y navegación

Antes de lanzar el primer escaneo, es importante conocer la interfaz web de Nessus (`https://localhost:8834`) y para qué sirve cada sección.

![Instalacion Nessus](/assets/instalacion_30.png)

### Menú My Scans

| Sección | Descripción |
|---|---|
| **My Scans** | Vista personal. Desde aquí se crean carpetas por proyecto y se lanzan nuevos escaneos con **New Scan**. |
| **All Scans** | Todos los escaneos de la instancia, independientemente de quién los creó. Útil en equipos compartidos. |
| **Trash** | Escaneos eliminados — funcionan como papelera de reciclaje antes de borrarse definitivamente. |
| **Policies** | Plantillas de configuración reutilizables. Se define una vez y se aplica en múltiples escaneos. |
| **Plugin Rules** | Reglas personalizadas sobre plugins — por ejemplo, forzar que un plugin siempre aparezca como Info si es un falso positivo recurrente en tu entorno. |
| **Customized Reports** | Plantillas de informe personalizadas para exportar en el formato de la organización. |
| **Web App Scanning** | Sección dedicada al escaneo de aplicaciones web (targets como URLs, no IPs). |

> [!TIP]
> Crear una **carpeta de proyecto** antes de lanzar el primer escaneo evita tener todos los resultados mezclados. `My Scans → New Folder`.

![Instalacion Nessus](/assets/carpeta.png)


### Menú Settings

| Sección | Descripción |
|---|---|
| **About** | Versión instalada, estado de la licencia (tipo, expiración, límite de IPs) y número de plugins. Permite descargar logs del sistema. |
| **Advanced** | Parámetros avanzados del motor: SSL/TLS, lista de cifrados, valores por defecto globales de rendimiento. Para uso administrativo avanzado — no modificar en laboratorio. |
| **User Interface** | Preferencias visuales: idioma, zona horaria, opciones de visualización. |

![About](/assets/about.png)

### Dónde buscar plugins

Cada hallazgo de Nessus tiene asociado un **Plugin ID**. Para consultar su descripción técnica detallada hay dos recursos oficiales:

| URL | Para qué sirve |
|---|---|
| [tenable.com/plugins/families/about](https://www.tenable.com/plugins/families/about) | Ver cómo están **agrupados** los plugins por familias temáticas (Windows, Web Servers, Databases, Backdoors...). Útil para saber qué familia activar o desactivar según el tipo de target. |
| [tenable.com/plugins](https://www.tenable.com/plugins) | **Buscador individual** por Plugin ID, nombre o CVE. Al hacer clic en un plugin muestra su descripción técnica, severidad, CVEs asociados y solución recomendada. |

---

## Tipos de escaneo disponibles

Al hacer clic en **New Scan** se abre la galería de plantillas organizada en tres categorías: **Discovery**, **Vulnerabilities** y **Compliance**.

![Instalacion Nessus](/assets/escaneos.png)

---

### DISCOVERY

#### 🔵 Host Discovery
Mapea qué hosts están activos en la red y qué puertos tienen abiertos, sin buscar vulnerabilidades. Es el primer paso antes de un análisis de seguridad.

**Cuándo usarlo:** cuando se quiere hacer un reconocimiento inicial rápido de la red antes de decidir qué hosts analizar en profundidad.

![Instalacion Nessus](/assets/Host_discovery.png)


#### 🔵 Attack Surface Discovery
Descubre la superficie de ataque externa de una organización — dominios, subdominios, IPs y servicios expuestos a internet.

#### 🔵 Ping-Only Discovery
Detecta qué hosts responden a ping usando mínimo tráfico de red.

---

### VULNERABILITIES

#### 🔵 Basic Network Scan
El escaneo más equilibrado y el **punto de partida recomendado**. Detecta hosts, servicios, versiones y vulnerabilidades conocidas sin configuración avanzada.

**Cuándo usarlo:** primer análisis de un sistema desconocido o escaneos rutinarios de mantenimiento.

**Diferencia clave con Advanced:** en Basic Network Scan los plugins están predefinidos por Tenable y no se pueden modificar individualmente. Las credenciales sí se pueden añadir.

#### 🔴 Advanced Scan
Plantilla en blanco con control total sobre todos los parámetros. Activa la pestaña **Advanced** de configuración que no está disponible en Basic.

**Cuándo usarlo:** cuando se necesita un escaneo quirúrgico, en entornos críticos donde hay que controlar el impacto en la red, o cuando se quiere optimizar para granjas de servidores.

**En este proyecto:** se usó Advanced Scan para el escaneo principal del laboratorio.

#### 🟢 Credentialed Patch Audit
Se autentica en el sistema via SSH (Linux) o WMI (Windows) y comprueba qué parches están aplicados comparándolos contra la base de datos de Tenable.

**Cuándo usarlo:** cuando se tienen credenciales y se quiere el inventario más completo — detecta software desactualizado aunque no esté expuesto en red.

**Requisito obligatorio:** credenciales con privilegios suficientes.

#### 🟡 Web Application Tests
Busca vulnerabilidades OWASP en aplicaciones web: SQL Injection, XSS, CSRF, directorios expuestos, cabeceras de seguridad ausentes. Envía payloads de prueba activos.

> [!WARNING]
> Puede provocar comportamientos inesperados en la aplicación (formularios enviados, registros creados). Usar **solo en entornos controlados**.

#### 🟠 Malware Scan
Busca señales de compromiso activo: hashes de malware conocido, reglas YARA, conexiones a C2, archivos sospechosos. Requiere credenciales.

→ Ver sección completa: [Análisis de Malware](#análisis-de-malware)

#### 🔵 Advanced Dynamic Scan
Configura un plugin dinámico usando recomendaciones sin necesitar configuración manual.

#### 🔵 Mobile Device Scan
Audita dispositivos móviles vía Microsoft Exchange o MDM.

#### 🔵 Find AI
Detecta servicios de IA, LLM, GPUs y vulnerabilidades asociadas.

#### 🔵 Remote Monitoring and Management
Detecta herramientas RMM (Remote Monitoring and Management) y vulnerabilidades asociadas.

#### 🔵 Cryptographic Inventory
Identifica y clasifica los algoritmos criptográficos en uso en la red.

---

### COMPLIANCE

| Plantilla | Para qué sirve |
|---|---|
| **Audit Cloud Infrastructure** | Audita la configuración de servicios cloud de terceros |
| **Internal PCI Network Scan** | Escaneo PCI interno de red |
| **MDM Config Audit** | Configuración de dispositivos móviles |
| **Offline Config Audit** | Audita configuración de dispositivos de red offline |
| **PCI Quarterly External Scan** | Escaneo externo trimestral exigido por PCI-DSS |
| **Policy Compliance Auditing** | Verifica configuraciones contra estándares (CIS, NIST, DISA STIG...) |
| **SCAP and OVAL Auditing** | Audita sistemas usando SCAP y OVAL |

---

### Tabla resumen

| Tipo de escaneo | Credenciales | Agresividad | Mejor para |
|---|:---:|:---:|---|
| Host Discovery | No | Muy baja | Mapeo de red inicial |
| Basic Network Scan | Opcional | Baja | Primer análisis general |
| Advanced Scan | Opcional | Configurable | Análisis personalizado |
| Credentialed Patch Audit | ✅ | Media | Inventario completo de parches |
| Web Application Tests | No | Alta | Aplicaciones web (OWASP) |
| Malware Scan | ✅ | Media | Detección de compromiso |
| Compliance | ✅ | Baja | Auditorías normativas |

---

## Configuración detallada de un escaneo

Al seleccionar cualquier plantilla se abre el formulario de configuración con las siguientes pestañas:

---

### Pestaña Settings — BASIC

La capa administrativa del escaneo.

**General:**

| Campo | Descripción | Ejemplo |
|---|---|---|
| **Name** | Nombre descriptivo | `Escaneo avanzado` |
| **Description** | Notas del propósito | `Prueba de primer host discovery realizado` |
| **Folder** | Carpeta de destino | `Proyecto Inicial` |
| **Targets** | IPs, rangos o dominios | `192.168.1.134, testphp.vulnweb.com, 192.168.1.165` |

**Formatos válidos para Targets:**
```
192.168.1.134                      → IP única
192.168.1.0/24                     → Red completa (CIDR)
192.168.1.1-192.168.1.100          → Rango
testphp.vulnweb.com                → Dominio (sin https://)
192.168.1.134, testphp.vulnweb.com → Múltiples separados por coma
```

> [!IMPORTANT]
> Usar **nombre de dominio**, no URL completa. `testphp.vulnweb.com` ✅ — `https://testphp.vulnweb.com` ❌

**Schedule:** programa el escaneo a una hora concreta sin intervención manual. Útil para ejecutar fuera del horario laboral.
```
Enabled: ON · Frequency: Once · Starts: 19:30 · Timezone: UTC+01:00
```

**Notifications:** envía el informe por email al terminar.
```
Email Recipient(s): correo@ejemplo.com · Attach Report: ON · Formato: PDF/CSV/Nessus
```

> [!NOTE]
> Las notificaciones requieren que se haya configurado un servidor SMTP en Nessus. Si no está configurado aparece: *"Notifications will not be sent until your SMTP Server is configured."*

![Instalacion Nessus](/assets/host_discovery_2.png)
![Instalacion Nessus](/assets/schedule.png)
![Instalacion Nessus](/assets//notification.png)


---

### Pestaña Settings — DISCOVERY

Define cómo Nessus detecta los hosts y qué puertos escanea.

**Scan Type — opciones disponibles:**

| Opción | Qué hace | Tiempo |
|---|---|:---:|
| **Host enumeration** | Solo detecta hosts activos, sin escanear puertos | Muy rápido |
| **OS Identification** | Detecta el SO de cada host | Rápido |
| **Port scan (common ports)** | ~1.000 puertos más comunes ✅ recomendado | Moderado |
| **Port scan (all ports)** | Los 65.535 puertos TCP | Muy lento |
| **Custom** | Configuración manual de puertos y ping | Variable |

**Configuración interna de cada opción:**

*Port scan (common ports):*
```
General Settings: Always test local Nessus host · Use fast network discovery
Port Scanner Settings: Scan common ports · Use netstat if credentials provided · Use SYN scanner if necessary
Ping hosts using: TCP · ARP · ICMP (2 retries)
```

*Port scan (all ports):*
```
Port Scanner Settings: Scan all ports (1-65535) · Use netstat · Use SYN scanner
```

*Custom:* permite configurar manualmente **Host Discovery** y **Port Scanning** por separado.

**Remote Host Ping:**

```
Ping the remote host: OFF
```

En este laboratorio se puso **OFF** porque el firewall de Windows bloqueaba ICMP y Nessus asumía que el host estaba apagado. Con el ping desactivado, intenta conectarse directamente aunque no haya respuesta al ping.

> [!NOTE]
> Con ping en OFF los escaneos tardan más — Nessus no puede descartar hosts inactivos rápidamente. Usarlo cuando haya firewalls que filtren ICMP.

> [!WARNING]
> El escaneo de **puertos UDP** demora muchísimo. Desactivarlo a menos que sea estrictamente necesario. Para el primer uso, **Port scan (common ports)** sin UDP es la opción más equilibrada.

![Instalacion Nessus](/assets/discovery.png)
![Instalacion Nessus](/assets/discovery2.png)

---

### Pestaña Settings — ASSESSMENT

Controla el nivel de agresividad del análisis de vulnerabilidades.

- **Default:** equilibrio estándar de Tenable entre velocidad y cobertura.
- **Scan for known web vulnerabilities:** activa comprobaciones para aplicaciones web.
- **Thorough tests:** más exhaustivo pero más lento.

---

### Pestaña Settings — REPORT

Gestiona qué aparece en el informe y cómo se identifican los hosts.

| Opción | Descripción | Recomendación |
|---|---|---|
| **Allow users to edit scan results** | Permite borrar hallazgos del informe | ❌ Desactivar siempre en auditorías reales — evita que el cliente "maquille" resultados |
| **Designate hosts by their DNS name** | Muestra nombre DNS en lugar de IP | ✅ Activar — más legible en informes |
| **Display hosts that respond to ping** | Incluye hosts activos confirmados por ping | ✅ Activar normalmente |
| **Display unreachable hosts** | Incluye IPs que no respondieron | ⚠️ Solo si necesitas confirmar qué equipos estaban apagados — genera informes muy grandes en rangos amplios |
| **Display Unicode characters** | Muestra tildes, ñ y caracteres no ASCII | ✅ Activar si hay nombres con caracteres especiales |

![Instalacion Nessus](/assets/report.png)

---

### Pestaña Settings — ADVANCED

> [!NOTE]
> Esta pestaña **solo está disponible en Advanced Scan**, no en Basic Network Scan.

Su propósito es la **optimización y el sigilo**: ajustar el rendimiento para que el escaneo sea rápido sin tirar la red ni ser bloqueado por sistemas de seguridad.

**Performance Options:**

| Parámetro | Descripción |
|---|---|
| `Slow down the scan when network congestion is detected` | Si la red está saturada, Nessus reduce automáticamente la velocidad |
| `Max simultaneous checks per host` | Comprobaciones paralelas por host (default: 5) |
| `Max simultaneous hosts per scan` | Hosts escaneados a la vez (default: 256) |
| `Max number of concurrent TCP sessions per host` | Límite de conexiones TCP por host |
| `Max number of concurrent TCP sessions per scan` | Límite total de conexiones TCP del escaneo |
| `Reuse SSH connections` | Reutiliza conexiones SSH — evita que el servidor detecte el escaneo como ataque de fuerza bruta |
| `Max scan time per host (in minutes)` | Tiempo máximo por host antes de abandonar |

**Unix find command Options:**

| Opción | Descripción |
|---|---|
| `Exclude filepath` | Directorios a excluir de las búsquedas internas (bases de datos, temporales...) |
| `Exclude filesystem` | Sistemas de archivos a excluir |
| `Include filepath` | Directorios no estándar donde forzar la búsqueda |

> [!WARNING]
> Valores muy altos de hosts simultáneos pueden colapsar el switch de la red. En laboratorio, los valores por defecto son adecuados — no modificar para el primer uso.

![Instalacion Nessus](/assets/escaneo_avanzado.png)
![Instalacion Nessus](/assets/escaneo_avanzado_2.png)
![Instalacion Nessus](/assets/advanced_scan.png)

---

### Pestaña Credentials

Permite añadir credenciales para realizar escaneos autenticados (mayor cobertura, menos falsos positivos).

**Para Windows (SMB):**
```
Credentials → Windows
→ Authentication method: Password
→ Username: nessus · Password: password · Domain: (vacío si es cuenta local)

Opciones adicionales recomendadas:
✅ Never send credentials in the clear
✅ Do not use NTLMv1 authentication
✅ Start the Remote Registry service during the scan
✅ Enable administrative shares during the scan
✅ Start the Server service during the scan
```

**Para Linux (SSH):**
```
Credentials → SSH
→ Username: usuario con sudo
→ Authentication method: password o clave privada
→ Escalate privileges with: sudo
```

> [!NOTE]
> Para que las credenciales Windows funcionen, el target debe estar configurado previamente (usuario local, SMB habilitado, WMI configurado, registro modificado). Ver [`03-instalacion.md`](03-instalacion.md).

> [!TIP]
> 📸 **Captura recomendada:** pestaña Credentials con la configuración Windows SMB completa.

---

### Pestaña Plugins (solo Advanced Scan)

Permite habilitar o deshabilitar **familias enteras de plugins** según el tipo de sistema objetivo.

**Optimización recomendada:** si el target es puramente Linux, desactivar todas las familias de Windows, Cisco y macOS. Reduce el tiempo de escaneo y elimina ruido en los resultados.

```
Plugins → seleccionar familia → Enable / Disable
```

> [!TIP]
> 📸 **Captura recomendada:** pestaña Plugins mostrando las familias con algunas desactivadas.

---

### Pestaña Compliance (solo Advanced Scan)

Carga archivos de auditoría para verificar cumplimiento normativo:

- **CIS Benchmarks** — hardening de sistemas operativos y aplicaciones
- **NIST** — guías del National Institute of Standards and Technology
- **PCI-DSS** — entornos de datos de tarjetas de pago
- **ISO 27001** — gestión de seguridad de la información

> [!IMPORTANT]
> Requiere credenciales. Sin acceso autenticado no se pueden verificar configuraciones internas.

---

## Cómo realizar un escaneo paso a paso

El siguiente flujo refleja exactamente cómo se realizó el escaneo del laboratorio desde **Windows 11**.

### Paso 1 — Preparar el entorno

```powershell
# Verificar que Nessus está corriendo
Get-Service -Name "Tenable Nessus"  # Estado: Running
```

```cmd
# Verificar conectividad con los hosts objetivo
ping 192.168.1.165
ping 192.168.1.163
```

Si se quieren levantar servicios adicionales en la máquina local para que aparezcan en el escaneo, arrancarlos ahora. En este laboratorio se activó **XAMPP** (Apache + MySQL) antes del escaneo para enriquecer los resultados del host `192.168.1.134`.

### Paso 2 — Crear carpeta de proyecto

```
My Scans → New Folder → "Proyecto Inicial"
```

### Paso 3 — Crear nuevo escaneo

```
My Scans → New Scan → Advanced Scan (o la plantilla elegida)
```

### Paso 4 — Configurar y guardar

Rellenar BASIC (nombre, descripción, folder, targets), configurar DISCOVERY (tipo de escaneo, ping OFF si hay firewall), y hacer clic en **Save**.

### Paso 5 — Lanzar

```
Carpeta del proyecto → botón ▶ Launch
```

### Paso 6 — Revisar resultados

Al finalizar, hacer clic en el escaneo para acceder a las cuatro pestañas:

| Pestaña | Contenido |
|---|---|
| **Scan Summary** | Resumen: política, estado, duración, gráfico de vulnerabilidades, autenticación |
| **Hosts** | Lista de hosts con barras de severidad. Clic en un host para ver sus vulnerabilidades individuales |
| **Vulnerabilities** | Vista agregada de todos los hallazgos con columnas Sev, CVSS, VPR, EPSS, Name |
| **History** | Histórico de ejecuciones para comparar antes/después de remediaciones |

**Datos del escaneo real de este laboratorio:**
```
Policy:   Advanced Scan
Status:   Completed
Start:    March 8 at 2:13 PM
End:      March 8 at 2:34 PM
Elapsed:  21 minutes
Hosts:    3  (192.168.1.134 · 192.168.1.165 · 192.168.1.163)
Total vulnerabilities: 60
Auth:     0 Succeeded / 2 Failed (escaneo sin credenciales — esperado)
```

> [!TIP]
> 📸 **Captura recomendada:** pestaña Scan Summary con los Scan Details, el gráfico de vulnerabilidades y el bloque de autenticación.

### Paso 7 — Profundizar en un hallazgo

Al hacer clic en cualquier vulnerabilidad se abre el detalle:

```
[MEDIUM] Unencrypted Telnet Server

Description:
  The remote host is running a Telnet server over an unencrypted channel.
  Logins, passwords and commands are transferred in cleartext...

Solution:
  Disable the Telnet service and use SSH instead.

Output:
  Nessus collected the following banner from the remote Telnet server:
  [banner del servidor]

Port: 23/tcp/telnet    Host: 192.168.1.165
```

> [!TIP]
> 📸 **Captura recomendada:** detalle completo de un hallazgo mostrando Description, Solution, See Also, Output y Port/Hosts.

---

## Análisis de Malware

El módulo de Malware Scan de Nessus busca señales de **compromiso activo** en el sistema, más allá de las vulnerabilidades técnicas de versiones o configuraciones.

```
New Scan → VULNERABILITIES → Malware Scan
```

> [!IMPORTANT]
> El Malware Scan **requiere credenciales obligatoriamente**. Sin autenticación en el sistema no puede inspeccionar el sistema de archivos ni comparar hashes.

### Configuración — Assessment → Malware Settings

**Hash and Allowlist Files:**

| Opción | Descripción |
|---|---|
| **Custom Netstat IP Threat List** | Lista de IPs de servidores C2 conocidos a detectar |
| **Bad MD5/SHA1/SHA256 hashes** | Hashes de archivos maliciosos — si Nessus los encuentra en el target, los reporta |
| **Good MD5/SHA1/SHA256 hashes** | Lista blanca de hashes legítimos — Nessus los ignorará para evitar falsos positivos |
| **Hosts file allowlist** | IPs y hostnames legítimos a ignorar aunque aparezcan en listas de amenazas |

**Yara Rules:**
```
Yara Rules → Add File
```
Permite subir un archivo `.yar` con reglas YARA personalizadas para detectar patrones específicos de malware. Solo se puede subir un archivo, pero puede contener múltiples reglas.

**File System Scanning:**
```
Scan file system: ON
```

Activa el escaneo de directorios y archivos. Directorios habilitados por defecto en Windows:

| Directorio | Default |
|---|:---:|
| `%Systemroot%` | ✅ |
| `%ProgramFiles%` | ✅ |
| `%ProgramFiles(x86)%` | ✅ |
| `%ProgramData%` | ✅ |
| `Scan User Profiles` | ❌ |

En Linux: `$PATH` y `/home` están desactivados por defecto. En macOS: `/Users`, `/Applications` y `/Library` también desactivados por defecto.

> [!WARNING]
> Activar File System Scanning en escaneos con 10+ hosts simultáneos puede degradar el rendimiento significativamente.

> [!TIP]
> 📸 **Captura recomendada:** pestaña Assessment → Malware Settings mostrando las secciones de Hash files, Yara Rules y File System Scanning con los directorios.

### Credenciales para Malware Scan

Las mismas que para escaneos credenciales. Para Windows, añadir en **Credentials → Windows** con las opciones:
- ✅ Start the Remote Registry service during the scan
- ✅ Enable administrative shares during the scan
- ✅ Start the Server service during the scan

### Auditoría de Parches (Patch Audit)

Relacionado con el Malware Scan en cuanto a requisitos, la auditoría de parches verifica si todos los parches del sistema están al día:

```
New Scan → VULNERABILITIES → Credentialed Patch Audit
```

> [!IMPORTANT]
> Requiere credenciales. Sin autenticación, Nessus no puede consultar los paquetes instalados ni su versión de parche.

> [!TIP]
> 📸 **Captura recomendada:** plantilla Credentialed Patch Audit en la galería de Scan Templates.

---

← [Volver al índice](../README.md) · Siguiente: [Políticas de escaneo →](06-politicas-escaneo.md)
