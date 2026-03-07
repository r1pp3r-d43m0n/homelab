# 🏠 Tom's Lab Space | homelab adventures

> **A self-built, production-grade purple-team home lab** — designed for hands-on cybersecurity learning, attack simulation, and defensive tool testing. Everything here is documented as I build it.

---

## Purpose

This repo is the living documentation of my home lab. Every major build step, configuration decision, lesson learned, and "why did that break" moment gets written up here. The goal is twofold:

1. **Force myself to understand things deeply enough to explain them**
2. **Build a portfolio that shows real hands-on work, not just certifications**

The lab is intentionally purple-team: I use it to run attacks, observe what they look like in logs and SIEM alerts, refine detection rules, and harden defenses. Break it, fix it, document it, repeat.

---

## Architecture Overview

```
┌─────────────────────────────────────────────────────────┐
│                   PROXMOX VE HOST                        │
│                                                          │
│  ┌──────────────┐         ┌────────────────┐            │
│  │   Docker /   │         │  Technitium    │            │
│  │  Portainer   │   -►    │  DNS/DHCP VM   │            │
│  │  (Services)  │         │  (HA Node 2)   │            │
│  └──────────────┘         └────────────────┘            │
│                                                          │
│  ┌─────────────┐  ┌──────────────┐                       │
│  │  Splunk VM  │  │  Other VMs   │                       │
│  │  (SIEM)     │  │  (Lab/Test)  │                       │
│  └─────────────┘  └──────────────┘                       │
└─────────────────────────────────────────────────────────┘
         │ VirtioFS storage passthrough
         ▼
┌─────────────────────┐       ┌──────────────────────────┐
│   Raspberry Pi 4    │       │     NETWORK LAYER         │
│  Technitium DNS/    │       │  Caddy Reverse Proxy      │
│  DHCP (HA Node 1)   │       │  (auto TLS / ACME)        │
└─────────────────────┘       │  Tailscale Mesh VPN       │
                              │  (WireGuard / zero-trust) │
                              │  Gluetun VPN Gateway      │
                              └──────────────────────────┘
```

---

## Full Stack

### Virtualization & Compute
| Component | Technology | Notes |
|---|---|---|
| Hypervisor | **Proxmox VE** (KVM/QEMU) | Main host — runs VMs and LXC containers |
| Storage passthrough | **VirtioFS** | Host-to-guest filesystem sharing |
| Container management | **Docker + Portainer** | Containerized services stack |

### Security & Monitoring
| Component | Technology | Notes |
|---|---|---|
| SIEM | **Splunk** | Log ingestion, alerting, dashboards |
| SIEM | **Wazuh** | On the Roadmap. Deploying soon. |
| IDS/IPS | **OPNsense & Firewall Audits** |

### Networking & Access
| Component | Technology | Notes |
|---|---|---|
| DNS / DHCP | **Technitium** | HA cluster: Raspberry Pi + Proxmox VM |
| Reverse Proxy | **Caddy** | Automatic TLS via ACME / Let's Encrypt |
| Mesh VPN | **Tailscale** (WireGuard) | Zero-trust remote access with ACL policies |
| VPN Gateway | **Gluetun** | Containerized WireGuard tunnel for isolated traffic |

### Media Services Stack (Docker)
| Service | Purpose |
|---|---|
| Sonarr / Radarr | Automated media management |
| Prowlarr | Indexer management |
| Bazarr | Subtitle automation |

---

## 📁 (Planned) Repo Structure

```
homelab/
│
├── README.md                  ← You are here
│
├── architecture/
│   ├── network-diagram.md     ← Network topology and diagrams
│   └── decisions.md           ← Architecture decision log (why I chose X over Y)
│
├── proxmox/
│   ├── README.md              ← Proxmox setup overview
│   ├── vm-templates.md        ← VM/LXC templates and configs
│   └── virtiofs-setup.md      ← VirtioFS passthrough walkthrough
│
├── networking/
│   ├── README.md
│   ├── technitium-ha-cluster.md   ← DNS/DHCP HA cluster build
│   ├── caddy-reverse-proxy.md     ← Caddy + auto TLS setup
│   ├── tailscale-vpn.md           ← Tailscale mesh VPN + ACLs
│   └── gluetun-vpn-gateway.md     ← Gluetun WireGuard container
│
├── docker/
│   ├── README.md
│   ├── portainer-setup.md
│   ├── arr-stack/
│   │   ├── README.md
│   │   └── docker-compose.yml
│   └── gluetun/
│       └── docker-compose.yml
│
├── security/
│   ├── README.md
│   ├── splunk/
│   │   ├── setup.md
│   │   └── detection-rules.md
│   ├── security-onion/
│   │   └── setup.md
│   └── attack-simulations/
│       └── README.md          ← Lab attack scenarios + what the logs looked like
│
├── linux-hardening/
│   ├── README.md
│   ├── ssh-hardening.md
│   └── ufw-iptables.md
│
└── lessons-learned/
    └── README.md              ← The good stuff: what broke, why, and how I fixed it
```

