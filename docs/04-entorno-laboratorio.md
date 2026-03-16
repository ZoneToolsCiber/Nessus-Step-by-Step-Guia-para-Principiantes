# 🧪 Configuración del Entorno de Laboratorio

← [Volver al índice](../README.md)

---

Todo el análisis se realizó desde una máquina **Windows 11** con Nessus Expert instalado directamente sobre el sistema operativo — no desde una VM de Kali Linux. Las máquinas objetivo corren como máquinas virtuales en la misma red que el host Windows.

> [!NOTE]
> Aunque el entorno teórico descrito en otros documentos usa Kali Linux como plataforma del analista, **en la práctica real de este proyecto se utilizó Windows 11**. Nessus Expert está disponible para ambos sistemas y funciona de forma idéntica — la diferencia es únicamente cómo se instala y cómo se gestiona el servicio (ver [`03-instalacion.md`](03-instalacion.md)).

---

## Arquitectura de red

```
┌──────────────────────────────────────────────────────────────┐
│                  Red Interna / NAT Network                   │
│                       192.168.1.0/24                         │
│                                                              │
│  ┌───────────────────┐       ┌──────────────────────────┐   │
│  │   Windows 11      │       │   Hosts objetivo (VMs)   │   │
│  │   (Analista)      │◄─────►│                          │   │
│  │   192.168.1.134   │       │  192.168.1.134 (Win11)   │   │
│  │                   │       │  192.168.1.165 (Linux)   │   │
│  │  Nessus Expert    │       │  192.168.1.163 (Linux)   │   │
│  │  :8834            │       │                          │   │
│  └───────────────────┘       └──────────────────────────┘   │
└──────────────────────────────────────────────────────────────┘
```

> [!NOTE]
> El host `192.168.1.134` es la misma máquina Windows 11 donde está instalado Nessus — es decir, Nessus se escaneó a sí mismo además de a los otros dos hosts. Esto es una práctica válida para analizar la propia máquina de trabajo.

---

## Máquina Analista — Windows 11

| Parámetro | Valor |
|---|---|
| **Sistema Operativo** | Windows 11 |
| **Herramienta** | Nessus Expert v10.x (trial 7 días) |
| **Configuración de red** | Red local / NAT |
| **IP** | `192.168.1.134` |
| **Interfaz Nessus** | `https://localhost:8834` |

---

## Máquinas Objetivo — Targets

| Host | Sistema Operativo | Servicios destacados |
|---|---|---|
| `192.168.1.134` | Windows 11 (misma máquina analista) | XAMPP (Apache, MySQL) activos durante el escaneo |
| `192.168.1.165` | Linux (Metasploitable 2 / Ubuntu) | Samba, NFS, Telnet, OpenSSH |
| `192.168.1.163` | Linux (Ubuntu 14.04 EOL) | Apache Tomcat, OpenSSH, SMB |

> [!NOTE]
> **XAMPP:** antes de lanzar el escaneo se activó XAMPP en la máquina Windows para levantar servicios de Apache y MySQL y poder ver resultados más completos en el análisis del host local.

> [!NOTE]
> **¿Por qué Metasploitable 2?** Es una máquina virtual diseñada específicamente para ser vulnerable, usada en entornos de aprendizaje. Viene con servicios intencionalmente desactualizados y configuraciones inseguras que permiten practicar análisis y explotación sin riesgo legal ni ético.
> Descargable en: [sourceforge.net/projects/metasploitable](https://sourceforge.net/projects/metasploitable/)

---

## Configuración de red en VirtualBox / VMware

Para que ambas máquinas se vean entre sí, deben estar en la **misma red**. Hay dos opciones:

**Opción A — NAT Network (recomendada):**
```
VirtualBox: Archivo → Preferencias → Red → Añadir NAT Network
→ Asignar la misma NAT Network a ambas VMs en su configuración de red
```

**Opción B — Red Interna:**
```
En la configuración de cada VM:
→ Adaptador de red → Conectado a: Red Interna
→ Nombre: intnet (el mismo en ambas)
```

> [!TIP]
> La opción NAT Network permite también que las VMs tengan acceso a internet (para actualizaciones), mientras que la Red Interna es completamente aislada.

---

## Verificación de conectividad

Antes de lanzar cualquier escaneo, confirmar que ambas máquinas se ven:

```bash
# Desde Kali Linux — comprobar visibilidad del target
ping -c 4 192.168.1.50
```

Salida esperada:
```
PING 192.168.1.50 (192.168.1.50): 56 bytes de datos
64 bytes de 192.168.1.50: icmp_seq=0 ttl=64 time=0.8 ms
64 bytes de 192.168.1.50: icmp_seq=1 ttl=64 time=0.6 ms
--- 192.168.1.50 ping statistics ---
4 packets transmitted, 4 received, 0% packet loss
```

```bash
# Reconocimiento previo con nmap (referencia antes de Nessus)
nmap -sV 192.168.1.50
```

> [!TIP]
> 📸 **Captura recomendada:** resultado del `ping` y del `nmap` mostrando los servicios detectados antes de lanzar Nessus.

---

← [Volver al índice](../README.md) · Siguiente: [Tipos de escaneo →](05-tipos-escaneo.md)
