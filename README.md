# aws-security-labs
# 🛡️ Enterprise Hybrid Cloud & Security Operations Lab

![Maintenance](https://img.shields.io/badge/Maintained%3F-yes-green.svg)
![Environment](https://img.shields.io/badge/Environment-On--Premise%20%7C%20AWS%20Hybrid-orange)
![Security Focus](https://img.shields.io/badge/Focus-Blue%20Team%20%26%20Cloud%20Hardening-blue)

## 📌 Visión General del Proyecto

Este repositorio documenta el diseño, implementación y hardening de un entorno de red empresarial híbrido. El proyecto está dividido en dos fases principales:

1. **Infraestructura On-Premise (Home Lab):** Segmentación de red estricta, filtrado DNS, proxy inverso, acceso remoto zero-trust y monitoreo centralizado con un SIEM local.
2. **Infraestructura en Nube (AWS):** Extensión de la red mediante conectividad híbrida, aplicación del principio de menor privilegio (IAM), auditoría centralizada de llamadas a la API, detección de amenazas mediante Machine Learning y remediación automatizada de eventos de seguridad.

El objetivo central es simular la postura de seguridad defensiva (*Blue Team*) de un entorno corporativo real, aplicando marcos de trabajo y buenas prácticas de la industria.

---

## 📐 Arquitectura Global del Sistema
                  +------------------------------------------+
                  |         USUARIO REMOTO (Tailscale)       |
                  +--------------------+---------------------+
                                       |
                                       v
              [ INTERNET ] <---> [ WAN (pfSense) ] <---------------+
                                       |
      +---------------------+---------------------+---------------------+---------------------+---------------------+
      |                     |                     |                     |                     |                     |
      v                     v                     v                     v                     v                     v
    [ VLAN 10 ]         [ VLAN 20 ]          [ VLAN 30 ]           [ VLAN 40 ]           [ VLAN 50 ]           [ VLAN 90 ]
     Management          Services           Security (SIEM)       IoT / Isolation          Clients                Guess

    - Proxmox VE       - AdGuard Home      - Nginx Proxy Mgr     - Home Assistant      - Users Clients     - External devices
    - PfSense          - Wazuh Manager
    - Tailscale
    - Homelable

---

## 🗂️ Estructura de la Documentación

### 🔹 Fase 1: Entorno On-Premise
* [**Arquitectura de Red y Perímetro**](./docs/network-and-perimeter/): Configuración de pfSense (VLANs, Aliases, Firewall Rules), AdGuard Home y Nginx Reverse Proxy.
* [**Acceso Remoto Seguro (Zero Trust)**](./docs/remote-access/): Integración de Tailscale con ACLs estrictas para gestión de la red.
* [**Monitoreo y Detección (SIEM)**](./docs/monitoring-and-siem/): Despliegue de Wazuh SIEM, instalación de agentes y recolección de logs.
* [**Aislamiento y Automatización**](./docs/automation-and-lab/): Segmentación de dispositivos IoT (Home Assistant) y monitoreo de estado.

### 🔸 Fase 2: Entorno Cloud (AWS - En progreso)
* [**Hardening de Cuenta e IAM**](./docs/aws-iam-hardening/): Configuración de alertas de presupuesto, MFA y políticas JSON de menor privilegio.
* [**Redes en la Nube (VPC & Hybrid)**](./docs/aws-vpc-hybrid/): Subredes públicas/privadas, Security Groups y túnel de comunicación con pfSense.
* [**Integración de Telemetría**](./docs/aws-telemetry-wazuh/): Exportación de AWS CloudTrail a S3 y consumo de alertas en Wazuh local.

---

## 🛠️ Tecnologías y Herramientas Utilizadas

| Categoría | Herramientas / Servicios |
| :--- | :--- |
| **Virtualización / Hypervisor** | Proxmox VE |
| **Firewall / Routing** | pfSense |
| **DNS / Proxy** | AdGuard Home, Nginx Reverse Proxy |
| **Acceso Seguro** | Tailscale (Overlay VPN) |
| **SIEM / Telemetría** | Wazuh SIEM, Syslog |
| **Cloud Provider** | AWS (IAM, VPC, CloudTrail, GuardDuty, S3) |
| **Infraestructura como Código** | Terraform *(Próximamente)* |

---

## 👤 Autor

* **Contacto:** [Eddie Navarrete / [Linkedin Eddie Navarrete](https://www.linkedin.com/in/eddie-navarrete-389b4a232/)
* **Objetivo Profesional:** AWS Cloud Security Engineer / Junior Cloud Security Analyst.

