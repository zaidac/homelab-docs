# Homelab Docs

Documentación de mi homelab personal, orientado a practicar y demostrar skills de **gestión de redes / infraestructura IT**.

## Objetivo

Este homelab arrancó como un entorno de pruebas sobre Proxmox para experimentar con virtualización, DNS propio y exposición segura de servicios sin abrir puertos en el router. La idea es irlo documentando a medida que crece.

## Arquitectura actual

```
        Router (192.168.1.1) — DHCP entrega 192.168.1.60 como DNS
              |
     Proxmox - Acer One (192.168.1.50)  [2 vCPU / 2GB RAM / 120GB HDD]
        /                    \
Technitium DNS          Cloudflared
(192.168.1.60)          (192.168.1.51)
      |                        |
Cloudflare (DoH,           acer1.corvexdev.com → 192.168.1.50
filtrado ads/           dns-server.corvexdev.com → 192.168.1.60
malware/tracking)       (protegido por Cloudflare Access + OTP por email)
```

Diagrama completo (editable) en [`diagrams/topology.drawio`](diagrams/topology.drawio) — se abre en [app.diagrams.net](https://app.diagrams.net) o con la extensión de VS Code.

| Componente  | Rol                                              | IP             |
|-------------|---------------------------------------------------|----------------|
| Router      | Gateway de la red local, DHCP                      | 192.168.1.1    |
| Proxmox     | Hipervisor (LXC), corre en un Acer One             | 192.168.1.50   |
| Technitium  | Servidor DNS propio, forwarders vía DoH a Cloudflare  | 192.168.1.60   |
| Cloudflared | Túnel de Cloudflare, expone interfaces admin bajo `*.corvexdev.com` | 192.168.1.51 |

**Dominio:** `*.corvexdev.com` (subdominios para cada servicio expuesto vía túnel).

## Índice de notas

- [Proxmox](notes/proxmox.md)
- [Technitium DNS](notes/technitium.md)
- [Cloudflared](notes/cloudflared.md)

## Configs

En [`configs/`](configs/) van los archivos de configuración relevantes, (ver `configs/README.md`).

## Próximos pasos (roadmap)

- [ ] Segmentar la red con VLANs (IoT, servidores, gestión, invitados) y firewall entre ellas
- [ ] Sumar monitoreo (Prometheus + Grafana o Uptime Kuma)
- [ ] Automatizar el aprovisionamiento con Ansible
- [ ] Versionar la infraestructura con Terraform/OpenTofu
- [ ] Agregar VPN (Wireguard o Tailscale) para acceso remoto
- [ ] Escribir posts explicando cada avance

---
*Última actualización: Change forawarder to Cloudflare.*
