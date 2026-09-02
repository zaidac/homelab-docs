# Cloudflared

**IP:** `192.168.1.51`
**Corre en:** contenedor sobre Proxmox (192.168.1.50)

## Qué es

Cliente de Cloudflare Tunnel. Permite exponer servicios internos del homelab hacia internet **sin abrir puertos en el router** ni exponer la IP pública directamente, ya que el tráfico sale por un túnel saliente hacia la red de Cloudflare.

## Por qué Cloudflared

Lo elegí para poder **acceder remotamente a las interfaces de administración (Proxmox y Technitium) sin exponer el router a internet**: nada de port forwarding, nada de NAT, y la IP pública de mi casa nunca queda expuesta. Todo el tráfico sale por un túnel saliente hacia Cloudflare, que se encarga de la terminación TLS de cara al usuario.

## Configuración a alto nivel

**Servicios expuestos vía túnel:**

| Hostname público                  | Destino interno    | Servicio  |
|------------------------------------|---------------------|-----------|
| `acer1.corvexdev.com`              | `192.168.1.50` (HTTPS) | Interfaz web de Proxmox |
| `dns-server.corvexdev.com`         | `192.168.1.60` (HTTPS) | Interfaz web de Technitium |

Ambos destinos se configuraron con **"No TLS Verify" en el origen** (`originServerName`/`noTLSVerify`), ya que los certificados de las interfaces internas de Proxmox y Technitium son autofirmados. Esto es aceptable en este caso porque el tramo sin verificación queda contenido dentro de la LAN — el usuario final igual navega por HTTPS válido hacia Cloudflare.

**Protección de acceso:** ambos hostnames están detrás de una **política de Cloudflare Access** configurada para permitir el acceso únicamente a tu email, mediante un código de un solo uso (OTP) enviado por correo antes de llegar al servicio. Esto agrega una capa de autenticación *delante* del login propio de Proxmox/Technitium, así que incluso si alguien encontrara el hostname, no puede ni ver la pantalla de login sin pasar el Access primero.

## Problemas encontrados

Ninguno hasta el momento, aunque todavía no hay tráfico DNS pasando por el túnel — Technitium se resuelve solo internamente por ahora (ver roadmap).

## Próximos pasos para este componente

- [ ] Evaluar si conviene exponer resolución DNS a través del túnel (o mantenerla solo interna por diseño)
- [ ] Revisar rotación/expiración del certificado del túnel

## Próximos pasos para este componente

- [ ] Documentar todos los hostnames expuestos y a qué apuntan
- [ ] Evaluar Cloudflare Access para servicios sensibles (auth adicional antes de llegar al servicio)
