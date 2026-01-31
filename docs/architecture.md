# Architecture

## Intent
Provide a stable, reusable lab foundation that mirrors common enterprise patterns:
- VPN-gated admin access
- bastion host for management entry
- segmented internal server network
- allow-list firewall rules with default deny
- internal servers reachable only from bastion

## Topology (anonymized)

Admin Workstation (Windows, WireGuard client)
  |
  | VPN (10.200.77.0/24) routes only bastion SSH
  v
Router (WireGuard server)
  |
  | LAN (172.16.50.0/24)
  v
jump01 (RHEL 9 bastion)
  - LAN NIC: 172.16.50.50
  - Internal NIC: 10.10.20.10
  |
  | Internal network (10.10.20.0/24)
  v
srv01 (RHEL 9 internal server)
  - Internal-only NIC: 10.10.20.11

## Trust boundaries
- Workstation -> jump01: allowed only via VPN + allow-list on jump01
- jump01 -> internal servers: allowed (management path)
- LAN/VPN -> internal servers: not allowed
- internal -> jump01 (SSH): not allowed (directionality preserved)

## Why it’s a foundation
This network/access pattern remains stable while you build new scenario labs on internal servers
(systemd, SELinux, storage, logging, patching, etc.).
