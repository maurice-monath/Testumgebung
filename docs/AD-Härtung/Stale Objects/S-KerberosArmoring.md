# Sicherstellen, dass Clients Kerberos-Armierung unterstützen, wenn die Domänen-Funktionsstufe mindestens Windows Server 2012 beträgt

**Rule ID:** S-KerberosArmoring

**Beschreibung:**
Ziel ist es sicherzustellen, dass Clients Kerberos-Armierung unterstützen, wenn die Domänen-Funktionsstufe mindestens Windows Server 2012 beträgt.

**Technische Erklärung:**
Kerberos-Armierung ist eine Optimierung des Kerberos-Protokolls. Sie umgeht die Vorschauthentifizierungsschritte und verhindert damit Vorschauthentifizierungsangriffe.
Es wird nur ab Windows Server 2012 DC und Windows 8 Arbeitsstationen unterstützt.
Wenn Kerberos-Armierung für andere Betriebssysteme angefordert wird (wie Windows 7 oder Linux), kann das Kerberos-Authentifizierungsprotokoll fehlschlagen.

**Empfohlene Lösung:**
Um Kerberos-Armierung für Clients zu aktivieren, bearbeiten Sie die Gruppenrichtlinie und navigieren Sie zu `Computer Configuration` > `Policies` > `Administrative Templates` > `System` > `Kerberos`.
Aktivieren Sie dann die Richtlinie "Kerberos client support for claims, compound authentication and Kerberos armoring".

Diese Maßnahme gewährleistet, dass Clients die optimierte Sicherheitsfunktion Kerberos-Armierung nutzen können, wenn die Domänen-Funktionsstufe dies unterstützt.
