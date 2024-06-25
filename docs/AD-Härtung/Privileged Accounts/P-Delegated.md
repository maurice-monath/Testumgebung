# Mindestens ein Administratorkonto kann delegiert werden

**Rule ID:** P-Delegated

**Beschreibung:**
Ziel ist es sicherzustellen, dass alle Administratorkonten die Konfigurationsoption "Dieses Konto ist sensibel und kann nicht delegiert werden" aktiviert haben (oder Mitglieder der integrierten Gruppe "Geschützte Benutzer" sind, wenn Ihre Domänen-Funktionsstufe mindestens Windows Server 2012 R2 beträgt).

**Technische Erklärung:**
Ohne die Option "Dieses Konto ist sensibel und kann nicht delegiert werden" kann jedes Konto von einem Dienstekonto impersoniert werden. Es ist bewährte Praxis, diese Option für Administratorkonten zu erzwingen.

**Empfohlene Lösung:**
Um die Situation zu korrigieren, stellen Sie sicher, dass alle Ihre Administratorkonten das Kontrollkästchen "Dieses Konto ist sensibel und kann nicht delegiert werden" aktiviert haben oder fügen Sie Ihre Administratorkonten der integrierten Gruppe "Protected User" hinzu, wenn Ihre Domänen-Funktionsstufe mindestens Windows Server 2012 R2 beträgt (einige Funktionen funktionieren möglicherweise nicht ordnungsgemäß danach, überprüfen Sie bitte die offizielle Dokumentation).
Wenn Sie das Kontrollkästchen "This account is sensitive and cannot be delegated" aktivieren möchten, dies jedoch nicht möglich ist, weil das Kästchen nicht vorhanden ist (typischerweise bei GMSA-Konten), können Sie die Flagge manuell hinzufügen, indem Sie die Zahl 1048576 zum Attribut "useraccountcontrol" des Kontos hinzufügen.
Bitte beachten Sie, dass im Bericht weiter unten ein Abschnitt mit dem Namen "Admin-Gruppen" weitere Informationen liefert.



```powershell
# Überprüfen und korrigieren Sie die Einstellung für "Dieses Konto ist sensibel und kann nicht delegiert werden" für Administratorkonten

# Einstellungen
$domainController = "te-dc01.test.azubi.gkd-re.de"
$adminGroup = "Administrators"  # Anpassen je nach Ihrem Administratoren-Gruppennamen

# Suchen nach allen Administratorkonten
$admins = Get-ADGroupMember -Identity $adminGroup | Where-Object {$_.objectClass -eq "user"}

foreach ($admin in $admins) {
    $adminAccount = Get-ADUser -Identity $admin.SamAccountName -Server $domainController -Properties useraccountcontrol

    # Überprüfen, ob die Option "Dieses Konto ist sensibel und kann nicht delegiert werden" gesetzt ist
    if (-not ($adminAccount.useraccountcontrol -band 0x1000000)) {
        Write-Output "Konto $($adminAccount.SamAccountName) hat die Option nicht gesetzt."

        # Aktivieren der Option "Dieses Konto ist sensibel und kann nicht delegiert werden"
        Set-ADUser -Identity $adminAccount -Server $domainController -Add @{useraccountcontrol=0x1000000}
        Write-Output "Option erfolgreich aktiviert für $($adminAccount.SamAccountName)"
    } else {
        Write-Output "Konto $($adminAccount.SamAccountName) hat die Option bereits gesetzt."
    }
}

# Hinzufügen von Administratorkonten zur Gruppe "Geschützte Benutzer", falls erforderlich
# Anpassen Sie diese Logik entsprechend Ihren spezifischen Anforderungen

# Beispiel:
# Add-ADGroupMember -Identity "Geschützte Benutzer" -Members "Administrator1", "Administrator2"



```