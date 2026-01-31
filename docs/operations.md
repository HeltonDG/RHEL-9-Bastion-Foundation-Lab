# Operations

## Access workflow
1) VPN ON (WireGuard on workstation)
2) SSH to bastion (jump01)
3) SSH from bastion to internal servers

## One-command workflow (nested SSH)
From workstation:
```bash
ssh helton@172.16.50.50 -t "ssh srv01"

