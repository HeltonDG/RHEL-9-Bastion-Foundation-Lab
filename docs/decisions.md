# Design decisions

## 1) Allow-list + default deny (no SSH reject rules)
SSH enforcement uses allow-list rules only.
Reject rules were avoided to prevent brittle ordering behavior during iteration.
Default deny handles “block everyone else” cleanly.

## 2) Internal servers are internal-only
Internal servers live only on the internal subnet/VM network.
No LAN NIC and no “convenience” NAT NIC.
This prevents bypass routes and reduces attack surface.

## 3) Bastion is a bastion, not a router
The bastion is the management entry point, not a general transit router.
The internal network remains an enclave; reachability is intentionally limited.

## 4) Connectivity enforcement before hardening
First enforce reachability constraints (VPN + firewall allow-lists).
Then harden SSH on internal servers (keys-only, no root login).
This keeps changes small and testable.
