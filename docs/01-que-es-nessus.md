# 🔎 ¿Qué es Nessus Expert?

← [Volver al índice](../README.md)

---

## ¿Qué es Nessus?

**Nessus**, desarrollado por **Tenable**, es el escáner de vulnerabilidades más utilizado en la industria de la ciberseguridad. Su funcionamiento se basa en una base de datos de más de **175.000 plugins** — pequeños programas especializados — que comprueban si un sistema presenta una debilidad conocida.

A diferencia de un simple escáner de puertos como Nmap, Nessus no solo detecta qué servicios están activos: analiza sus versiones, comprueba si tienen vulnerabilidades conocidas asociadas, verifica la configuración del sistema y genera un reporte priorizado listo para ser trabajado por un equipo de seguridad.

---

## ¿Qué nos permite hacer?

| Capacidad | Descripción |
|---|---|
| **Escaneo de redes** | Detecta qué dispositivos están activos y qué puertos tienen abiertos |
| **Auditoría de parches** | Comprueba si el sistema operativo y los servicios están actualizados |
| **Detección de malware** | Identifica señales de botnets, rootkits o procesos sospechosos |
| **Cumplimiento (Compliance)** | Verifica si la configuración sigue estándares como CIS Benchmarks |
| **Escaneo cloud** | Audita recursos en AWS, Azure y GCP *(exclusivo Expert)* |
| **Análisis IaC** | Detecta configuraciones inseguras en Terraform, CloudFormation... *(exclusivo Expert)* |

---

## ¿Cómo funciona internamente?

Cuando Nessus lanza un escaneo, sigue este flujo:

```
1. Descubrimiento de hosts
   → ¿Qué IPs están activas en el rango objetivo?
         │
         ▼
2. Detección de puertos y servicios
   → ¿Qué puertos están abiertos? ¿Qué software corre en cada uno?
         │
         ▼
3. Identificación de versiones
   → ¿Qué versión exacta tiene Apache / SSH / MySQL?
         │
         ▼
4. Ejecución de plugins
   → ¿Esa versión tiene CVEs conocidos? ¿Está mal configurada?
         │
         ▼
5. Generación del reporte
   → Hallazgos clasificados por severidad con solución recomendada
```

---

## Versiones de Nessus disponibles

Antes de empezar, es importante saber qué versión tienes y qué puedes hacer con ella:

| Característica | Essentials | Professional | Expert |
|---|:---:|:---:|:---:|
| Precio | Gratuita | De pago | De pago (superior) |
| IPs escaneables | 16 IPs | Ilimitadas | Ilimitadas |
| Plugins disponibles | Todos | Todos | Todos |
| Exportación de reportes | Básica | Avanzada | Avanzada |
| Escaneo cloud (AWS/Azure/GCP) | ❌ | ❌ | ✅ |
| Análisis IaC (Terraform...) | ❌ | ❌ | ✅ |
| Soporte técnico prioritario | ❌ | ✅ | ✅ |
| Ideal para | Estudiantes / Lab | Pentesters | Equipos empresariales |

> [!NOTE]
> En este proyecto se utiliza **Nessus Expert**, que permite escanear un número ilimitado de IPs y acceder a todas las capacidades de exportación y análisis avanzado.

---

← [Volver al índice](../README.md) · Siguiente: [Glosario de términos →](02-glosario.md)
