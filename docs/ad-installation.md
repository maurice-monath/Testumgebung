# AD Installation

```powerhsell
Install-WindowsFeature -name AD-Domain-Services -IncludeManagementTools
```
```powerhsell
Install-ADDSForest -DomainName "test.azubi.gkd-re.de" -InstallDNS -Force
```
```powerhsell
Install-ADDSDomainController -InstallDns -DomainName "test.azubi.gkd-re.de" -Credential (Get-Credential) -Force
```