---

## 📋 Build Log

| Date | Milestone |
|---|---|
| 2022 | Initial Proxmox VE host setup 🗸 |
| 2023 | Caddy reverse proxy + auto TLS 🗸 |
| 2023 | Docker + Portainer deployment 🗸 |
| 2023 | VirtioFS storage passthrough 🗸 |
| 2024 | Splunk SIEM deployment 🗸 |
| 2024 | Security Onion NSM/IDS deployment 🗸 |
| 2025 | Homarr Dashboard deployment 🗸 |
| 2025 | Arr stack (Sonarr/Radarr/Prowlarr/Bazarr) 🗸 |
| 2025 | Gluetun WireGuard VPN gateway container 🗸 |
| 2026 | Technitium DNS/DHCP HA cluster (Pi + VM) 🗸 |
| 2026 | Tailscale mesh VPN + subnet routing + split DNS + ACL policies 🗸 |
| 2026 | Currently Building: |
|  WIP | Self-Hosted AI Lab Container w/Perplexica, Ollama, Open WebUI |
|  WIP | Portfolio Site + Authelia SSO + Coolify! |
|  WIP | Nextcloud deployment + Obsidian sync + Claude integration |
|  WIP | n8n Workflow Automation |
|  WIP | Github CI/CD + Portfolio Site integration |
|  WIP | VLAN topology restructure |
|  WIP | Network topology mapping w/ NetAlertX & draw.io |
|  WIP | OPNsense re-deploy (to play nice with all isolated networks and subnets) |
|  WIP | Wazuh deployment for variation on SIEM workflow(s)
| Ongoing | Security Audit + Stack Consolidation & Cleanup (constant) |
| Ongoing | Purple team attack/detect/improve cycles |

*Dates are approximate — full detailed logs coming in individual section READMEs.*

---

##  Key Things I've Learned

- **Bro, DNS really is everything.** When something breaks mysteriously, it's almost always DNS. The t-shirts are true. Building a redundant DNS cluster taught me more about how the protocol actually works than any course.
- **VirtioFS passthrough** requires specific kernel support — learned this the hard way after a kernel update broke mounts silently. Wanted to **fstab** something. 🔪 ...But I got it!
- **Tailscale ACLs** are deceptively powerful. Treating access by identity rather than IP flipped how I think about network segmentation. Really feels like it could be the enterprise standard soon. 
- **SIEM alerting is an art.** Too broad = alert fatigue. Too narrow = blind spots. Tuning Splunk against my own lab attacks has been the most valuable hands-on security learning I've done. Pairing it with Wireshark and port mapping/sniffing tools, plus applying things I've learned from online hacking labs has been super rewarding!
- **Caddy is underrated.** Compared to nginx or Apache for a homelab reverse proxy, the automatic TLS handling alone saves so much time and so many headaches.

---

## 🔗 Related

- GitHub Profile: [github.com/r1pp3r-d43m0n](https://github.com/r1pp3r-d43m0n)
- LinkedIn: [linkedin.com/in/thomasmsumer](https://linkedin.com/in/thomasmsumer)
- TryHackMe: Active — SEC0 Certified - Pursuing SEC1, SAL1, PT1
- HackTheBox: Active (Private, will go public with portfolio site) - Pursuing CJCA, CDSA

---

<sub>This repo is actively maintained. Configs, writeups, and lessons learned are added as I build. Nothing here is aspirational — if it's documented, it's been **built** and **tested**. Anytime there's a disaster you can expect a postmortum here where I pick through the rubble.</sub>
