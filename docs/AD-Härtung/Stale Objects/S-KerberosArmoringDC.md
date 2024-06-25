# Sicherstellen, dass DC Kerberos-Armierung unterstützt, wenn die Funktionsstufe mindestens Windows Server 2012 beträgt

**Rule ID:** S-KerberosArmoringDC

**Beschreibung:**
Ziel ist es sicherzustellen, dass der DC Kerberos-Armierung unterstützt, wenn die Funktionsstufe mindestens Windows Server 2012 beträgt.

**Technische Erklärung:**
Kerberos-Armierung ist eine Optimierung des Kerberos-Protokolls. Sie umgeht die Vorschauthentifizierungsschritte und verhindert damit Vorschauthentifizierungsangriffe.
Es wird nur ab Windows Server 2012 DC und Windows 8 Arbeitsstationen unterstützt.
Wenn Kerberos-Armierung für andere Betriebssysteme angefordert wird (wie Windows 7 oder Linux), kann das Kerberos-Authentifizierungsprotokoll die Arbeit verweigern.

**Empfohlene Lösung:**
Um Kerberos-Armierung für Domänencontroller zu aktivieren, bearbeiten Sie die Gruppenrichtlinie und navigieren Sie zu `Computer Configuration` > `Policies` > `Administrative Templates` > `System` > `KDC`.
Aktivieren Sie dann die Richtlinie "KDC support for claims, compound authentication and Kerberos armoring".
Die Richtlinie sollte mindestens auf "Supported" gesetzt werden.

Die sicherste Einstellung ist "Fail authentication requests when Kerberos armoring is not available", sollte jedoch nur aktiviert werden, wenn die Clients Kerberos-Armierung unterstützen.

Diese Maßnahmen gewährleisten eine sichere Konfiguration und Nutzung des Kerberos-Protokolls innerhalb Ihrer Domäne.
