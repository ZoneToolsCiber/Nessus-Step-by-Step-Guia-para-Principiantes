# 🔧 Políticas de Escaneo

← [Volver al índice](../README.md)

---

## Tabla de Contenidos

- [¿Qué son las políticas de escaneo?](#qué-son-las-políticas-de-escaneo)
- [¿Para qué sirven?](#para-qué-sirven)
- [Cómo se definen](#cómo-se-definen)
- [Importancia en un entorno empresarial](#importancia-en-un-entorno-empresarial)
- [Templates de Windows y Linux](#templates-de-windows-y-linux)
- [La importancia de los Plugins](#la-importancia-de-los-plugins)
- [Política aplicada en este proyecto](#política-aplicada-en-este-proyecto)

---

## ¿Qué son las políticas de escaneo?

Una **política de escaneo** en Nessus es una plantilla de configuración guardada y reutilizable que define exactamente **cómo** debe comportarse un escaneo: qué puertos analizar, qué plugins ejecutar, con qué credenciales autenticarse, a qué velocidad trabajar y qué incluir en el informe final.

En lugar de configurar manualmente cada parámetro cada vez que se lanza un escaneo, se define la política una vez y se aplica tantas veces como se necesite — siempre con el mismo comportamiento garantizado.

```
My Scans → Policies → New Policy
→ Seleccionar plantilla base → Configurar parámetros → Guardar
→ Al crear un nuevo escaneo: Settings → Policy → [seleccionar política guardada]
```

> [!TIP]
> 📸 **Captura recomendada:** sección Policies mostrando una política guardada con su nombre y plantilla base.

---

## ¿Para qué sirven?

Las políticas resuelven tres problemas fundamentales en la gestión de vulnerabilidades:

### 1. Estandarización
Sin políticas, cada analista podría lanzar escaneos con configuraciones diferentes, obteniendo resultados inconsistentes e incomparables entre sí. Una política garantiza que el escaneo del lunes y el del viernes, o el del analista A y el analista B, usan exactamente los mismos parámetros.

### 2. Eficiencia
Configurar un Advanced Scan desde cero con todos sus parámetros (puertos, plugins, credenciales, rendimiento, notificaciones) puede llevar tiempo. Una política bien definida permite lanzar un escaneo complejo en segundos.

### 3. Repetibilidad y trazabilidad
En auditorías y procesos de cumplimiento normativo, poder demostrar que todos los escaneos periódicos se realizaron bajo las mismas condiciones es un requisito. La política actúa como evidencia de que el proceso es reproducible y controlado.

---

## Cómo se definen

Una política se construye configurando las mismas pestañas que en un escaneo normal, pero guardándola de forma independiente para reutilizarla:

### Paso 1 — Acceder a Policies
```
My Scans → Policies (menú lateral) → New Policy
```

### Paso 2 — Elegir la plantilla base
Se selecciona la plantilla que servirá de punto de partida. Las más usadas como base de políticas son:

| Plantilla base | Cuándo usarla como base |
|---|---|
| **Advanced Scan** | Para políticas con control total de parámetros |
| **Basic Network Scan** | Para políticas estándar de reconocimiento general |
| **Credentialed Patch Audit** | Para políticas de auditoría de parches autenticada |
| **Malware Scan** | Para políticas de detección de compromiso |

### Paso 3 — Configurar los parámetros

Los parámetros clave que definen el comportamiento de la política son:

**Alcance (Discovery):**
- Qué puertos escanear (top 1000, todos los 65.535, rango personalizado)
- Si se hace ping previo o se escanea directamente
- Métodos de ping (ICMP, TCP, ARP, UDP)

**Profundidad (Assessment):**
- Nivel de agresividad del análisis
- Si se activan pruebas específicas de aplicaciones web
- Si se realizan pruebas exhaustivas o rápidas

**Credenciales:**
- Tipo de autenticación (SMB para Windows, SSH para Linux)
- Usuarios y contraseñas o claves privadas
- Opciones de elevación de privilegios

**Plugins:**
- Qué familias de plugins están activas
- Qué plugins individuales se incluyen o excluyen

**Rendimiento (Advanced):**
- Número máximo de hosts simultáneos
- Número máximo de comprobaciones por host
- Timeouts y límites de conexión TCP

**Informe (Report):**
- Cómo se identifican los hosts (por IP o por DNS)
- Qué hosts aparecen en el informe
- Si el informe es editable o no

### Paso 4 — Guardar y asociar al escaneo
```
Save → la política queda disponible en la sección Policies
Al crear un nuevo escaneo → Settings → Policy → [nombre de la política]
```

---

## Importancia en un entorno empresarial

En una empresa real, las políticas de escaneo no son opcionales — son un elemento central de la **gestión de vulnerabilidades** por varias razones:

---

### Cumplimiento normativo

Marcos como **PCI-DSS**, **ISO 27001**, **ENS** o **NIST** exigen que los escaneos de vulnerabilidades se realicen de forma **periódica, documentada y bajo condiciones controladas**. Una política garantiza que cada escaneo trimestral o mensual se ejecuta exactamente igual — lo que permite comparar resultados entre periodos y demostrar ante auditores externos que el proceso es riguroso.

---

### Segmentación por entorno

En una empresa no todos los sistemas son iguales ni tienen el mismo nivel de criticidad. Las políticas permiten definir escaneos adaptados a cada tipo de entorno:

| Política | Entorno objetivo | Configuración típica |
|---|---|---|
| `Política-Servidores-Producción` | Servidores críticos en producción | Lenta, sin UDP, fuera de horario laboral, con credenciales |
| `Política-Escritorios-Windows` | Equipos de usuario final | Media velocidad, plugins de Windows, con SMB |
| `Política-Servidores-Linux` | Granjas de servidores Linux | SSH credenciado, plugins de Linux únicamente, alta prioridad |
| `Política-Periféricos-Red` | Switches, routers, impresoras | Solo puertos de gestión, plugins de red, sin credenciales |
| `Política-Web-Externo` | Aplicaciones web públicas | Web Application Tests, agresividad alta, fuera del horario |

---

### Control del impacto en la red

En producción, lanzar un escaneo agresivo sin control puede degradar el rendimiento de la red o incluso tirar servicios sensibles. Una política bien definida incluye límites de rendimiento que garantizan que el escaneo no impacta en el negocio:

```
Max simultaneous hosts:   10   → no satura el switch
Max checks per host:       5   → no colapsa servidores con poca RAM
Network timeout:          30s  → no bloquea el escaneo en hosts lentos
Scan only outside hours:  ON   → se programa para las 2 AM
```

---

### Reducción de falsos positivos

Una política bien ajustada para el entorno concreto genera menos ruido. Si se sabe que todos los servidores de un segmento son Linux con Apache, activar plugins de Windows o Cisco solo añade falsos positivos y alarga el escaneo innecesariamente. La política elimina ese ruido desde el origen.

---

### Trabajo en equipo

En un SOC o equipo de seguridad con varios analistas, las políticas garantizan que todos trabajan bajo los mismos estándares. Un analista junior no puede "olvidarse" de activar los plugins de SSH o de poner las credenciales — están en la política y se aplican automáticamente.

---

## Templates de Windows y Linux

Nessus distingue entre sistemas operativos y adapta su análisis según el tipo de host detectado. La elección correcta de plugins y credenciales según el SO del objetivo marca la diferencia entre un escaneo superficial y uno completo.

---

### Template para Windows

Cuando el objetivo es un sistema Windows, el escaneo más completo requiere:

**Credenciales:**
```
Credentials → Windows → SMB
→ Authentication method: Password / NTLMv2
→ Username / Password / Domain
```

**Plugins clave a activar (familias):**

| Familia de plugins | Qué detecta |
|---|---|
| `Windows` | Vulnerabilidades del SO Windows y sus componentes |
| `Windows: Microsoft Bulletins` | Boletines de seguridad de Microsoft sin aplicar |
| `Windows: User Management` | Usuarios locales, grupos, políticas de contraseñas |
| `SMB` | Configuración insegura de recursos compartidos |
| `Windows: Compliance` | Verificación contra CIS Benchmarks de Windows |

**Plugins a desactivar para reducir ruido:**
- Familias de Linux, Solaris, AIX, macOS → no aplican a Windows
- Familias de Cisco, Juniper → no aplican en servidores Windows estándar

**Opciones específicas de Windows en el escaneo:**
```
✅ Start the Remote Registry service during the scan
✅ Enable administrative shares during the scan
✅ Start the Server service during the scan
✅ Do not use NTLMv1 authentication
```

**¿Qué puede detectar un escaneo Windows credenciado que uno sin credenciales no puede?**

- Versiones exactas de software instalado (Word, Chrome, Adobe Reader...)
- Parches de Windows Update pendientes de aplicar
- Configuración del registro del sistema
- Políticas de contraseñas y usuarios locales
- Servicios en ejecución con permisos excesivos
- Software EOL instalado que no tiene puertos de red abiertos

> [!TIP]
> 📸 **Captura recomendada:** pestaña Credentials con la configuración Windows SMB y las opciones adicionales activadas.

---

### Template para Linux

Cuando el objetivo es un sistema Linux, el acceso más completo se logra via SSH:

**Credenciales:**
```
Credentials → SSH
→ Authentication method: password o clave privada (recomendada)
→ Username: usuario con sudo
→ Escalate privileges with: sudo
→ sudo password: (si es necesario)
```

**Plugins clave a activar (familias):**

| Familia de plugins | Qué detecta |
|---|---|
| `Ubuntu Local Security Checks` | Parches pendientes específicos de Ubuntu/Debian |
| `Red Hat Local Security Checks` | Parches de RHEL/CentOS/Fedora |
| `Debian Local Security Checks` | Parches específicos de Debian |
| `General Linux` | Vulnerabilidades genéricas de Linux |
| `Firewalls` | Configuración de iptables, nftables, firewalld |
| `Web Servers` | Apache, Nginx, Tomcat y sus vulnerabilidades |
| `Databases` | MySQL, PostgreSQL, MongoDB... |

**Plugins a desactivar para reducir ruido:**
- Familias de Windows, macOS, Cisco → no aplican
- `Microsoft Bulletins` → solo para sistemas Windows

**¿Qué puede detectar un escaneo Linux credenciado que uno sin credenciales no puede?**

- Paquetes instalados con versiones vulnerables (aunque el servicio no esté expuesto en red)
- Versión exacta del kernel y sus CVEs asociados
- Archivos con permisos SUID/SGID peligrosos
- Configuración de sudoers
- Servicios activos que no tienen puertos de red abiertos
- Configuración de SSH (`/etc/ssh/sshd_config`)
- Cron jobs sospechosos

> [!TIP]
> 📸 **Captura recomendada:** pestaña Credentials con la configuración SSH y la opción de elevación de privilegios con sudo.

---

### Comparativa: sin credenciales vs con credenciales

| Aspecto | Sin credenciales | Con credenciales |
|---|:---:|:---:|
| Visión | Externa (como un atacante) | Interna (como el administrador) |
| Detección de versiones | Solo por banners de red | Versiones exactas de todos los paquetes |
| Falsos positivos | Más frecuentes | Menos — datos más precisos |
| Software no expuesto | ❌ No detectable | ✅ Detectable |
| Parches del SO | Solo los visibles desde red | Todos los pendientes |
| Configuración interna | ❌ | ✅ Registro, sudoers, sshd_config... |
| Tiempo de escaneo | Más rápido | Más lento (más comprobaciones) |

---

## La importancia de los Plugins

Los plugins son el **corazón de Nessus**. Sin ellos, Nessus es solo un escáner de puertos. Son los plugins los que transforman un puerto abierto en un hallazgo de seguridad concreto con CVE, severidad y solución.

---

### ¿Qué son exactamente?

Un plugin es un programa escrito en **NASL** (Nessus Attack Scripting Language) que encapsula:

1. **La firma de la vulnerabilidad** — cómo identificarla en un sistema real
2. **El algoritmo de detección** — la secuencia de comprobaciones para confirmar que el sistema es vulnerable
3. **La información del hallazgo** — descripción, CVE, CVSS, solución, referencias
4. **Las acciones de reparación** — qué hay que hacer exactamente para corregirlo

Cada plugin tiene un **ID único numérico**. Por ejemplo:
- Plugin `11219` → Nessus SYN scanner (detecta puertos abiertos)
- Plugin `20089` → SSL/TLS Versions Supported
- Plugin `55523` → vsftpd 2.3.4 Backdoor Detection

---

### ¿Por qué son tan importantes?

**La cobertura de Nessus es exactamente igual a la suma de sus plugins activos.** Si un plugin está desactivado, Nessus no puede detectar esa vulnerabilidad — aunque esté presente en el sistema. Por eso la gestión de plugins es una decisión estratégica, no técnica.

**Un ejemplo concreto:** si en una empresa hay servidores Apache y se desactiva la familia de plugins `Web Servers`, Nessus podría ver el puerto 80 abierto pero nunca reportaría que la versión de Apache tiene CVEs críticos. El sistema aparecería como "limpio" cuando en realidad no lo es.

---

### Actualización continua de plugins

Tenable publica actualizaciones de plugins **todos los días** a medida que se descubren nuevas vulnerabilidades. Este proceso es automático en Nessus:

```
Settings → About → Plugins
→ Plugin Set: [fecha de la última actualización]
→ Last Updated: [fecha y hora]
```

Mantener los plugins actualizados es crítico. Una vulnerabilidad publicada hoy (como Log4Shell, Heartbleed o EternalBlue en su momento) puede tener un plugin disponible en Nessus en cuestión de horas — pero solo se detectará si los plugins están al día.

> [!TIP]
> 📸 **Captura recomendada:** pantalla About de Nessus mostrando la fecha de actualización de plugins y el número total instalado.

---

### Familias de plugins — organización y selección

Los más de 175.000 plugins están organizados en **familias temáticas**. La selección correcta de familias según el entorno es una de las optimizaciones más importantes:

| Criterio | Acción recomendada |
|---|---|
| Target es Windows puro | Desactivar familias de Linux, macOS, Cisco, Juniper |
| Target es Linux/Ubuntu | Desactivar familias de Windows, macOS, dispositivos de red |
| Target es un router/switch | Activar solo familias de Cisco/Juniper/F5, desactivar el resto |
| Target es una aplicación web | Activar familia `CGI abuses`, `Web Servers` — desactivar el resto |
| Escaneo rápido de reconocimiento | Desactivar familias de compliance y auditoría local |

**Ventajas de una selección óptima:**

- **Velocidad:** menos plugins = menos comprobaciones = escaneo más rápido
- **Menos ruido:** los plugins de una familia que no aplica generan hallazgos irrelevantes o falsos positivos
- **Menor impacto en la red:** menos peticiones enviadas al objetivo

---

### Plugin Rules — ajuste fino de resultados

Además de activar o desactivar familias enteras, Nessus permite crear **reglas individuales sobre plugins** desde `My Scans → Plugin Rules`:

| Tipo de regla | Cuándo usarla |
|---|---|
| Cambiar severidad de un plugin | Cuando un hallazgo de severidad Media es irrelevante en tu entorno concreto — bajarlo a Info para que no contamine el informe |
| Ocultar un plugin | Cuando es un falso positivo recurrente confirmado en un sistema específico |
| Recategorizar un plugin | Cuando la severidad por defecto no refleja el riesgo real en tu contexto |

> [!NOTE]
> Las Plugin Rules son herramientas de ajuste fino — no deben usarse para "esconder" vulnerabilidades reales, sino para eliminar ruido legítimamente documentado (falsos positivos confirmados).

---

## Política aplicada en este proyecto

En el laboratorio se configuró una política personalizada basada en **Advanced Scan** con las siguientes modificaciones específicas para el entorno de Windows 11 + máquinas Linux:

### Configuración general

- **Scan Type:** Port scan (common ports) — equilibrio entre cobertura y velocidad.
- **Remote Host Ping:** OFF — el firewall de Windows bloqueaba ICMP.
- **Detección de servicios:** búsqueda de **banners** activada para identificar versiones exactas de Apache, MySQL y vsftpd.
- **Escaneo UDP:** desactivado — demora excesivamente el escaneo sin aportación proporcional.

### Enumeración de usuarios SSH

```
Settings → Assessment → Brute Force → SSH
→ Activar: "Attempt to enumerate valid SSH usernames"
```

Permite verificar si el servidor SSH revela nombres de usuario válidos mediante diferencias en los tiempos de respuesta — vector previo en ataques de fuerza bruta dirigida.

> [!NOTE]
> Esto es posible en OpenSSH versiones antiguas (como la 4.7 de Metasploitable). Las versiones modernas de OpenSSH mitigan este comportamiento.

### Consideraciones de rendimiento

Para no saturar las VMs con recursos limitados:

```
Settings → Performance
→ Max simultaneous checks per host: 5  (por defecto: 30)
→ Max simultaneous hosts per scan:  1
```

### Resumen de la política aplicada

| Parámetro | Valor configurado | Valor por defecto |
|---|---|---|
| Cobertura de puertos | Common ports | Top 1.000 |
| Remote Host Ping | OFF | ON |
| Service Detection | Todos los puertos | Solo well-known |
| Banner grabbing | Activado | Activado |
| Escaneo UDP | Desactivado | Desactivado |
| Enumeración SSH | Activada | Desactivada |
| Max checks simultáneos | 5 | 30 |
| Max hosts simultáneos | 1 | 5 |

> [!TIP]
> 📸 **Captura recomendada:** pantalla de configuración de la política mostrando los ajustes de puertos, ping, detección de servicios y rendimiento.

---

← [Volver al índice](../README.md) · Siguiente: [Cómo leer un reporte →](07-como-leer-reporte.md)
