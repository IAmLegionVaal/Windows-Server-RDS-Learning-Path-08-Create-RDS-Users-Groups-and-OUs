# Windows Server RDS Learning Path 08 — Create RDS Users Groups and OUs

**Level:** Beginner · **Module:** 8/70

## Goal
Create the OU, user and group structure used to manage RDS servers and access.

## Setup
1. Create `OU=RDS` with child OUs `Servers`, `Users`, `Groups` and `Service Accounts`.
2. Protect the OUs from accidental deletion.
3. Create `GG-RDS-Users` and `GG-RDS-Admins-Lab`.
4. Create two normal test users and add them to `GG-RDS-Users`.
5. Document group owner, purpose and review date.

```powershell
$Base='OU=RDS,DC=corp,DC=lab'
New-ADOrganizationalUnit -Name RDS -Path 'DC=corp,DC=lab'
'Users','Servers','Groups','Service Accounts' | ForEach-Object {
  New-ADOrganizationalUnit -Name $_ -Path $Base
}
New-ADGroup -Name GG-RDS-Users -GroupScope Global -GroupCategory Security -Path "OU=Groups,$Base"
Get-ADOrganizationalUnit -SearchBase $Base -Filter *
Get-ADGroup GG-RDS-Users -Properties Members,Description,ManagedBy
```

## Evidence
Store the OU diagram, PowerShell inventory, group properties, user membership and accidental-deletion protection under `evidence/`. Record why each OU and group exists.

## Troubleshooting
Use exact distinguished names when objects are created in the wrong parent. Refresh logon tokens after group changes.

## Security
OUs are policy and delegation scopes, not security boundaries. Keep privileged identities separate from normal RDS users.

## Rollback
Remove only disposable users and groups after checking dependencies; delete OUs only when empty and deliberately unprotected.

## Next
`Windows-Server-RDS-Learning-Path-09-Install-a-Windows-Client`
