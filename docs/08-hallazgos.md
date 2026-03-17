# 🔬 Hallazgos del Laboratorio

← [Volver al índice](../README.md)

---

**Escenario:** Advanced Scan sin credenciales contra tres hosts del entorno de laboratorio.
**Plataforma del analista:** Windows 11 con Nessus Expert instalado localmente.
**Tipo de escaneo:** Advanced Scan — sin credenciales — CVSS Score: CVSSv3.
**Hosts analizados:** `192.168.1.134` · `192.168.1.165` · `192.168.1.163`
**Duración:** 21 minutos
**Total de vulnerabilidades encontradas:** 60

> [!NOTE]
> Todo el análisis se realizó desde una máquina **Windows 11** con Nessus Expert instalado directamente sobre el sistema operativo — no desde una VM de Kali Linux. El escaneo se ejecutó **sin credenciales** (uncredentialed), lo que simula la perspectiva de un atacante externo. La columna **Auth** aparece como `Fail` en todos los hosts — esto es esperado en un escaneo sin credenciales y no indica un error. Un escaneo con credenciales revelaría aún más vulnerabilidades internas.

---

## Resumen ejecutivo del escaneo

| Severidad | Cantidad detectada |
|:---:|:---:|
| 🔴 Crítico | Confirmado (ver detalle) |
| 🟠 Alto | 2 confirmados |
| 🟡 Medio | Múltiples (ver detalle) |
| 🟢 Bajo | 1 confirmado |
| 🔵 Info | Múltiples |
| **MIXED** | Múltiples grupos |

> [!TIP]
> 📸 **Captura recomendada:** pantalla principal de la pestaña Hosts mostrando los tres hosts con sus barras de vulnerabilidades y el panel lateral de Scan Details con el tiempo de 21 minutos.

> [!TIP]
> 📸 **Captura recomendada:** pestaña Vulnerabilities con las 60 vulnerabilidades listadas, incluyendo los indicadores CRITICAL, HIGH, MEDIUM, LOW, INFO y MIXED.

---

## Distribución por host

| Host | Auth | Vulnerabilidades destacadas |
|---|:---:|---|
| `192.168.1.134` | Fail | Concentración de hallazgos CRITICAL — Apache, Samba |
| `192.168.1.165` | Fail | Hallazgos HIGH y MEDIUM — servicios de red expuestos |
| `192.168.1.163` | Fail | Mezcla de severidades — Apache Tomcat, SSH, SMB |

![vulns encontradas](/assets/escaneo_avanzado_2.png)

---

## Lista completa de vulnerabilidades detectadas

A continuación se recogen todos los hallazgos identificados en la pestaña Vulnerabilities del escaneo, con su severidad, CVSS, VPR, EPSS y nombre tal como los muestra Nessus:

| Severidad | CVSS | VPR | EPSS | Nombre |
|:---:|:---:|:---:|:---:|---|
| 🔴 CRITICAL | 10.0 | — | — | Canonical Ubuntu Linux SEoL (14.04.x) |
| 🟠 HIGH | 7.5 | 5.9 | 0.7714 | Samba Badlock Vulnerability |
| 🟠 HIGH | 7.5 | — | — | NFS Shares World Readable |
| 🟡 MEDIUM | 6.5 | — | — | Unencrypted Telnet Server |
| 🟡 MEDIUM | 5.3 | 4.2 | 0.0815 | SSL Certificate Signed Using Weak Hashing Algorithm |
| 🟡 MEDIUM | 5.0* | 3.6 | 0.0008 | Finger Recursive Request Arbitrary Site Redirection |
| **MIXED** | — | — | — | SSL (Multiple Issues) — 9 hallazgos |
| **MIXED** | — | — | — | SSH (Multiple Issues) — 7 hallazgos |
| **MIXED** | — | — | — | Apache Tomcat (Multiple Issues) — 4 hallazgos |
| **MIXED** | — | — | — | Openbsd Openssh (Multiple Issues) — 2 hallazgos |
| 🟢 LOW | 2.1* | 2.2 | 0.0037 | ICMP Timestamp Request Remote Date Disclosure |
| 🔵 INFO | — | — | — | SMB (Multiple Issues) — 6 hallazgos |
| 🔵 INFO | — | — | — | HTTP (Multiple Issues) — 3 hallazgos |
| 🔵 INFO | — | — | — | TLS (Multiple Issues) — 3 hallazgos |

![vulns encontradas](/assets/vuln_encontradas.png)


