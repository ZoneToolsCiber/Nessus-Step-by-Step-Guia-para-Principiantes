# 📊 Cómo Leer e Interpretar un Reporte de Nessus

← [Volver al índice](../README.md)

---

Saber lanzar un escaneo es la mitad del trabajo. La otra mitad — y la más importante para un analista — es saber leer lo que Nessus devuelve y traducirlo en decisiones de remediación concretas.

---

## El código de colores

Nessus clasifica cada hallazgo por severidad usando un código de colores basado en la puntuación CVSS:

| Color | Severidad | CVSS | Qué significa en la práctica |
|:---:|---|:---:|---|
| 🔴 | **Crítico** | 9.0 – 10.0 | Existe un exploit público. Un atacante puede tomar control sin credenciales. **Acción inmediata.** |
| 🟠 | **Alto** | 7.0 – 8.9 | El atacante puede obtener privilegios elevados o acceder a datos sensibles. **Acción urgente.** |
| 🟡 | **Medio** | 4.0 – 6.9 | Fallos de configuración que facilitan ataques más complejos. **Planificar remediación.** |
| 🟢 | **Bajo** | 0.1 – 3.9 | Información útil para el reconocimiento del atacante. Bajo impacto directo. |
| 🔵 | **Info** | 0.0 | Datos informativos: puertos abiertos, certificados, software detectado. No es una vulnerabilidad. |

---

![vulns encontradas](/assets/vuln_encontradas.png)

## Anatomía de un hallazgo

Cada entrada en el reporte de Nessus tiene la siguiente estructura. Entender cada campo es clave para priorizar y remediar correctamente:

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
│  Description:  La versión instalada es anterior a 2.4.50.    │
│                Está afectada por Path Traversal y RCE        │
│                cuando mod_cgi está activo.                   │
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

### Cómo leer cada campo

**Plugin Name + Plugin ID:** identifican exactamente qué comprobación realizó Nessus. El ID permite buscar el plugin en la base de datos de Tenable para más información técnica.

**CVE:** el identificador estándar de la vulnerabilidad. Permite buscarla en el NVD ([nvd.nist.gov](https://nvd.nist.gov)) para ver el análisis completo, las versiones afectadas y los CVSSv2/v3 oficiales.

**CVSS v3 + VPR:** dos puntuaciones complementarias. CVSS mide el impacto técnico teórico; VPR mide el riesgo real actual. Si ambas son altas, la vulnerabilidad es urgente. Si el CVSS es alto pero el VPR es bajo, puede ser que el exploit sea teórico y rara vez se use en la práctica.

**Synopsis:** resumen de una línea. Útil para el reporte ejecutivo.

**Description:** explicación técnica del problema. Aquí se entiende por qué es una vulnerabilidad y cómo puede ser explotada.

**Solution:** qué hay que hacer para arreglarlo. Siempre leer esto antes de proceder — Nessus da la solución exacta.

**Port:** en qué servicio y puerto se detectó. Fundamental para localizar el problema.

---

## Priorización: ¿por dónde empezar?

Con un objetivo como Metasploitable, Nessus puede reportar decenas de hallazgos. El orden correcto de priorización es:

```
1. 🔴 Críticos con VPR alto + exploit público activo  → Remediar de inmediato
2. 🔴 Críticos sin exploit público conocido           → Remediar esta semana
3. 🟠 Altos en servicios expuestos externamente       → Remediar este mes
4. 🟡 Medios                                          → Backlog de seguridad
5. 🟢 Bajos + 🔵 Info                                → Documentar, sin urgencia
```

> [!TIP]
> Usar el filtro **VPR Score** en Nessus para ordenar los hallazgos por riesgo real en lugar de por CVSS teórico. Esto da una lista de prioridades más accionable.

---

## Qué no es una vulnerabilidad

Los hallazgos **Info (🔵)** , en general no suelen ser vulnerabilidades — son observaciones. Por ejemplo:

- `Puerto 22/tcp abierto` → es informativo, no un fallo en sí mismo.
- `Certificado SSL válido hasta 2025` → informativo.
- `Servidor responde a ping` → informativo.

Incluirlos en un informe de vulnerabilidades sin contexto genera confusión. Documentarlos pero no tratarlos como puntos a remediar urgentemente.

---

## Exportar el reporte

Nessus Expert permite exportar el reporte en múltiples formatos:

```
Scan completado → Export → Seleccionar formato
```

| Formato | Uso |
|---|---|
| **PDF** | Informe ejecutivo para presentar a dirección o al profesor |
| **CSV** | Exportar datos para análisis en Excel o scripts propios |
| **Nessus** | Formato nativo para importar en otra instancia de Nessus |
| **HTML** | Informe navegable en el navegador sin necesidad de Nessus |

---

← [Volver al índice](../README.md) · Siguiente: [Hallazgos →](08-hallazgos.md)
