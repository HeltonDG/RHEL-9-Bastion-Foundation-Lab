# srv01 firewalld policy (summary)

## Goal
Allow SSH to srv01 only from the bastion’s internal IP:
- 10.10.20.10/32

## Approach
- Add allow-list rule for tcp/22 from 10.10.20.10/32
- Default deny blocks all other sources

Result:
- No LAN access
- No VPN-client access
- Bastion-only management
