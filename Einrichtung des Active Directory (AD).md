# Installation eines neuen AD-Forests und Hinzufügen weiterer Domain-Controller

## Schritt 1: Netzwerkkonfiguration

Ändern Sie die IP-Adresse des Computers:

```powershell
New-NetIPAddress -InterfaceAlias "Ethernet" -IPAddress "192.168.30.x" -PrefixLength 24 -DefaultGateway "192.168.30.1"
```
Computer Namen bearbeiten:
```powershell
Rename-Computer -NewName "NeuerName" -Restart
```

Zeitzone auf Deutschland setzen
Set-TimeZone -Id "W. Europe Standard Time"

## Schritt 1: AD-Domain-Services auf `TE-DC01` installieren

Führen Sie den folgenden PowerShell-Befehl aus, um die Active Directory Domain Services auf `TE-DC01` zu installieren:

```powershell
Install-WindowsFeature -Name AD-Domain-Services -IncludeManagementTools
```

# Installation eines neuen AD-Forests und Hinzufügen weiterer Domain-Controller

## Schritt 1: AD-Domain-Services auf `TE-DC01` installieren

Führen Sie den folgenden PowerShell-Befehl aus, um die Active Directory Domain Services auf `TE-DC01` zu installieren:

```powershell
Install-WindowsFeature -Name AD-Domain-Services -IncludeManagementTools
```
## Schritt 2: Neuen AD-Forest auf TE-DC01 mit Site “Testumgebung” erstellen

Führen Sie den folgenden PowerShell-Befehl aus, um einen neuen AD-Forest mit der Domain test.azubi.gkd-re.de und der Site “Testumgebung” zu erstellen:

```powershell
Install-ADDSForest -DomainName "test.azubi.gkd-re.de" -InstallDNS -Force -SiteName "Testumgebung"
```
Anmerkung:

	•	Dieser Befehl installiert auch den DNS-Server und konfiguriert ihn für die neue Domain.
	•	Der Parameter -Force bestätigt alle Eingabeaufforderungen automatisch.

## Schritt 3: AD-Domain-Services auf TE-DC02 und dc03 installieren

Führen Sie auf TE-DC02 und dc03 den folgenden PowerShell-Befehl aus, um die Active Directory Domain Services zu installieren:

```powershell
Install-WindowsFeature -Name AD-Domain-Services -IncludeManagementTools
```

## Schritt 4: TE-DC02 und dc03 als zusätzliche Domain-Controller hinzufügen

Führen Sie den folgenden PowerShell-Befehl auf TE-DC02 und dc03 aus, um sie als zusätzliche Domain-Controller für die Domain test.azubi.gkd-re.de und der Site “Testumgebung” hinzuzufügen:

Auf TE-DC02:
```powershell
Install-ADDSDomainController -InstallDNS -DomainName "test.azubi.gkd-re.de" -Credential (Get-Credential) -SiteName "Testumgebung" -ReplicationSourceDC "dc01.test.azubi.gkd-re.de" -Force
```

Auf TE-DC03:
```powershell
Install-ADDSDomainController -InstallDNS -DomainName "test.azubi.gkd-re.de" -Credential (Get-Credential) -SiteName "Testumgebung" -ReplicationSourceDC "dc01.test.azubi.gkd-re.de" -Force
```
Anmerkung:

	•	Der Parameter -Credential (Get-Credential) fordert die Eingabe der Anmeldeinformationen eines Benutzers mit den erforderlichen Berechtigungen an.
	•	Der Parameter -SiteName gibt den Standortnamen an, in diesem Fall “Testumgebung”.
	•	Der Parameter -ReplicationSourceDC gibt den Quell-Domain-Controller für die Replikation an.


## Schritt 5: Überprüfen der Installation

Nach der Installation können Sie die Installation überprüfen, indem Sie die folgenden Befehle ausführen:

Überprüfen Sie die AD-Domäne und Domain-Controller auf TE-DC01:
```powershell
Get-ADDomain
Get-ADDomainController -Filter *
```
Überprüfen Sie die Replikation zwischen den Domain-Controllern:
```powershell
Repadmin /replsummary
Repadmin /showrepl
```
