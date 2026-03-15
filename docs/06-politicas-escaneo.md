# 🔧 Políticas de Escaneo Aplicadas

← [Volver al índice](../README.md)

---

En este proyecto se configuró una política personalizada basada en **Basic Network Scan** con modificaciones específicas para maximizar la detección de vulnerabilidades en Metasploitable 2 sin comprometer la estabilidad del objetivo.

---

## Configuración general

### Cobertura de puertos

- **Scan Type:** Long — escaneo de los **65.535 puertos TCP** completos.

Por defecto, Nessus solo escanea los 1.000 puertos más comunes. Cambiar a `Long` garantiza que ningún servicio en un puerto no estándar quede sin detectar. En Metasploitable, algunos servicios vulnerables corren en puertos altos.

### Detección de servicios

- Activada la búsqueda de **banners de servicios** para identificar versiones exactas de Apache, MySQL y vsftpd.
- **Service Detection** habilitado en **todos los puertos**, no solo en los well-known (< 1024).

---

## Enumeración de usuarios SSH

Una de las configuraciones manuales aplicadas fue habilitar la **enumeración de usuarios SSH** en:

```
Settings → Assessment → Brute Force → SSH
→ Activar: "Attempt to enumerate valid SSH usernames"
```

Esto permite a Nessus verificar si el servidor SSH revela si un nombre de usuario es válido o no mediante diferencias en los tiempos de respuesta del protocolo — un vector previo crítico en ataques de fuerza bruta dirigida.

> [!NOTE]
> En OpenSSH versiones antiguas (como la 4.7 de Metasploitable), esta enumeración es posible porque el servidor tarda más en responder a usuarios inexistentes que a usuarios válidos. Las versiones modernas de OpenSSH mitigan este comportamiento.

---

## Consideraciones de rendimiento

Para no saturar la CPU de Metasploitable, que tiene recursos muy limitados, se redujo el número de comprobaciones simultáneas:

```
Settings → Performance
→ Max simultaneous checks per host: 5 (por defecto: 30)
→ Max simultaneous hosts per scan: 1
```

Sin este ajuste, Nessus puede enviar tantas peticiones simultáneas que el objetivo deja de responder y el escaneo no se completa correctamente — especialmente en VMs con poca RAM.

> [!TIP]
> 📸 **Captura recomendada:** pantalla de configuración de la política en Nessus mostrando los ajustes de puertos, detección de servicios y rendimiento.

---

## Resumen de la política aplicada

| Parámetro | Valor configurado | Valor por defecto |
|---|---|---|
| Cobertura de puertos | 65.535 (Long) | Top 1.000 |
| Service Detection | Todos los puertos | Solo well-known |
| Banner grabbing | Activado | Activado |
| Enumeración SSH | Activada | Desactivada |
| Max checks simultáneos | 5 | 30 |
| Max hosts simultáneos | 1 | 5 |

---

← [Volver al índice](../README.md) · Siguiente: [Cómo leer un reporte →](07-como-leer-reporte.md)