> [!NOTE]
> Los valores marcados con `*` indican que el CVSS base puede variar según la versión del vector empleado. Las entradas **MIXED** agrupan varios plugins de diferente severidad bajo un mismo servicio o protocolo.

---

## Hallazgos Críticos 🔴

---

### Canonical Ubuntu Linux SEoL (14.04.x)

| Campo | Detalle |
|---|---|
| **Severidad** | 🔴 CRITICAL |
| **CVSS v3** | 10.0 |
| **Puerto** | 22/tcp/ssh |
| **Host afectado** | `192.168.1.163` |

**Descripción (tal como la muestra Nessus):**
> According to its version, Canonical Ubuntu Linux is 14.04.x. It is, therefore, no longer maintained by its vendor or provider. Lack of support implies that no new security patches for the product will be released by the vendor. As a result, it may contain security vulnerabilities.

**¿Qué significa?** Ubuntu 14.04 alcanzó el **fin de vida (EOL) el 24 de abril de 2019** — hace más de 6 años. Desde esa fecha, Canonical no publica ningún parche de seguridad para esta versión. Cualquier vulnerabilidad nueva descubierta en el sistema operativo, en sus librerías o en sus servicios base nunca recibirá corrección oficial. Es la vulnerabilidad más grave del entorno porque afecta a la base sobre la que corren todos los demás servicios.

**Output de Nessus:**
```
OS                          : Ubuntu Linux 14.04
Security End of Life        : April 24, 2019
Time since Security End of Life (Est.) : >= 6 years
```

**Solución:** Actualizar a una versión de Ubuntu con soporte activo (22.04 LTS o 24.04 LTS).

![vulns encontradas](/assets/v_critical.png)

---

## Hallazgos Altos 🟠

---

### Samba Badlock Vulnerability

| Campo | Detalle |
|---|---|
| **Severidad** | 🟠 HIGH |
| **CVSS v3** | 7.5 |
| **VPR** | 5.9 |
| **EPSS** | 0.7714 |
| **Puerto** | 445/tcp/cifs |
| **Host afectado** | `192.168.1.165` |

**Descripción (tal como la muestra Nessus):**
> The version of Samba, a CIFS/SMB server for Linux and Unix, running on the remote host is affected by a flaw, known as Badlock, that exists in the Security Account Manager (SAM) and Local Security Authority (Domain Policy) (LSAD) protocols due to improper authentication level negotiation over Remote Procedure Call (RPC) channels.

**¿Qué significa?** Un atacante posicionado en la misma red que pueda interceptar el tráfico entre un cliente y un servidor Samba puede **forzar una degradación del nivel de autenticación**. Esto le permite ejecutar llamadas arbitrarias a la red Samba en el contexto del usuario interceptado — lo que incluye ver o modificar datos sensibles en la base de datos Active Directory o deshabilitar servicios críticos.

**Output de Nessus:**
```
Nessus detected that the Samba Badlock patch has not been applied.
```

**Solución:** Actualizar Samba a versión 4.2.11 / 4.3.8 / 4.4.2 o posterior.

![vulns encontradas](/assets/v_high_2.png)

---

### NFS Shares World Readable

| Campo | Detalle |
|---|---|
| **Severidad** | 🟠 HIGH |
| **CVSS v3** | 7.5 |
| **Puerto** | 2049/tcp/rpc-nfs |
| **Host afectado** | `192.168.1.165` |

**Descripción (tal como la muestra Nessus):**
> The remote NFS server is exporting one or more shares without restricting access (based on hostname, IP, or IP range).

**¿Qué significa?** El servidor NFS está exportando recursos compartidos **sin ninguna restricción de acceso**. Cualquier máquina de la red puede montar estas carpetas y leer o escribir en ellas sin necesidad de autenticación. En este caso, la share exportada es `/ *` — es decir, el sistema de archivos raíz, accesible desde cualquier IP.

**Output de Nessus:**
```
The following shares have no access restrictions :
  / *
```

**Solución:** Añadir restricciones de acceso por IP en `/etc/exports`. Ejemplo:
```
/datos    192.168.1.0/24(ro,sync,no_subtree_check)
```

![vulns encontradas](/assets/v_high.png)
---

## Hallazgos Medios 🟡

---

### Unencrypted Telnet Server

| Campo | Detalle |
|---|---|
| **Severidad** | 🟡 MEDIUM |
| **CVSS v3** | 6.5 |
| **Puerto** | 23/tcp/telnet |
| **Host afectado** | `192.168.1.165` |

