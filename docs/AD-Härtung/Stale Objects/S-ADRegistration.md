## Überprüfung des Prozesses der Registrierung von Computern in der Domäne

**Regel-ID:** S-ADRegistration

**Beschreibung:**
Der Zweck besteht darin sicherzustellen, dass grundlegende Benutzer keine zusätzlichen Computer in der Domäne registrieren können.

**Technische Erklärung:**
Standardmäßig kann ein grundlegender Benutzer bis zu 10 Computer innerhalb der Domäne registrieren. Diese Standardkonfiguration stellt ein Sicherheitsproblem dar, da grundlegende Benutzer keine solchen Konten erstellen sollten. Diese Aufgabe sollte von Administratoren übernommen werden.

Hinweis: Dieses Programm überprüft auch die GPO für die Zuweisung des Rechts "SeMachineAccountPrivilege". Diese Zuweisung kann verwendet werden, um die Auswirkungen des Schlüssels "ms-DS-MachineAccountQuota" einzuschränken.

**Empfohlene Lösung:**
Um das Problem zu lösen, begrenzen Sie die Anzahl der zusätzlichen Computer, die von einem grundlegenden Benutzer registriert werden können. Dies kann durch Ändern des Wertes von "ms-DS-MachineAccountQuota" auf null (0) reduziert werden. Eine weitere Lösung kann darin bestehen, die Gruppe "Authentifizierte Benutzer" in der Richtlinie der Domänencontroller vollständig zu entfernen. Beachten Sie, dass, wenn Sie eine Delegation an ein Konto einrichten müssen, damit es Computer in die Domäne hinzufügen kann, dies auf zwei Methoden erfolgen kann: Delegation in der OU oder durch Zuweisung des Rechts "SeMachineAccountPrivilege" an eine spezielle Gruppe.


## Anleitung zur Begrenzung der Registrierung von Computern in der Domäne

**Ziel:**
Begrenzen Sie die Anzahl der zusätzlichen Computer, die von einem grundlegenden Benutzer und Administratoren registriert werden können, auf null. Berechtigen Sie eine spezielle Gruppe, Computer in die Domäne aufnehmen zu können.

### Schritte zur Begrenzung der Registrierung von Computern auf Null

```powershell
# PowerShell-Skript zum Ändern des ms-DS-MachineAccountQuota-Werts auf 0
Set-ADDomain -Identity "test.azubi.gkd-re.de" -Replace @{ "ms-DS-MachineAccountQuota" = 0 }
```

### Schritte zur Zuweisung von "SeMachineAccountPrivilege" an eine spezielle Gruppe

```powershell
# PowerShell-Skript zum Erstellen einer Gruppe und Zuweisen von "SeMachineAccountPrivilege" per GPO
Import-Module ActiveDirectory
Import-Module GroupPolicy

# Variablen definieren
$domainName = "test.azubi.gkd-re.de"
$groupName = "Computer Registrars"
$ouPath = "OU=Groups,DC=test,DC=azubi,DC=gkd-re,DC=de"
$gpoName = "Delegation of Computer Join"
$domain = "test.azubi.gkd-re.de"

# Erstellen Sie die Gruppe in der angegebenen OU
New-ADGroup -Name "Computer Registrars" -SamAccountName "Computer Registrars" -GroupCategory Security -GroupScope Global -Path "CN=Users,DC=test,DC=azubi,DC=gkd-re,DC=de" -Description "Group for delegating computer join rights"

# Erstellen Sie eine neue GPO
New-GPO -Name $gpoName -Domain $domain

# Verknüpfen Sie die GPO mit der Domäne
New-GPLink -Name $gpoName -Target "DC=$($domain -replace '\.', ',DC=')"

# Fügen Sie die Gruppe der Richtlinie "Add workstations to domain" hinzu
Set-GPRegistryValue -Name $gpoName -Key "HKLM\SOFTWARE\Policies\Microsoft\Windows\CurrentVersion\Internet Settings\Zones\1" -ValueName "SeMachineAccountPrivilege" -Type MultiString -Value "$domain\$groupName"

Write-Host "Die Gruppe wurde erstellt und das Recht 'SeMachineAccountPrivilege' wurde der Gruppe erfolgreich per GPO zugewiesen."

```

5. **Anwenden der GPO:**
   - Schließen Sie den Gruppenrichtlinienverwaltungs-Editor.
   - Stellen Sie sicher, dass die GPO mit den gewünschten OUs oder der Domäne verknüpft ist.

6. **Erzwingen der Gruppenrichtlinienaktualisierung:**
   - Sie können die Gruppenrichtlinien auf den Client-Computern aktualisieren, indem Sie `gpupdate /force` auf den Computern ausführen.
   - Alternativ warten Sie, bis die Gruppenrichtlinienaktualisierung automatisch erfolgt (Standardintervall beträgt 90 Minuten).

### Zusammenfassung

Durch diese Schritte stellen Sie sicher, dass:
- Grundlegende Benutzer keine zusätzlichen Computer in die Domäne registrieren können.
- Administratoren ebenfalls keine zusätzlichen Computer registrieren können.
- Nur Mitglieder der speziellen Gruppe `Computer Registrars` die Berechtigung haben, Computer in die Domäne aufzunehmen.

Diese Konfiguration verbessert die Sicherheit Ihrer Domäne, indem sie die Kontrolle über die Computerregistrierung zentralisiert und Missbrauchsmöglichkeiten einschränkt.
