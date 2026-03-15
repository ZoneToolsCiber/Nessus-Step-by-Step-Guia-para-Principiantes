# ⚙️ Instalación de Nessus Expert

← [Volver al índice](../README.md)

---

## Requisitos previos

| Componente | Mínimo recomendado |
|---|---|
| Sistema operativo | Kali Linux / Ubuntu 20.04+ / Debian 11+ |
| RAM | 4 GB (8 GB recomendado) |
| Almacenamiento | 30 GB libres |
| CPU | 4 núcleos |
| Navegador | Chrome / Firefox |
| Conectividad | Acceso a internet para activación y actualizaciones de plugins |

---

## Paso 1 — Obtener la licencia de activación

1. Ir a [tenable.com/products/nessus](https://www.tenable.com/products/nessus) y seleccionar **Nessus Expert**.
2. Registrarse con una cuenta y completar el proceso de compra o solicitud de prueba gratuita.
3. Anotar el **Activation Code** que llegará por email — se necesitará en el Paso 4.

---

## Paso 2 — Descargar el paquete

Desde la [página de descargas de Tenable](https://www.tenable.com/downloads/nessus), seleccionar el paquete para el sistema operativo. Para Kali Linux / Debian:

```bash
# Descargar el paquete .deb (sustituir X.X.X por la versión actual)
wget https://www.tenable.com/downloads/api/v1/public/pages/nessus/downloads/XXXXX/get \
     -O Nessus-X.X.X-debian10_amd64.deb
```

> [!TIP]
> Es más cómodo descargarlo directamente desde el navegador en la página de Tenable, que siempre muestra la versión más reciente, y transferirlo a la VM si es necesario.

---

## Paso 3 — Instalar el paquete

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

---

## Paso 4 — Activar y configurar desde el navegador

1. Abrir el navegador y acceder a: `https://localhost:8834`
2. Aceptar el aviso de certificado autofirmado (es normal — Nessus usa HTTPS con certificado propio).
3. En la pantalla de bienvenida, seleccionar **Nessus Expert**.
4. Introducir el **Activation Code** recibido por email.
5. Crear el usuario administrador (nombre de usuario y contraseña).
6. Esperar a que Nessus descargue e instale todos los plugins.

> [!IMPORTANT]
> La descarga de plugins puede tardar entre **10 y 30 minutos** dependiendo de la conexión. **No cerrar el navegador ni reiniciar el servicio** durante este proceso. Si se interrumpe, ejecutar `sudo systemctl restart nessusd` y acceder de nuevo para retomarlo.

---

## Paso 5 — Verificar la instalación

Una vez completada la descarga de plugins, la interfaz mostrará el dashboard principal. Para confirmar que la versión activa es Expert:

```
Menú superior derecho → tu usuario → About
→ Debe mostrar: "Nessus Expert" con la fecha de expiración de la licencia
```

> [!TIP]
> 📸 **Captura recomendada:** pantalla *About* de Nessus confirmando la versión Expert y la licencia activa.

---

## Solución de problemas comunes

| Problema | Causa probable | Solución |
|---|---|---|
| El navegador no carga `localhost:8834` | El servicio no está corriendo | `sudo systemctl start nessusd` |
| La activación falla con "Invalid code" | El código ya fue usado o está caducado | Solicitar un nuevo código en el portal de Tenable |
| Los plugins no se descargan | Sin conexión a internet o firewall bloqueando | Verificar conectividad: `curl -I https://plugins.nessus.org` |
| El escaneo no detecta el objetivo | El target no responde a ping | Deshabilitar "Ping the remote host" en opciones del escaneo |

---

← [Volver al índice](../README.md) · Siguiente: [Entorno de laboratorio →](04-entorno-laboratorio.md)
