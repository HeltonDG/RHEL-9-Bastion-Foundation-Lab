# jump01 SSH client configuration (summary)

## Goal
Make bastion-to-internal administration predictable.

Example ~/.ssh/config entry on jump01:

Host srv01
    HostName 10.10.20.11
    User helton
    IdentityFile ~/.ssh/id_jump_to_internal
