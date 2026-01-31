# Threat model (lightweight)

## Threat: direct access to internal servers from LAN/VPN
Controls:
- internal server firewall allow-lists SSH only from jump01 internal IP
- internal servers exist only on internal subnet

## Threat: brute-force attempts against bastion SSH
Controls:
- VPN gating (workstation cannot SSH without VPN)
- bastion firewall allow-lists sources (default deny)

## Threat: lateral movement from internal servers back to bastion/LAN
Controls:
- bastion firewall does not permit SSH from internal subnet
- preserve directionality (admin access flows from bastion to internal only)

## Threat: secrets exposure in public repo
Controls:
- repo excludes private keys and VPN configs
- .gitignore includes WireGuard and key patterns
- publish only sanitized snippets and validation procedures
