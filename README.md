# Raspberry Pi Pi-hole & WireGuard VPN Lab

A network privacy and security lab built on a Raspberry Pi. Deploys Pi-hole as a DNS sinkhole to block ads, trackers, and malicious domains network-wide, integrated with WireGuard VPN for encrypted remote access.

---

## Environment

- **Hardware:** Raspberry Pi
- **OS:** Raspberry Pi OS (Linux)
- **Tools:** Pi-hole, WireGuard VPN

---

## What This Does

### Pi-hole — DNS Sinkhole

Pi-hole intercepts DNS queries across the entire network and blocks requests to known ad servers, trackers, and malicious domains at the DNS level — before any connection is made. This improves:

- Network privacy — stops trackers from phoning home
- Security posture — blocks known malicious domains
- Network visibility — every DNS query is logged and queryable

### WireGuard VPN

WireGuard provides encrypted remote access back to the home network. Configured with firewall rules and port forwarding to enforce least-privilege connectivity — only authorized devices can tunnel in.

---

## Setup Overview

### Pi-hole Installation

    curl -sSL https://install.pi-hole.net | bash

- Set Pi-hole as your network's DNS server via router DHCP settings
- Configure upstream DNS (Cloudflare 1.1.1.1 or similar)
- Enable DNSSEC for additional security

### WireGuard Installation

    sudo apt install wireguard

Server config (/etc/wireguard/wg0.conf):

    [Interface]
    Address = 10.0.0.1/24
    ListenPort = 51820
    PrivateKey = <server_private_key>

    [Peer]
    PublicKey = <client_public_key>
    AllowedIPs = 10.0.0.2/32

Firewall rules:

    sudo ufw allow 51820/udp
    echo "net.ipv4.ip_forward=1" | sudo tee -a /etc/sysctl.conf
    sudo sysctl -p

### Port Forwarding

Configure your router to forward UDP port 51820 to your Raspberry Pi's local IP address.

---

## Security Considerations

- WireGuard uses modern cryptography — ChaCha20, Poly1305, Curve25519
- Pi-hole blocks DNS-over-HTTPS bypass attempts via blocklists
- Firewall rules enforce least-privilege — only WireGuard port exposed externally
- All DNS queries logged locally — no third-party logging of browsing activity

---

## Key Takeaways

- DNS-level blocking is more effective than browser extensions — covers all devices on the network
- WireGuard is significantly faster and simpler to configure than OpenVPN or IPSec
- Combining Pi-hole with VPN means remote devices get the same DNS protection as local devices

---

## Connect

- LinkedIn: [linkedin.com/in/faysal-dhimbil](https://linkedin.com/in/faysal-dhimbil)
- Medium: [medium.com/@faysaldhimbil87](https://medium.com/@faysaldhimbil87)
