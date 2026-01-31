# srv01 SSH hardening (summary)

## Goal
Keys-only access, no root SSH.

## Recommended posture (concept)
- PasswordAuthentication no
- PermitRootLogin no
- PubkeyAuthentication yes

## Notes
Network policy still enforces bastion-only reachability.
SSH hardening is layered on top.
