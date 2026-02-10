                                Internet
                                   |
                             [ISP Modem/ONT]
                                   |
                                   |
                     +--------------------------------+
                     |   Router / Gateway              |
                     |   - WireGuard Server            |
                     |   - LAN Gateway                 |
                     |   LAN: 172.16.50.1/24           |
                     |   WG:  10.200.77.1/24           |
                     +---------------+----------------+
                                     |
                                     | LAN 172.16.50.0/24
        --------------------------------------------------------------------
        |                     |                      |                    |
        |                     |                      |                    |
 [LAN Devices]         [Admin Workstation]    [Admin Workstation]     [Hypervisor Host]
 phones/tv/etc.        (local)                (remote)                (emergency path)
                       172.16.50.x            Internet -> WG VPN      172.16.50.208
                                              -> 10.200.77.x

Allowed administrative path (final state):
Remote Admin (10.200.77.x) -> Router -> LAN -> SSH -> jump01 -> SSH -> internal servers

Policy intent:
- LAN devices should NOT be allowed to SSH to jump01
- SSH to jump01 allowed ONLY from:
  - WireGuard subnet (10.200.77.0/24)
  - Hypervisor host IP (172.16.50.208/32) [emergency]

                                     |
                                     | SSH (22) restricted sources
                                     v
                     +-----------------------------------+
                     |   jump01 (RHEL 9 Bastion)          |
                     |   NIC1: LAN (bridged)              |
                     |     - 172.16.50.50 (reserved)      |
                     |   NIC2: Internal (host-only)       |
                     |     - 10.10.20.10/24               |
                     +------------------+----------------+
                                        |
                                        | Internal Server Network (isolated)
                                        | 10.10.20.0/24 (host-only VMnet)
                                        v
                 +--------------------------------------------------+
                 | Internal Servers (no LAN/bridged NICs)            |
                 | - srv01: 10.10.20.11                               |
                 | - log01: 10.10.20.20                               |
                 | SSH allowed ONLY from jump01 (10.10.20.10)         |
                 | Centralized logging: TCP/514 -> log01              |
                 +--------------------------------------------------+
