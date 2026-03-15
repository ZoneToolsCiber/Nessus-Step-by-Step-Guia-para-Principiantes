## 📂 Estructura y Guía del Proyecto

Este repositorio está organizado por fases para facilitar la comprensión de la metodología de auditoría de vulnerabilidades aplicada.

### 1. [Metodología de Escaneo y Configuración](./docs/scan-policies.md)
En este apartado se detalla el uso del motor de Nessus y la personalización del **Advanced Scan**.
* **Discovery:** Configuración de enumeración de hosts y puertos para la detección de servicios críticos (HTTP, MySQL, SSH).
* **Performance:** Ajustes de rendimiento para evitar la saturación de red en entornos de producción.
* **Authentication:** Implementación de escaneos de "Caja Blanca" mediante credenciales para una visibilidad profunda del sistema.

### 2. [Entorno de Laboratorio](./lab-environment/network-config.md)
Descripción de la arquitectura de red utilizada para las pruebas.
* **Targets:** Análisis detallado de las máquinas objetivo (Metasploitable, servidores Ubuntu).
* **Network Map:** Diagrama de conectividad y reglas de firewall aplicadas.

### 3. [Análisis de Hallazgos y Resultados](./reports/findings-analysis.md)
Comentarios y categorización de las vulnerabilidades detectadas tras el análisis de varios activos.
* **Vulnerabilidades Críticas:** Explotación de servicios obsoletos (ej. Apache RCE).
* **Configuraciones Débiles:** Análisis de permisos incorrectos y servicios expuestos innecesariamente (ej. Puerto 3306 abierto a internet).
* **Criptografía:** Detección de protocolos SSL/TLS inseguros.

### 4. [Plan de Remediación y Soluciones](./reports/remediation-plan.md)
Propuestas técnicas paso a paso para mitigar los riesgos encontrados basándose en el **Hardening de Sistemas Linux**.
* **Gestión de Parches:** Actualización selectiva de binarios y kernels.
* **Bastionado de SSH:** Desactivación de login por root y uso de llaves criptográficas.
* **Seguridad en Bases de Datos:** Restricción de acceso a MySQL mediante segmentación de red.

---

## 📈 Resultados del Proyecto
Puedes consultar los informes técnicos generados directamente por la herramienta en la carpeta de reportes:
* [📄 Reporte Inicial (PDF)](./reports/nessus-report-initial.pdf)
* [📄 Reporte tras Mitigación (PDF)](./reports/nessus-report-rescan.pdf)