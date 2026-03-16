<div align="center">

# 🛡️ Análisis de Vulnerabilidades con Nessus Expert
### Guía completa para iniciarse  con Nessus

![Nessus](/assets/nessus.png)
---

![Tool](https://img.shields.io/badge/Tool-Nessus%20Expert-00B4E0?style=for-the-badge&logo=tenable&logoColor=white)
![Platform](https://img.shields.io/badge/Target-Metasploitable%202-critical?style=for-the-badge)
![OS](https://img.shields.io/badge/Attacker-Kali%20Linux-557C94?style=for-the-badge&logo=kalilinux&logoColor=white)
![Purpose](https://img.shields.io/badge/Purpose-Academic%20%2F%20Máster-informational?style=for-the-badge)

</div>

---

> [!WARNING]
> Este repositorio tiene **fines exclusivamente académicos**. Todo el análisis se realizó sobre máquinas virtuales en un entorno de laboratorio controlado y aislado. Escanear sistemas sin autorización explícita es ilegal y constitutivo de delito.

---

## 📋 Índice del Proyecto

Este repositorio está organizado en documentos independientes para facilitar la lectura y la navegación. El README actúa como índice principal — cada sección tiene su propio archivo dentro de `docs/`.

---

### 🎓 Sobre este proyecto

> *"Como estudiante apasionado por la ciberseguridad, entiendo que la teoría es solo el primer paso. El análisis de vulnerabilidades es una de las habilidades más críticas en el mundo real. He creado este repositorio no solo para documentar mi progreso, sino también para que sirva de guía a otros compañeros que, como yo, están empezando a utilizar Nessus para securizar sus entornos. Mi meta es democratizar el conocimiento técnico y facilitar la curva de aprendizaje."*


En el panorama actual de la ciberseguridad, la superficie de ataque de las organizaciones crece de forma exponencial. La gestión de vulnerabilidades no es una tarea puntual, sino un **proceso cíclico y crítico**. Este proyecto cubre las fases de **Descubrimiento de Información** y **Evaluación de Vulnerabilidades** utilizando Nessus Expert como herramienta principal, sobre un entorno de laboratorio virtualizado.

---

### ❓ ¿Por qué este proyecto?

- **Identificar proactivamente** fallos de seguridad antes de que sean explotados.
- **Priorizar riesgos** basándose en métricas estándar como CVSS v3.1 y VPR.
- **Cumplir con normativas**: NIST, ISO 27001 y ENS exigen escaneos periódicos.
- **Demostrar el ciclo completo**: desde el escaneo inicial hasta la remediación verificada con re-scan.

---

## 📂 Documentación

| Documento | Descripción |
|---|---|
| [`01-Que-es-nessus`](docs/01-que-es-nessus.md) | Qué es Nessus Expert, versiones disponibles y comparativa |
| [`02-Glosario`](docs/02-glosario.md) | Glosario de términos: CVE, CVSS, VPR, Plugin, False Positive... |
| [`03-Instalacion`](docs/03-instalacion.md) | Instalación paso a paso de Nessus Expert en Kali Linux |
| [`04-Entorno-laboratorio`](docs/04-entorno-laboratorio.md) | Configuración del entorno de laboratorio virtualizado |
| [`05-Tipos-escaneo`](docs/05-tipos-escaneo.md) | Tipos de escaneo explicados: cuándo y cómo usar cada uno |
| [`06-Politicas-escaneo`](docs/06-politicas-escaneo.md) | Políticas de escaneo configuradas en este proyecto |
| [`07-Como-leer-reporte`](docs/07-como-leer-reporte.md) | Cómo leer e interpretar un reporte de Nessus |
| [`08-Hallazgos`](docs/08-hallazgos.md) | Hallazgos del laboratorio con análisis detallado |
| [`09-Remediacion`](docs/09-remediacion.md) | Plan de remediación y verificación con re-scan |

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
│   └── 📂 diagrams/
│       └── lab_network.png
│
├── 📂 docs/
│   ├── 01-que-es-nessus.md
│   ├── 02-glosario.md
│   ├── 03-instalacion.md
│   ├── 04-entorno-laboratorio.md
│   ├── 05-tipos-escaneo.md
│   ├── 06-politicas-escaneo.md
│   ├── 07-como-leer-reporte.md
│   ├── 08-hallazgos.md
│   └── 09-remediacion.md
│
├── 📂 reports/
│   ├── nessus-report-initial.pdf     # Reporte del primer escaneo
│   └── nessus-report-rescan.pdf      # Reporte del re-scan post-remediación
│
├── 📂 lab-environment/
│   ├── network-config.md             # Configuración de red de las VMs
│   └── metasploitable-setup.md       # Guía de configuración de Metasploitable 2
│
└── 📄 README.md                      # Este archivo — índice principal
```

---

<div align="center">

---

*Desarrollado con fines académicos · Módulo 4 — Seguridad en Arquitecturas Linux*
*Herramienta: [Nessus Expert — Tenable](https://www.tenable.com/products/nessus)*

</div>
