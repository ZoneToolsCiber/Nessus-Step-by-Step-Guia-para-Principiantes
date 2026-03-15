# 🔬 Hallazgos del Laboratorio

← [Volver al índice](../README.md)

---

**Escenario:** Auditoría de servidor web con arquitectura LAMP sobre Metasploitable 2.
**Tipo de escaneo:** Basic Network Scan modificado — sin credenciales — contra `192.168.1.50`.

---

## Resumen ejecutivo del escaneo

| Severidad | Cantidad detectada |
|:---:|:---:|
| 🔴 Crítico | *(completar con captura)* |
| 🟠 Alto | *(completar con captura)* |
| 🟡 Medio | *(completar con captura)* |
| 🟢 Bajo | *(completar con captura)* |
| 🔵 Info | *(completar con captura)* |

> [!TIP]
> 📸 **Captura recomendada:** pantalla principal del escaneo en Nessus mostrando el gráfico de distribución por severidad y el total de hallazgos.

---

## Hallazgos Críticos 🔴

---

### Apache HTTP Server — Path Traversal y RCE

| Campo | Detalle |
|---|---|
| **Plugin ID** | 153584 |
| **CVE** | CVE-2021-41773, CVE-2021-42013 |
| **CVSS v3** | 9.8 |
| **Puerto** | 80/tcp |

**¿Qué significa?** La versión de Apache instalada es vulnerable a Path Traversal: un atacante puede construir una URL especialmente formada para navegar fuera del directorio raíz del servidor web y leer archivos del sistema operativo. Si el módulo `mod_cgi` está activo — como ocurre en Metasploitable — el fallo escala a **ejecución remota de código (RCE)**: el atacante puede ejecutar comandos arbitrarios en el servidor sin ninguna credencial.

**Acción recomendada:**
```bash
sudo apt-get update
sudo apt-get install --only-upgrade apache2
apache2 -v  # verificar nueva versión
```

> [!TIP]
> 📸 **Captura recomendada:** detalle del hallazgo en Nessus con la descripción completa, CVE y puntuación CVSS.

---

### vsftpd 2.3.4 — Backdoor

| Campo | Detalle |
|---|---|
| **Plugin ID** | 55523 |
| **CVE** | CVE-2011-2523 |
| **CVSS v3** | 10.0 |
| **Puerto** | 21/tcp |

**¿Qué significa?** Esta versión concreta de vsftpd contiene una **backdoor deliberadamente insertada** por un atacante que comprometió el repositorio oficial en 2011. Al enviar la cadena `:)` como nombre de usuario en una conexión FTP, el servidor abre automáticamente una shell de root en el **puerto 6200**. Cualquiera que conozca este truco puede obtener acceso root completo al sistema sin ninguna credencial.

**Acción recomendada:**
```bash
sudo apt-get remove vsftpd
sudo apt-get install vsftpd  # instalar versión limpia desde repositorio oficial
```

> [!TIP]
> 📸 **Captura recomendada:** detalle del hallazgo mostrando el CVE-2011-2523 y la descripción de la backdoor.

---

## Hallazgos Altos 🟠

---

### MySQL 5.0 — End of Life

| Campo | Detalle |
|---|---|
| **CVE** | — |
| **CVSS v3** | 7.5 |
| **Puerto** | 3306/tcp |

**¿Qué significa?** MySQL 5.0 llegó a **fin de vida (EOL) en diciembre de 2012** y no recibe ningún parche de seguridad desde entonces. Cualquier vulnerabilidad descubierta hoy en esta versión nunca será corregida por Oracle. Mantener software EOL en producción es equivalente a no tener seguridad en ese componente.

**Acción recomendada:** Migrar a MySQL 8.x o MariaDB 10.x.

---

### MySQL — Autenticación en Texto Claro

| Campo | Detalle |
|---|---|
| **CVE** | — |
| **CVSS v3** | 7.5 |
| **Puerto** | 3306/tcp |

**¿Qué significa?** Las credenciales de MySQL viajan por la red **sin cifrar**. Un atacante con acceso a la misma red — o posicionado con un ataque Man-in-the-Middle — puede capturar el tráfico con Wireshark y leer el usuario y la contraseña de la base de datos en texto plano.

**Acción recomendada:**
```sql
-- En MySQL, activar cifrado obligatorio
SET GLOBAL require_secure_transport = ON;
```

---

## Hallazgos Medios 🟡

---

### SSH Root Login Enabled

| Campo | Detalle |
|---|---|
| **CVE** | — |
| **CVSS v3** | 5.3 |
| **Puerto** | 22/tcp |

**¿Qué significa?** El servidor SSH permite intentos de login directo como usuario `root`. Aunque el atacante también necesita la contraseña, esto facilita enormemente los ataques de fuerza bruta porque no hay que adivinar primero el nombre de usuario — directamente se ataca al usuario con más privilegios del sistema.

**Acción recomendada:**
```bash
sudo nano /etc/ssh/sshd_config
# Cambiar: PermitRootLogin yes → PermitRootLogin no
sudo systemctl restart sshd
```

---

### OpenSSH < 7.0 — Fuga de Clave Privada

| Campo | Detalle |
|---|---|
| **CVE** | CVE-2016-0777 |
| **CVSS v3** | 6.5 |
| **Puerto** | 22/tcp |

**¿Qué significa?** Esta versión de OpenSSH tiene una vulnerabilidad en el cliente SSH que, bajo ciertas condiciones, puede filtrar fragmentos de la clave privada del usuario hacia el servidor al que se conecta. Si un atacante controla un servidor SSH malicioso, puede robar claves privadas de los clientes que se conectan a él.

**Acción recomendada:**
```bash
sudo apt-get install --only-upgrade openssh-server openssh-client
```

---

← [Volver al índice](../README.md) · Siguiente: [Plan de remediación →](09-remediacion.md)
