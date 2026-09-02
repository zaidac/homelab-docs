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

**Forwarders (DNS-over-HTTPS):** el servidor reenvía las consultas externas a dos perfiles de **NextDNS** vía DoH:

```
https://dns.nextdns.io/<MY_NEXTDNS_PROFILE_ID>/DNS-SERVER (NEXTDNS_IP_SERVER_1)
https://dns.nextdns.io/<MY_NEXTDNS_PROFILE_ID>/DNS-SERVER (NEXTDNS_IP_SERVER_2)
```

Estos perfiles tienen configurada una lista amplia de bloqueo de dominios de ads, tracking y malware, delegando el filtrado a NextDNS en vez de mantener listas propias en Technitium.


## Problemas encontrados

Sin problemas hasta el momento con la resolución DNS ni con el reenvío vía DoH a NextDNS.

## Distribución a los clientes

El router (`192.168.1.1`) distribuye `192.168.1.60` como servidor DNS a través de su servidor DHCP, por lo que todos los dispositivos de la red reciben automáticamente a Technitium como su resolver, sin necesidad de configuración manual por dispositivo.

## Próximos pasos para este componente

- [ ] Documentar las zonas/registros configurados
- [ ] Evaluar una segunda instancia para alta disponibilidad (DNS redundante)
