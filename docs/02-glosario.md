# 📖 Glosario de Términos Clave

← [Volver al índice](../README.md)

---

Este glosario está pensado para quien empieza desde cero. Antes de lanzar el primer escaneo, entender estos términos marca la diferencia entre leer un reporte y entenderlo.

---

## CVE — Common Vulnerabilities and Exposures

Es el **identificador único de una vulnerabilidad conocida** a nivel mundial. Tiene el formato `CVE-AÑO-NÚMERO`. Por ejemplo, `CVE-2021-41773` es la vulnerabilidad crítica de Apache que permite ejecución remota de comandos.

Cuando Nessus detecta un hallazgo, lo asocia siempre a su CVE si existe, lo que permite buscar información detallada en bases de datos como el **NVD** (National Vulnerability Database) en [nvd.nist.gov](https://nvd.nist.gov).

```
Ejemplo: CVE-2011-2523
→ Año de publicación: 2011
→ ID correlativo: 2523
→ Descripción: Backdoor en vsftpd 2.3.4
```

---

## CVSS — Common Vulnerability Scoring System

Es el **sistema de puntuación estándar internacional** que mide la gravedad de una vulnerabilidad en una escala del **0 al 10**. No es un número inventado por Nessus — es un estándar calculado por FIRST.org que tiene en cuenta:

- **Vector de ataque:** ¿hay que estar físicamente presente o se puede explotar por red?
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

> [!TIP]
> Actualmente conviven **CVSSv2** y **CVSSv3.1**. Siempre usar la puntuación v3.1 cuando esté disponible — es más precisa y tiene en cuenta factores modernos de explotación.

---

## VPR — Vulnerability Priority Rating

Es la **puntuación propia de Tenable** que complementa al CVSS. Mientras que el CVSS mide el impacto técnico teórico, el VPR tiene en cuenta también:

- Si hay **exploits activos** circulando en la red en este momento.
- Si la vulnerabilidad está siendo **explotada en campañas reales**.
- Qué tan probable es que afecte a tu entorno específico.

Va del **0 al 10**. Un CVSSv3 de 7.5 puede tener un VPR de 4.0 si el exploit nunca se usa en la práctica, o un VPR de 9.8 si hay una campaña activa esta semana.

**El VPR es más útil para priorizar** la remediación que el CVSS solo, porque refleja el riesgo real en lugar del riesgo teórico.

---

## Plugin

Un plugin es un **pequeño programa dentro de Nessus** que realiza una comprobación específica. Hay plugins para:

- Detectar versiones antiguas de Apache
- Verificar si SSH permite login como root
- Comprobar si MySQL acepta contraseñas en texto plano
- Identificar backdoors conocidas en servicios FTP
- Comprobar si un certificado SSL ha caducado

Nessus tiene más de **175.000 plugins** y los actualiza diariamente. Cada plugin tiene un ID numérico único. Por ejemplo:

| Plugin ID | Nombre | Qué comprueba |
|---|---|---|
| `10881` | SSH Protocol Version 1 Detection | Si el servidor acepta el protocolo SSH v1 (inseguro) |
| `11219` | Nessus SYN scanner | Detección de puertos abiertos |
| `20089` | SSL / TLS Versions Supported | Qué versiones de TLS acepta el servidor |

---

## Credentialed Scan vs. Uncredentialed Scan

Uno de los conceptos más importantes para entender el alcance de un escaneo:

**Uncredentialed (sin credenciales):**
Nessus escanea desde fuera, exactamente como lo haría un atacante externo. Solo ve lo que está expuesto en la red: puertos, banners, versiones anunciadas por los servicios. Es menos exhaustivo pero refleja la visión real de un atacante sin acceso previo.

**Credentialed (con credenciales):**
Nessus se autentica en el sistema via **SSH** (Linux) o **WMI** (Windows) y lo analiza desde dentro: versiones exactas de paquetes instalados, archivos de configuración, políticas del sistema, software instalado pero no expuesto en red. Es mucho más completo y preciso — puede detectar vulnerabilidades que desde fuera son invisibles.

```
Uncredentialed → visión del atacante externo
Credentialed   → visión del administrador del sistema
```

> [!NOTE]
> En este proyecto se utiliza un escaneo **uncredentialed** para simular la perspectiva de un atacante externo sobre Metasploitable.

---

## False Positive

Un **falso positivo** ocurre cuando Nessus reporta una vulnerabilidad que en realidad no existe en ese sistema. Las causas más comunes:

- El plugin detecta la versión del software por el **banner del servicio**, pero el administrador ya aplicó el parche sin actualizar el número de versión. Esto es habitual en distribuciones **Debian/Ubuntu**, que aplican backports de seguridad manteniendo el número de versión original.
- El plugin asume que cierta configuración es insegura, pero en ese entorno específico hay controles compensatorios que lo mitigan.

Saber identificar y documentar falsos positivos es una habilidad clave del analista — reportar un falso positivo como vulnerabilidad real daña la credibilidad del informe.

---

## Asset

En Nessus, un **asset** es cualquier dispositivo o sistema que ha sido descubierto o escaneado: un servidor, una VM, un router, un contenedor, una instancia cloud. Nessus agrupa todos los hallazgos por asset, lo que permite ver de un vistazo qué máquinas son las más vulnerables y priorizar cuál remediar primero.

---

## Remediation / Re-scan

- **Remediation:** el conjunto de acciones tomadas para corregir o mitigar una vulnerabilidad detectada (actualizar software, cambiar configuración, cerrar puerto, aplicar parche...).
- **Re-scan:** un segundo escaneo realizado después de la remediación para verificar que los hallazgos han desaparecido. Es la **prueba objetiva** de que la corrección fue efectiva. Sin re-scan, no hay evidencia de que el problema se resolvió.

---

← [Volver al índice](../README.md) · Siguiente: [Instalación →](03-instalacion.md)
