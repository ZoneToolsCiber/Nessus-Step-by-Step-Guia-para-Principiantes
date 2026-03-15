# 🩹 Plan de Remediación

← [Volver al índice](../README.md)

---

El plan de remediación recoge las acciones tomadas para corregir cada hallazgo detectado y verifica su efectividad mediante un **re-scan** posterior. Sin re-scan no hay evidencia objetiva de que el problema fue resuelto.

---

## Acción 1 — Actualización de Apache

**Hallazgo que resuelve:** Apache HTTP Server RCE (CVE-2021-41773)
**Severidad:** 🔴 Crítico

```bash
# Actualizar Apache a la versión más reciente disponible
sudo apt-get update
sudo apt-get install --only-upgrade apache2

# Verificar que la versión ha cambiado
apache2 -v
```

**Resultado esperado del re-scan:** el plugin `Apache HTTP Server < 2.4.50 Multiple Vulns` desaparece de los hallazgos críticos.

---

## Acción 2 — Eliminación de vsftpd 2.3.4

**Hallazgo que resuelve:** vsftpd 2.3.4 Backdoor (CVE-2011-2523)
**Severidad:** 🔴 Crítico

```bash
# Eliminar la versión con backdoor
sudo apt-get remove vsftpd

# Instalar una versión limpia desde el repositorio oficial
sudo apt-get install vsftpd

# Verificar la nueva versión
vsftpd -v
```

**Resultado esperado del re-scan:** el plugin `vsftpd Backdoor` desaparece.

---

## Acción 3 — Desactivar login root en SSH

**Hallazgo que resuelve:** SSH Root Login Enabled
**Severidad:** 🟡 Medio

```bash
# Editar la configuración de SSH
sudo nano /etc/ssh/sshd_config

# Localizar y modificar la línea:
# PermitRootLogin yes  →  PermitRootLogin no

# Aplicar los cambios reiniciando el servicio
sudo systemctl restart sshd

# Verificar que el cambio se aplicó
grep PermitRootLogin /etc/ssh/sshd_config
```

**Resultado esperado del re-scan:** el hallazgo `SSH Root Login Enabled` desaparece.

---

## Acción 4 — Cierre de puertos innecesarios con iptables

**Hallazgo que resuelve:** MySQL 3306 expuesto externamente
**Severidad:** 🟠 Alto

```bash
# Bloquear acceso externo al puerto de MySQL
sudo iptables -A INPUT -p tcp --dport 3306 -j DROP

# Verificar que la regla se aplicó
sudo iptables -L INPUT -n -v

# Hacer la regla persistente entre reinicios
sudo apt-get install iptables-persistent
sudo iptables-save > /etc/iptables/rules.v4
```

**Resultado esperado del re-scan:** el puerto 3306 ya no aparece accesible externamente en los hallazgos.

---

## Acción 5 — Control de tráfico con tc (QDISC HTB)

**Hallazgo que resuelve:** ausencia de límites de tráfico — exposición a DoS
**Severidad:** relacionado con la Tarea 1 del módulo

```bash
# Configurar control de tráfico HTB en la interfaz de red
sudo tc qdisc add dev eth0 root handle 1: htb default 30
sudo tc class add dev eth0 parent 1: classid 1:1 htb rate 100mbit
sudo tc class add dev eth0 parent 1:1 classid 1:30 htb rate 10mbit ceil 100mbit

# Verificar que el qdisc está activo
sudo tc qdisc show dev eth0
```

---

## Resultado del re-scan

Tras aplicar todas las acciones de remediación, se realizó un nuevo escaneo completo con la misma política y contra el mismo objetivo.

| Hallazgo | Estado antes | Estado después |
|---|:---:|:---:|
| Apache RCE (CVE-2021-41773) | 🔴 Crítico | ✅ Resuelto |
| vsftpd Backdoor (CVE-2011-2523) | 🔴 Crítico | ✅ Resuelto |
| MySQL puerto expuesto | 🟠 Alto | ✅ Resuelto |
| SSH Root Login | 🟡 Medio | ✅ Resuelto |

> [!TIP]
> 📸 **Captura recomendada:** comparativa lado a lado del primer escaneo (con hallazgos críticos) y el re-scan (hallazgos resueltos). Esta comparativa es la **evidencia objetiva** de la remediación.

---

## Lecciones aprendidas

- **Un escaneo sin re-scan no está completo.** La remediación solo puede darse por válida cuando un segundo escaneo confirma la desaparición del hallazgo.
- **Los servicios EOL son una deuda técnica de seguridad.** MySQL 5.0 y Apache 2.2.8 no reciben parches — cualquier vulnerabilidad futura en esas versiones nunca será corregida por el fabricante.
- **La exposición de servicios internos es evitable.** MySQL no debería ser accesible desde fuera del servidor — un simple firewall elimina ese vector de ataque.
- **La configuración por defecto es insegura.** `PermitRootLogin yes` viene habilitado en sistemas antiguos. El hardening básico es la primera línea de defensa.

---

← [Volver al índice](../README.md)
