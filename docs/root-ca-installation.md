# Root-CA Installation Guide

## Installationsanleitung für die Root-Zertifizierungsstelle (Root-CA)

### Übersicht

Diese Anleitung bietet schrittweise Anweisungen zur Installation und Konfiguration einer Root-Zertifizierungsstelle (Root-CA) unter Verwendung von Windows Server.

### Voraussetzungen

- Windows Server installiert und konfiguriert.
- Administrativer Zugriff auf den Server.
- Sicheres Administrationspasswort.

### Installationsschritte

1. **Installieren von Active Directory Certificate Services (AD CS)**

   - Öffnen Sie den Server-Manager.
   - Klicken Sie auf Verwalten -> Rollen und Features hinzufügen.
   - Wählen Sie Active Directory-Zertifikatsdienste aus.
   - Folgen Sie dem Installationsassistenten und wählen Sie die Standardoptionen oder passen Sie sie an die Anforderungen Ihrer Organisation an.
   - Stellen Sie sicher, dass die Rolle Zertifizierungsstelle ausgewählt ist.

2. **Konfigurieren der Root-CA**

   - Nach der Installation öffnen Sie den Server-Manager.
   - Klicken Sie auf Werkzeuge -> Zertifizierungsstellenverwaltung, um die Verwaltungskonsole für die Zertifizierungsstelle zu öffnen.
   - Klicken Sie mit der rechten Maustaste auf CA (ComputerName) und wählen Sie Active Directory-Zertifikatsdienste konfigurieren.
   - Folgen Sie dem Assistenten, um die Root-Zertifizierungsstelle zu konfigurieren, z.B. als eigenständige CA.
   - Konfigurieren Sie Einstellungen wie kryptografische Optionen (z.B. SHA-512, 4096-Bit-Schlüssel), Gültigkeitsdauer und administrative Berechtigungen.
   - Schließen Sie den Assistenten ab, um die Konfiguration abzuschließen.

3. **Backup und Sicherheit**

   - Sichern Sie regelmäßig den privaten Schlüssel der Root-CA und die Konfigurationseinstellungen.
   - Implementieren Sie Sicherheitsmaßnahmen zum Schutz der Root-CA vor unbefugtem Zugriff.
   - Definieren und dokumentieren Sie Betriebsverfahren für Wartung und Notfallwiederherstellung.

4. **Testen und Bereitstellen**

   - Testen Sie die Funktionalität der Root-CA, indem Sie Testzertifikate ausstellen.
   - Integrieren Sie die Root-CA in die PKI-Infrastruktur Ihrer Organisation.
   - Dokumentieren Sie Bereitstellungsdetails und Konfiguration für zukünftige Referenzen.
