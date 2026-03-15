# 🧪 Configuración del Entorno de Laboratorio

← [Volver al índice](../README.md)

---

Todo el entorno corre sobre **VirtualBox** o **VMware**, con ambas máquinas en la misma red privada para que puedan comunicarse sin exponer tráfico al exterior.

---

## Arquitectura de red

```
┌──────────────────────────────────────────────────────────┐
│               Red Interna / NAT Network                  │
│                    192.168.1.0/24                        │
│                                                          │
│  ┌──────────────────┐         ┌─────────────────────┐   │
│  │   Kali Linux     │         │   Metasploitable 2  │   │
│  │   (Analista)     │◄───────►│   (Target)          │   │
│  │   192.168.1.10   │         │   192.168.1.50      │   │
│  │                  │         │                     │   │
│  │  Nessus Expert   │         │  Apache  2.2.8      │   │
│  │  :8834           │         │  MySQL   5.0        │   │
│  └──────────────────┘         │  vsftpd  2.3.4      │   │
│                               │  OpenSSH 4.7        │   │
│                               └─────────────────────┘   │
└──────────────────────────────────────────────────────────┘
```

---

## Máquina Atacante — Analista (Kali Linux)

| Parámetro | Valor |
|---|---|
| **Sistema Operativo** | Kali Linux 2023.x |
| **Herramienta** | Nessus Expert v10.x |
| **Adaptador de red** | NAT Network / Red Interna |
| **IP** | `192.168.1.10` |
| **Interfaz Nessus** | `https://localhost:8834` |

---

## Máquina Objetivo — Target (Metasploitable 2)

| Parámetro | Valor |
|---|---|
| **Sistema Operativo** | Metasploitable 2 (Ubuntu 8.04) |
| **Servicios activos** | Apache 2.2.8, MySQL 5.0, vsftpd 2.3.4, OpenSSH 4.7 |
| **Adaptador de red** | NAT Network / Red Interna |
| **IP** | `192.168.1.50` |

> [!NOTE]
> **¿Por qué Metasploitable 2?** Es una máquina virtual diseñada específicamente para ser vulnerable, usada en entornos de aprendizaje. Viene con servicios intencionalmente desactualizados y configuraciones inseguras que permiten practicar análisis y explotación sin riesgo legal ni ético.
>
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
