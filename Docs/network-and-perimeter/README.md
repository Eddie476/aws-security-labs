# 🌐 Arquitectura de Red y Perímetro (On-Premise)

Esta sección documenta la capa de red principal del laboratorio local, abarcando la segmentación mediante VLANs, políticas de firewall en pfSense, filtrado DNS y la publicación segura de servicios web.

---

## 1. Segmentación de Red (VLANs)

Para limitar el radio de impacto (*blast radius*) en caso de un compromiso de red, la infraestructura local está dividida estrictamente mediante VLANs gestionadas desde **pfSense**:

| ID VLAN | Nombre | Subred | Propósito / Alcance |
| :--- | :--- | :--- | :--- |
| **VLAN 10** | Management | `192.168.10.0/24` | Interfaces de administración (Proxmox, pfSense GUI, Switches). |
| **VLAN 20** | Services | `192.168.20.0/24` | Servicios internos y Nginx Reverse Proxy. |
| **VLAN 30** | Security | `192.168.30.0/24` | Infraestructura de monitoreo (Wazuh SIEM Manager). |
| **VLAN 40** | IoT / Isolation | `192.168.40.0/24` | Dispositivos inteligentes (Home Assistant) sin acceso a otras VLANs. |

---

## 2. Firewall & Políticas de Tráfico (pfSense)

Se aplica una estrategia de **Denegación por Defecto (*Default Deny*)**. Las reglas principales de filtrado son:

1. **Bloqueo Inter-VLAN:** Ninguna subred de menor confianza (ej. VLAN 40) puede iniciar conexiones hacia la subred de administración (VLAN 10) ni de seguridad (VLAN 30).
2. **Acceso al SIEM:** Solo los agentes autorizados en la VLAN 20 y VLAN 10 tienen permitido enviar logs por el puerto `1514/TCP` hacia la VLAN 30 (Wazuh).
3. **Egresos a Internet:** Limitados a tráfico HTTP/HTTPS y consultas DNS dirigidas exclusivamente al resolvedor interno (AdGuard Home).

---

## 3. Filtrado DNS (AdGuard Home)

**AdGuard Home** actúa como el primer nivel de defensa en la capa de red:

* **Sinkhole DNS:** Bloqueo de dominios de maliciosos, botnets y rastreadores mediante listas de reputación actualizadas.
* **Redirección Estricta:** pfSense redirige todas las consultas salientes del puerto `53/UDP` hacia AdGuard para evitar que dispositivos individuales salten la protección DNS.

---

## 4. Reverse Proxy & Certificados SSL (Nginx)

* **Proxy Inverso:** Nginx gestiona la entrada de tráfico web hacia los servicios internos de la VLAN 20.
* **Terminación TLS:** Garantiza el cifrado de extremo a extremo en las comunicaciones administrativas internas mediante certificados SSL/TLS.
* **Cierre de Puertos:** Se elimina la necesidad de exponer puertos individuales de aplicaciones a la red local.

---

## 📸 Evidencias y Diagramas

*(Añade aquí tus capturas de pantalla de la interfaz de pfSense, tablas de reglas o diagramas de Draw.io)*

* `docs/network-and-perimeter/images/pfsense-vlan-rules.png`
* `docs/network-and-perimeter/images/adguard-dashboard.png`
