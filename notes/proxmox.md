# Proxmox VE

**IP:** `192.168.1.50`
**Host físico:** Acer One (Laptop)

## Qué es

Hipervisor tipo 1 (bare-metal) que uso como base de todo el homelab. Corre sobre una laptop Acer One y aloja tanto VMs como contenedores LXC/Docker donde viven el resto de los servicios (Technitium, Cloudflared, y lo que se vaya sumando).

## Por qué Proxmox

Lo elegí principalmente por ser **liviano**, algo clave considerando que corre sobre una laptop (Acer One) con recursos limitados, no un server dedicado. A diferencia de hipervisores más pesados como ESXi, Proxmox permite mezclar VMs y contenedores LXC en la misma instancia y administrar todo desde una única interfaz web, lo que simplifica mucho la gestión cuando el objetivo es correr varios servicios pequeños en paralelo sin desperdiciar recursos en overhead de virtualización completa.

## Configuración a alto nivel

**Specs del host (Acer One):**

| Recurso | Cantidad |
|---------|----------|
| CPU     | 2 cores  |
| RAM     | 2 GB     |
| Storage | 120 GB (HDD) |

El particionado del storage quedó con el esquema por defecto de la instalación de Proxmox (LVM-thin sobre el disco completo), sin personalización adicional todavía.

**Virtualización:** por ahora, únicamente **LXC** (contenedores), sin VMs completas. Tiene sentido dado el hardware: con solo 2GB de RAM, los contenedores LXC son mucho más eficientes que VMs porque comparten el kernel del host en vez de reservar memoria dedicada para cada uno — permite correr Technitium y Cloudflared (y lo que se sume después) sin agotar recursos.

## Problemas encontrados

Sin problemas hasta el momento. El host viene siendo estable corriendo los contenedores actuales (Technitium y Cloudflared) sin issues de rendimiento ni caídas.

> *Nota: con 2GB de RAM, a medida que sume más servicios (monitoreo, reverse proxy, etc.) va a ser importante vigilar el uso de memoria — es el recurso más ajustado del setup actual.*

## Próximos pasos para este componente

- [ ] Configurar backups automáticos (Proxmox Backup Server o snapshots programados)
- [ ] Evaluar si conviene separar más servicios en contenedores independientes
