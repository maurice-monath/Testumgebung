Install-WindowsFeature -name AD-Domain-Services -IncludeManagementTools


Install-ADDSForest -DomainName "test.azubi.gkd-re.de" -InstallDNS -Force

Install-ADDSDomainController -InstallDns -DomainName "test.azubi.gkd-re.de" -Credential (Get-Credential) -Force
