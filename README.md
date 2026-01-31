# RHEL-9-Bastion-Foundation-Lab
RHEL 9 bastion host foundation with VPN-gated SSH access, internal network isolation, and allow-list firewall policy. Designed as a reusable platform for Linux administration and security labs.

# RHEL 9 Bastion Foundation Lab (Router WireGuard + Internal Isolation)

This repository documents a small, realistic Linux lab foundation designed to be reused for future projects.
It enforces secure administrative access using:
- a home router running a WireGuard **server**
- a RHEL 9 bastion host (“jump host”) as the only SSH entry point
- an internal-only server network reachable **only from the bastion**
- allow-list firewall policy (default deny)
- SSH hardening on internal servers (keys-only)

This is intentionally not overengineered. It uses standard RHEL tools (NetworkManager, firewalld, OpenSSH)
and an operator workflow that mirrors common enterprise patterns.

> Note: All IP addresses in this repo are anonymized and do not reflect any real environment.

---

## Goals
- Build a reusable “foundation architecture” for future Linux labs
- Practice RHCSA-aligned administration skills in a realistic topology
- Demonstrate enterprise security thinking: segmentation, least privilege, defense in depth
- Keep changes small, controlled, and testable

---

## Anonymized environment

### Networks
- VPN subnet: `10.200.77.0/24`
- LAN subnet: `172.16.50.0/24`
- Internal server network: `10.10.20.0/24`

### Systems
- Admin workstation: Windows 11 laptop (WireGuard client)
- Router: runs WireGuard server
- Hypervisor host: Windows 11 running VMware Workstation
- **jump01 (RHEL 9 bastion)**
  - LAN NIC: `172.16.50.50`
  - Internal NIC: `10.10.20.10`
- **srv01 (RHEL 9 internal server)**
  - Internal NIC only: `10.10.20.11`
- Emergency admin access from VMware host on LAN: `172.16.50.208/32`

---

## What enforces security

### VPN gating (router)
- The Windows WireGuard client routes only bastion access through the VPN.
- Example concept: `AllowedIPs = 10.200.77.0/24, 172.16.50.50/32`
- Result:
  - VPN ON: workstation can SSH to bastion
  - VPN OFF: workstation cannot SSH to bastion

### Bastion firewall (jump01)
Allow-list only (default deny blocks everything else):
- allow tcp/22 from VPN subnet `10.200.77.0/24`
- allow tcp/22 from emergency admin host `172.16.50.208/32`

### Internal server firewall (srv01)
Allow-list only:
- allow tcp/22 from jump01 internal IP `10.10.20.10/32`

No LAN access, no VPN-client access to internal servers.

### SSH posture
- Bastion can be password-auth (VPN + firewall gated)
- Internal servers are hardened to keys-only (no password auth, no root login)

---

## Operator workflow (simple and realistic)

### Normal flow
1) Workstation connects to bastion (VPN ON)
2) Bastion connects to internal servers

### One-command nested SSH from workstation (recommended)
This keeps internal private keys on the bastion while still giving a single command workflow:

```bash
ssh helton@172.16.50.50 -t "ssh srv01"
