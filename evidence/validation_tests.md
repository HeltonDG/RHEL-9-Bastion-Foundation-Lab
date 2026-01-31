# Validation tests (expected results)

## Bastion access
- Workstation -> jump01 (VPN ON): SUCCESS
  - ssh helton@172.16.50.50
- Workstation -> jump01 (VPN OFF): FAIL
  - ssh helton@172.16.50.50

## Emergency access
- VMware host (172.16.50.208) -> jump01: SUCCESS
  - ssh helton@172.16.50.50

## Internal server isolation
- Workstation/LAN -> srv01: FAIL
  - ssh helton@10.10.20.11
- jump01 -> srv01: SUCCESS
  - ssh helton@10.10.20.11   (or `ssh srv01` if configured)

## Directionality (optional policy goal)
- srv01 -> jump01 (SSH): FAIL
  - ssh helton@10.10.20.10

## Operator workflow
- One-command nested SSH from workstation: SUCCESS
  - ssh helton@172.16.50.50 -t "ssh srv01"
