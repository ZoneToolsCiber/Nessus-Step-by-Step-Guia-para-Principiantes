# ⚙️ Instalación de Nessus Expert

← [Volver al índice](../README.md)

---

Nessus Expert puede instalarse tanto en **Linux** como en **Windows**. El proceso de activación y configuración desde el navegador es idéntico en ambos sistemas — lo que cambia es la instalación del paquete y la gestión del servicio.

---

## Tabla de Contenidos

- [Requisitos previos](#requisitos-previos)
- [Paso 1 — Obtener la licencia](#paso-1--obtener-la-licencia-de-activación)
- [Instalación en Linux](#-instalación-en-linux-kali--debian--ubuntu)
- [Instalación en Windows](#-instalación-en-windows)
- [Paso común — Activación desde el navegador](#paso-común--activación-y-configuración-desde-el-navegador)
- [Verificar la instalación](#verificar-la-instalación)
- [Solución de problemas comunes](#solución-de-problemas-comunes)

---

## Requisitos previos

| Componente | Linux | Windows |
|---|---|---|
| **Sistema operativo** | Kali Linux / Ubuntu 20.04+ / Debian 11+ | Windows 10 / 11 / Server 2016+ |
| **RAM** | 4 GB (8 GB recomendado) | 4 GB (8 GB recomendado) |
| **Almacenamiento** | 30 GB libres | 30 GB libres |
| **CPU** | 4 núcleos | 4 núcleos |
| **Navegador** | Chrome / Firefox | Chrome / Firefox / Edge |
| **Permisos** | Usuario con `sudo` | Administrador local |
| **Conectividad** | Internet para activación y plugins | Internet para activación y plugins |

---

## Paso 1 — Obtener la licencia de activación

Este paso es el mismo independientemente del sistema operativo.

### Opción A — Prueba gratuita de 7 días (recomendada para laboratorio)

Tenable ofrece una **trial gratuita de 7 días completos** de Nessus Expert sin necesidad de introducir datos de pago. Es la opción ideal para un entorno académico o para evaluar la herramienta:

1. Ir a [tenable.com/products/nessus](https://www.tenable.com/products/nessus).
2. Seleccionar **Nessus Expert** y hacer clic en **Try for free**.
3. Crear una cuenta con nombre, apellido y email.
4. Se recibirá un **Activation Code** por email en pocos minutos.
5. Anotar el código — se usará en el paso de activación desde el navegador.

> [!NOTE]
> Los 7 días comienzan a contar desde el momento en que se activa la licencia en Nessus, no desde el registro en la web. Una vez expirada la trial, Nessus pasa a modo limitado hasta que se active una licencia de pago. Para este laboratorio, 7 días son más que suficientes.

### Opción B — Licencia de pago

Para uso profesional o continuado, adquirir la licencia completa desde el portal de Tenable. El proceso de activación es idéntico al de la trial — solo cambia el código recibido.

---

## 🐧 Instalación en Linux (Kali / Debian / Ubuntu)

### Paso 2L — Descargar el paquete `.deb`

Desde la [página de descargas de Tenable](https://www.tenable.com/downloads/nessus), seleccionar la versión para **Debian / Kali Linux (amd64)**:

```bash
# Descargar el paquete .deb (sustituir X.X.X por la versión actual)
wget https://www.tenable.com/downloads/api/v1/public/pages/nessus/downloads/XXXXX/get \
     -O Nessus-X.X.X-debian10_amd64.deb
```

> [!TIP]
> Es más cómodo descargarlo directamente desde el navegador en la página de Tenable, que siempre muestra la versión más reciente.

---

### Paso 3L — Instalar y arrancar el servicio

```bash
# Instalar el paquete descargado
sudo dpkg -i Nessus-X.X.X-debian10_amd64.deb

# Iniciar el servicio de Nessus
sudo systemctl start nessusd

# Habilitar el inicio automático con el sistema
sudo systemctl enable nessusd

# Verificar que el servicio está activo
sudo systemctl status nessusd
```

Salida esperada de `systemctl status`:
```
● nessusd.service - Nessus Vulnerability Scanner
     Loaded: loaded (/lib/systemd/system/nessusd.service; enabled)
     Active: active (running)
```

**Comandos de gestión del servicio en Linux:**

```bash
sudo systemctl start nessusd    # Arrancar
sudo systemctl stop nessusd     # Detener
sudo systemctl restart nessusd  # Reiniciar
sudo systemctl status nessusd   # Ver estado
```

---

## 🪟 Instalación en Windows

### Paso 2W — Descargar el instalador `.exe`

Desde la [página de descargas de Tenable](https://www.tenable.com/downloads/nessus), seleccionar la versión para **Windows (x86_64)**. Se descargará un archivo `.exe` del tipo:

```
Nessus-X.X.X-x64.exe
```

> [!TIP]
> Asegurarse de descargar la versión `x64` si el sistema es de 64 bits, que es lo habitual en cualquier equipo moderno.

---

### Paso 3W — Ejecutar el instalador

1. Hacer clic derecho sobre el archivo `.exe` descargado → **Ejecutar como administrador**.
2. Seguir el asistente de instalación: aceptar la licencia y dejar la ruta por defecto:
   ```
   C:\Program Files\Tenable\Nessus\
   ```
3. Al finalizar, el instalador arranca automáticamente el servicio **Tenable Nessus** en Windows.

> [!IMPORTANT]
> Si Windows Defender o el antivirus muestra una alerta durante la instalación, es un falso positivo habitual con herramientas de seguridad. Permitir la ejecución.

---

### Gestión del servicio en Windows

A diferencia de Linux, en Windows el servicio se gestiona desde el **Administrador de servicios** o desde PowerShell:

**Opción A — Administrador de servicios (interfaz gráfica):**
```
Tecla Windows → buscar "Servicios" → localizar "Tenable Nessus"
→ Clic derecho → Iniciar / Detener / Reiniciar
```

**Opción B — PowerShell (como Administrador):**
```powershell
# Ver estado del servicio
Get-Service -Name "Tenable Nessus"

# Arrancar
Start-Service -Name "Tenable Nessus"

# Detener
Stop-Service -Name "Tenable Nessus"

# Reiniciar
Restart-Service -Name "Tenable Nessus"
```

**Opción C — CMD (como Administrador):**
```cmd
net start "Tenable Nessus"
net stop  "Tenable Nessus"
```

> [!NOTE]
> En Windows, Nessus se registra como un **servicio de Windows** que arranca automáticamente con el sistema. No es necesario abrirlo manualmente — basta con abrir el navegador y acceder a `https://localhost:8834` mientras el servicio esté corriendo.

> [!TIP]
> 📸 **Captura recomendada:** Administrador de servicios de Windows mostrando "Tenable Nessus" en estado *En ejecución*.

---

## Paso común — Activación y configuración desde el navegador

Este paso es **idéntico en Linux y Windows**. Una vez que el servicio está corriendo:

1. Abrir el navegador y acceder a: `https://localhost:8834`
2. Aceptar el aviso de certificado autofirmado — es normal, Nessus usa HTTPS con certificado propio.
3. En la pantalla de bienvenida, seleccionar **Nessus Expert**.
4. Introducir el **Activation Code** recibido por email.
5. Crear el usuario administrador (nombre de usuario y contraseña).
6. Esperar a que Nessus descargue e instale todos los plugins.

> [!IMPORTANT]
> La descarga de plugins puede tardar entre **10 y 30 minutos** dependiendo de la conexión. No cerrar el navegador ni reiniciar el servicio durante este proceso.
>
> Si se interrumpe en **Linux**: `sudo systemctl restart nessusd`
> Si se interrumpe en **Windows**: reiniciar "Tenable Nessus" desde el Administrador de servicios.

---

## Verificar la instalación

Una vez completada la descarga de plugins, confirmar que la versión activa es Expert:

```
Menú superior derecho → tu usuario → About
→ Debe mostrar: "Nessus Expert" con la fecha de expiración de la licencia
```

> [!TIP]
> 📸 **Captura recomendada:** pantalla *About* de Nessus confirmando la versión Expert y la licencia activa.

---

## Solución de problemas comunes

| Problema | Sistema | Causa probable | Solución |
|---|:---:|---|---|
| El navegador no carga `localhost:8834` | Linux | Servicio no arrancado | `sudo systemctl start nessusd` |
| El navegador no carga `localhost:8834` | Windows | Servicio detenido | Reiniciar "Tenable Nessus" desde Servicios |
| Activación falla con "Invalid code" | Ambos | Código ya usado o caducado | Solicitar nuevo código en el portal de Tenable |
| Los plugins no se descargan | Linux | Firewall bloqueando | `curl -I https://plugins.nessus.org` |
| Los plugins no se descargan | Windows | Antivirus bloqueando | Añadir excepción para Nessus en el antivirus |
| El escaneo no detecta el objetivo | Ambos | Target no responde a ping | Desactivar "Ping the remote host" en las opciones del escaneo |
| Alerta del antivirus durante instalación | Windows | Falso positivo | Permitir la ejecución — comportamiento normal en herramientas de seguridad |

---

← [Volver al índice](../README.md) · Siguiente: [Entorno de laboratorio →](04-entorno-laboratorio.md)
