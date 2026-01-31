# Windows operator workflow (PowerShell)

## One-command access to internal server via bastion
From workstation:

ssh helton@172.16.50.50 -t "ssh srv01"

## Optional PowerShell helper (session function)
Define a shortcut in PowerShell:

function srv01 { ssh helton@172.16.50.50 -t "ssh srv01" }

Remove it from the current session:

Remove-Item function:srv01

Persist it by placing it in your PowerShell profile ($PROFILE) if desired.
