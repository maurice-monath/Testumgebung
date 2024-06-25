# Intermediate CA Installation Guide

## Installationsanleitung für die Zwischen-Zertifizierungsstelle (Intermediate CA)

### Übersicht

Diese Anleitung bietet schrittweise Anweisungen zur Installation und Konfiguration einer Zwischen-Zertifizierungsstelle (Intermediate CA) unter Verwendung von Windows Server.

### Voraussetzungen

- Root-CA bereits installiert und betriebsbereit.
- Windows Server installiert und für Intermediate CA konfiguriert.
- Administrativer Zugriff auf den Server.
- Sicheres Administrationspasswort.

### Installationsschritte

1. **Zertifikat anfordern und ausstellen**

   - Generieren Sie eine Zertifikatsanforderung (CSR) auf dem Intermediate CA-Server.
   - Reichen Sie die CSR bei der Root-CA zur Unterzeichnung ein.
   - Holen Sie das signierte Zertifikat (ausgestellt von Root-CA) ab.

2. **Installieren von Active Directory Certificate Services (AD CS)**

   - Öffnen Sie den Server-Manager.
   - Klicken Sie auf Verwalten -> Rollen und Features hinzufügen.
   - Wählen Sie Active Directory-Zertifikatsdienste aus.
   - Folgen Sie dem Installationsassistenten und wählen Sie die Standardoptionen oder passen Sie sie an die Anforderungen Ihrer Organisation an.
   - Stellen Sie sicher, dass die Rolle Zertifizierungsstelle ausgewählt ist.

3. **Konfigurieren der Intermediate CA**

   - Nach der Installation öffnen Sie den Server-Manager.
   - Klicken Sie auf Werkzeuge -> Zertifizierungsstellenverwaltung, um die Verwaltungskonsole für die Zertifizierungsstelle zu öffnen.
   - Klicken Sie mit der rechten Maustaste auf CA (ComputerName) und wählen Sie Alle Aufgaben -> Neue Anforderung einreichen.
   - Wählen Sie die signierte Zertifikatsdatei von der Root-CA aus.
   - Folgen Sie dem Assistenten, um das Zertifikat auf der Intermediate CA zu installieren.

4. **Backup und Sicherheit**

   - Sichern Sie regelmäßig den privaten Schlüssel der Intermediate CA und die Konfigurationseinstellungen.
   - Implementieren Sie Sicherheitsmaßnahmen zum Schutz der Intermediate CA vor unbefugtem Zugriff.
   - Definieren und dokumentieren Sie Betriebsverfahren für Wartung und Notfallwiederherstellung.

5. **Testen und Bereitstellen**

   - Testen Sie die Funktionalität der Intermediate CA, indem Sie Testzertifikate ausstellen.
   - Integrieren Sie die Intermediate CA in die PKI-Infrastruktur Ihrer Organisation.
   - Dokumentieren Sie Bereitstellungsdetails und Konfiguration für zukünftige Referenzen.