**Descripción (tal como la muestra Nessus):**
> The remote host is running a Telnet server over an unencrypted channel. Using Telnet over an unencrypted channel is not recommended as logins, passwords, and commands are transferred in cleartext. This allows a remote, man-in-the-middle attacker to eavesdrop on a Telnet session to obtain credentials or other sensitive information and to modify traffic exchanged between a client and server. SSH is preferred over Telnet since it protects credentials from eavesdropping and can tunnel additional data streams such as an X11 session.

**¿Qué significa?** El servidor tiene activo el servicio **Telnet**, un protocolo de acceso remoto de los años 70 que transmite todo — usuario, contraseña y comandos — **en texto plano sin cifrar**. Un atacante con acceso a la red puede capturar una sesión Telnet con Wireshark y leer las credenciales directamente. SSH hace exactamente lo mismo pero de forma cifrada.

**Solución:** Deshabilitar el servicio Telnet e instalar/usar SSH en su lugar.
```bash
sudo systemctl disable telnet
sudo systemctl stop telnet
```

![vulns encontradas](/assets/v_medium.png)
---

### Apache Tomcat Default Files

| Campo | Detalle |
|---|---|
| **Severidad** | 🟡 MEDIUM |
| **Puerto** | 8080/tcp/www |
| **Host afectado** | `192.168.1.163` |

**Descripción:** Los archivos por defecto de Apache Tomcat (página de error, index, ejemplos JSP y servlets) están instalados y accesibles en el servidor remoto. Estos archivos pueden revelar información sobre la instalación de Tomcat o del servidor que facilita el reconocimiento del atacante.

**Output de Nessus:**
```
http://192.168.1.163:8080/index.html
http://192.168.1.163:8080/docs/
http://192.168.1.163:8080/examples/servlets/index.html
http://192.168.1.163:8080/examples/jsp/index.html
http://192.168.1.163:8080/examples/websocket/index.html
```

**Solución:** Eliminar la página de error por defecto y los ejemplos. Seguir las guías de hardening de Tomcat o OWASP.

---

### SSH Weak Algorithms Supported

| Campo | Detalle |
|---|---|
| **Severidad** | 🟡 MEDIUM |
| **Puerto** | 22/tcp/ssh |
| **Host afectado** | `192.168.1.163` |

**Descripción:** El servidor SSH está configurado para usar el cifrado Arcfour o sin cifrado. RFC 4253 desaconseja el uso de Arcfour debido a problemas con las claves débiles.

**Output de Nessus (extracto):**
```
The following weak server-to-client encryption algorithms are supported:
  aes128-ctr, aes192-ctr, aes256-ctr, arcfour256, arcfour128,
  aes128-gcm@openssh.com, chacha20-poly1305@openssh.com...

The following weak client-to-server encryption algorithms are supported:
  [misma lista]
```

**Solución:** Contactar con el proveedor o consultar la documentación para eliminar los cifrados débiles del servidor SSH.

![vulns encontradas](/assets/v_medium_3.png)


---

## Hallazgos Bajos 🟢

---

### ICMP Timestamp Request Remote Date Disclosure

| Campo | Detalle |
|---|---|
| **Severidad** | 🟢 LOW |
| **CVSS v3** | 2.1* |
| **VPR** | 2.2 |
| **EPSS** | 0.0037 |

**¿Qué significa?** El sistema responde a peticiones ICMP Timestamp, lo que permite a un atacante conocer la fecha y hora del sistema remoto. Aunque el impacto directo es mínimo, esta información puede usarse como parte del reconocimiento inicial para sincronizar ataques o identificar el sistema operativo.

---

## Nota sobre el indicador EPSS

El PDF muestra una columna **EPSS** (*Exploit Prediction Scoring System*) junto a CVSS y VPR. El EPSS es un sistema de puntuación que predice la probabilidad de que una vulnerabilidad sea explotada en la práctica en los próximos 30 días:

| Vulnerabilidad | EPSS | Interpretación |
|---|:---:|---|
| Samba Badlock | 0.7714 | 77% de probabilidad de explotación activa — **prioridad máxima** |
| SSL Weak Hashing | 0.0815 | 8% — riesgo moderado |
| Finger Redirection | 0.0008 | 0.08% — riesgo muy bajo |
| ICMP Timestamp | 0.0037 | 0.37% — riesgo muy bajo |

> [!NOTE]
> El alto EPSS de Samba Badlock (0.77) confirma que esta vulnerabilidad tiene exploit activo y se usa en ataques reales, lo que la convierte en prioritaria aunque su CVSS sea "solo" 7.5 y no aparezca como Crítica.

---

← [Volver al índice](../README.md) · Siguiente: [Plan de remediación →](09-remediacion.md)
