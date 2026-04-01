# VLESS Reality Proxy

This repository contains documentation for setting up a private, secure proxy system. The project focuses on providing encrypted connectivity for personal devices and managing it through an isolated control panel.

## Project Goals
The system is built for the following legitimate purposes:

* Personal Data Protection: Encrypting internet traffic for personal devices to ensure security, especially when using public or untrusted networks (e.g., airports, cafes).
* Network Privacy: Protecting personal metadata and preventing unauthorized traffic analysis or monitoring by third parties.
* Educational Research: Practicing network administration and studying the implementation of modern protocols such as VLESS and XTLS-Reality.

## ⚠️ Important Notice

This documentation is provided for educational purposes and hands-on practice in network administration. All materials are intended for research into modern protocols and should be used responsibly according to your local regulations.

## Prerequisites

To successfully deploy this infrastructure, you will need the following:
* VPS Server: A Virtual Private Server from any provider (e.g., Fornex, Hetzner, DigitalOcean) running Ubuntu 24.04 LTS.
* Domain Name: A registered domain name (e.g., .com, .org, .net) to manage your subscription links.
* Cloudflare Account: A free Cloudflare account to hide your server's real IP and manage DNS records.
* Client Software: v2rayTun (or any Xray-core application) for connection and settings management.

## Beginner's Guide

If you are new to network administration, setting up a proxy might look complicated. To make it easier, you can use an AI assistant (like ChatGPT, Claude, or Gemini) as your personal tutor.

We have prepared a ready-to-use prompt containing all the necessary context. Just open the [ai-setup-prompt.txt](ai-setup-prompt.txt) file in this repository, copy its content, and paste it into any AI for a step-by-step installation guide.

---

## Architectural Specifications

### 1. Infrastructure & Access
| Feature | Value |
| :--- | :--- |
| OS | Ubuntu 24.04 LTS |
| Network | IPv4 (IPv6 disabled at kernel level) |
| Management | SSH Client (e.g., Termius) |
| SSH Port | Custom (5-digit, non-standard) |
| Auth Method | Cryptographic key only (Password auth disabled) |

### 2. Control Panel
| Feature | Value |
| :--- | :--- |
| Panel | 3x-ui (MHSanaei) |
| Access to Panel | Only via SSH tunnel (127.0.0.1) |
| Panel Port | Custom (5-digit, non-standard) |
| External Access to Panel | Blocked (inaccessible from internet) |

### 3. Network Security (Firewall)
| Feature | Value |
| :--- | :--- |
| Port 443 | ALLOW: Xray/VLESS |
| Port 2053 | ALLOW: Client subscriptions (Cloudflare IPs only) |
| SSH Port | ALLOW: System administration |
| Panel Port | CLOSED: No public web access |
| BitTorrent | BLOCKED: Restricted at panel level |

### 4. Protocol & Configuration

| Feature | Value |
| :--- | :--- |
| Core | Xray-core |
| Protocol | VLESS |
| Flow | xtls-rprx-vision |
| Xray Port | 443 |
| Security | XTLS-Reality |
| Fingerprint (uTLS) | Chrome |
| SNI / Target | example.com |
| Sniffing | Enabled (TLS, HTTP; QUIC: Disabled; Route Only: Enabled) |

#### SNI Selection Criteria
The target site (SNI) should meet the following technical requirements:
| Requirement | Description |
| :--- | :--- |
| TLS Version | Full support for TLS 1.3 |
| HTTP Version | Support for HTTP/2 |
| ASN Match | ASN of the site and your server must match |
| Accessibility | The site must be fully accessible in your region |
| SSL Provider | No Cloudflare certificate on the target |

### 5. Subscription System

| Feature | Value |
| :--- | :--- |
| Domain | example.com (Cloudflare integrated) |
| Cloudflare Mode | Proxy (Orange Cloud) — Server IP hidden |
| SSL/TLS Mode | Full (Strict) |
| Cloudflare Rule | Forwarding to port 2053 |
| External Proxy | Configured in 3x-ui (Server IP:443) |
| Reverse Proxy URI | Set in 3x-ui (Link generation via domain without IP/Port) |

#### Traffic Separation Scheme
| Plane | Path |
| :--- | :--- |
| Control Plane | Client → domain:2053 via Cloudflare (Subscription) |
| Data Plane | Client → Server IP:443 direct via VLESS + Reality (Main traffic) |

### 6. SSL/TLS Certificates
| Feature | Value |
| :--- | :--- |
| Domain Certificate | Let's Encrypt (managed automatically by Cloudflare) |
| Origin Certificate | Cloudflare Origin Certificate (15 years) |

### 7. DNS Configuration
| Feature | Value |
| :--- | :--- |
| DNS-over-HTTPS (DoH) | https://1.1.1.1/dns-query, https://8.8.8.8/dns-query |
| Request Strategy | UseIPv4 |
| Parallel Queries | Enabled |
| DNS Port | 443 (HTTPS) |
| Domain Routing | AsIs |

### 8. System Optimizations
| Feature | Value |
| :--- | :--- |
| BBR | Enabled (TCP speed optimization) |
| NTP Synchronization | UTC, active |
| Security Updates | Automatic (Unattended-upgrades) |

### 9. Backup System
| Feature | Value |
| :--- | :--- |
| Status | Daily automatic backup |
| Panel Database | x-ui.db |
| Xray Configuration | config.json |
| SSL Certificates | Origin Certificate & Private Key |

### 10. Client Configuration

| Feature | Value |
| :--- | :--- |
| Application | v2rayTun (App Store / Google Play) |
| Compatibility | Xray-core based applications |
| Subscription Update | Every 24 hours |
| Remote DNS | 1.1.1.1 (Cloudflare) |
| Bypass LAN | Enabled |
| Insecure Connections | Disabled |

---
*Technical Documentation | 2026*
