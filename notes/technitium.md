# Technitium DNS Server

**IP:** `192.168.1.60`
**Corre en:** contenedor sobre Proxmox (192.168.1.50)

## Qué es

Servidor DNS propio de código abierto, con interfaz web de administración. Actúa como resolver DNS para la red local, en lugar de usar el DNS del ISP o un DNS público directo.

## Por qué Technitium

A diferencia de herramientas como Pi-hole, que se enfocan casi exclusivamente en bloqueo de ads, elegí Technitium porque ofrece **control total sobre el servidor DNS**: es un servidor DNS completo (con soporte de zonas propias, forwarders configurables, DNS-over-HTTPS/TLS, API REST, y un sistema de apps/plugins), no solo una capa de filtrado sobre `dnsmasq`. Esto lo hace más versátil a medida que el homelab crece, porque puedo manejar resolución interna y filtrado externo desde el mismo lugar.

## Configuración a alto nivel

**Registros locales (zona interna):**

| Hostname          | IP             |
|-------------------|----------------|
| `acer1.lab`      | 192.168.1.50   |
| `dns-server.lab` | 192.168.1.60   |

Esto permite resolver los servicios internos por nombre en vez de memorizar IPs, algo que va a ser cada vez más útil a medida que sume más contenedores.

**Forwarders (DNS-over-HTTPS):** el servidor reenvía las consultas externas a los servidores de **Cloudflare** vía DoH:

```
https://cloudflare-dns.com/dns-query (1.1.1.1)
https://cloudflare-dns.com/dns-query (1.0.0.1)
```
## Block Lists

Por el momento solo agregue 2 block lists pero siendo de las mas grandes y robustas, **AdGuard DNS Filter** y **OISD**. Para el bloqueo de ADs, malware, tracking, adware, etc.

```
https://adguardteam.github.io/AdGuardSDNSFilter/Filters/filter.txt
https://big.oisd.nl/domainswild2
```

## Problemas encontrados

Anteriormente se utilizaba a NextDNS como forwarder via DoH, configurando las listas de bloqueo directamente en NextDNS pero al limitar las consultas a 300.000 por cuenta, conveni cambiar de forwarder a Cloudflare y configurar las listas de bloqueos en Technitium directamente

## Distribución a los clientes

El router (`192.168.1.1`) distribuye `192.168.1.60` como servidor DNS a través de su servidor DHCP, por lo que todos los dispositivos de la red reciben automáticamente a Technitium como su resolver, sin necesidad de configuración manual por dispositivo.

## Próximos pasos para este componente

- [ ] Documentar las zonas/registros configurados
- [ ] Evaluar una segunda instancia para alta disponibilidad (DNS redundante)
