# 🗂 Tipos de Escaneo en Nessus

← [Volver al índice](../README.md)

---

Nessus Expert ofrece múltiples tipos de escaneo. Elegir el correcto es tan importante como configurarlo bien — usar el tipo equivocado puede resultar en un análisis incompleto, en falsos positivos, o en impacto no deseado sobre el sistema objetivo.

---

## 🔵 Basic Network Scan

El escaneo más equilibrado y el **punto de partida recomendado** para cualquier analista. Realiza descubrimiento de hosts, identifica servicios y puertos abiertos, y comprueba vulnerabilidades conocidas en los servicios detectados. Es el equivalente a un chequeo médico general — amplio pero no invasivo.

**Cuándo usarlo:** primer análisis de un sistema desconocido, o cuando se quiere un panorama general sin configuración avanzada.

**Qué incluye por defecto:**
- Descubrimiento de hosts activos
- Escaneo de puertos (top 1000 por defecto, configurable)
- Detección de versiones de servicios
- Plugins de vulnerabilidades sin credenciales

---

## 🔴 Advanced Scan

Versión totalmente configurable del Basic Network Scan. Permite ajustar manualmente cada parámetro: qué familias de plugins activar, qué puertos escanear, si usar credenciales, umbrales de rendimiento, timeouts, etc. Da control total pero requiere más conocimiento para configurarlo correctamente.

**Cuándo usarlo:** cuando se necesita un escaneo muy específico adaptado a un entorno o requisito concreto.

---

## 🟢 Credentialed Patch Audit

Escaneo autenticado que se conecta al sistema via **SSH** (Linux/Unix) o **WMI** (Windows) y comprueba qué paquetes y versiones están instalados, comparándolos contra una base de datos de parches. Detecta software desactualizado aunque el servicio no esté expuesto en red.

**Cuándo usarlo:** cuando se tienen credenciales del sistema y se quiere el inventario más completo de vulnerabilidades — no solo las visibles desde fuera.

**Requisitos:**
- Credenciales SSH con privilegios suficientes (usuario con sudo o root)
- El servicio SSH debe estar activo en el target

---

## 🟡 Web Application Tests

Escanea aplicaciones web en busca de vulnerabilidades del **OWASP Top 10**: SQL Injection, Cross-Site Scripting (XSS), CSRF, directorios expuestos, cabeceras de seguridad ausentes, etc. Envía payloads de prueba activos contra formularios y endpoints de la aplicación.

**Cuándo usarlo:** cuando el objetivo tiene una aplicación web y se quiere analizar la capa de aplicación además de la capa de red.

> [!WARNING]
> Este tipo de escaneo puede provocar comportamientos inesperados: formularios enviados, registros creados en bases de datos, sesiones iniciadas. Usarlo **exclusivamente en entornos controlados** con autorización explícita.

---

## 🟠 Malware Scan

Busca señales de compromiso activo en el sistema: procesos sospechosos en ejecución, archivos con firmas de malware conocido, conexiones a dominios de C2 (Command & Control), modificaciones en archivos críticos del sistema. Requiere credenciales para ser efectivo.

**Cuándo usarlo:** cuando se sospecha que un sistema ya ha sido comprometido y se quiere confirmar o descartar la presencia de malware.

---

## 🔵 Compliance / Policy Compliance

Compara la configuración del sistema contra un estándar de seguridad reconocido — **CIS Benchmarks**, DISA STIG, PCI-DSS, HIPAA, ISO 27001 — y genera un informe de cumplimiento detallado: qué controles se cumplen, cuáles no y cuál es la configuración encontrada frente a la esperada.

No busca vulnerabilidades técnicas sino **errores de configuración** respecto a buenas prácticas establecidas.

**Cuándo usarlo:** en auditorías de cumplimiento normativo o para verificar el nivel de hardening de un servidor antes de ponerlo en producción.

---

## Tabla resumen

| Tipo de escaneo | Credenciales | Agresividad | Mejor para |
|---|:---:|:---:|---|
| Basic Network Scan | Opcional | Baja | Primer análisis general |
| Advanced Scan | Opcional | Configurable | Análisis personalizado |
| Credentialed Patch Audit | ✅ Requeridas | Media | Inventario completo de parches |
| Web Application Tests | No | Alta | Aplicaciones web (OWASP) |
| Malware Scan | ✅ Requeridas | Media | Detección de compromiso activo |
| Compliance | ✅ Requeridas | Baja | Auditorías normativas |

---

## ¿Qué tipo se usó en este proyecto?

En este laboratorio se utilizó un **Basic Network Scan modificado** con configuración personalizada para maximizar la detección en Metasploitable. Los detalles de configuración están en el documento de políticas.

→ Ver: [`06-politicas-escaneo.md`](06-politicas-escaneo.md)

---

← [Volver al índice](../README.md) · Siguiente: [Políticas de escaneo →](06-politicas-escaneo.md)
