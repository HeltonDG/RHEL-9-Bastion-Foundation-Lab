# jump01 firewalld policy (summary)

## Goal
Allow SSH to jump01 only from:
- VPN subnet: 10.200.77.0/24
- Emergency admin host: 172.16.50.208/32

## Approach
- Remove `ssh` service from zone services (avoid broad allow)
- Add explicit allow-list rules for tcp/22 from trusted sources
- Default deny blocks all other sources

## Notes
- No SSH reject rules used (keeps rule behavior predictable)
- This is intentional and stable for the foundation lab
