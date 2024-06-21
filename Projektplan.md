# Projektplan für die Testumgebung

## 1. Projektvorbereitung
   - **Projektziele definieren**: Sicherstellen der Funktionalität und Sicherheit der Testumgebung.
   - **Ressourcen planen**: Hardware, Software, und personelle Ressourcen.
   - **Zeitrahmen festlegen**: Start- und Enddatum des Projekts sowie Meilensteine.

## 2. Netzwerkdesign
   - **VLAN-Konfiguration**
     - VLAN 30: Für PAW (Privileged Access Workstation) - IP-Bereich: `192.168.30.x/24`
     - VLAN 40: Für Server, die auf das AD zugreifen sollen - IP-Bereich: `192.168.40.x/24`
   - **Subnetz und IP-Adressen planen**

## 3. Installation und Konfiguration des Windows Server 2025 Core
   - **Bereitstellung des Servers**: Installation von Windows Server 2025 Core.
   - **Konfiguration der Netzwerkschnittstellen**: IP-Adressen und DNS-Server einrichten.

## 4. Einrichtung des Active Directory (AD)
   - **AD-Rolle hinzufügen**
   - **ADDS-Konfiguration**

## 5. Härtung des Active Directory
   - **Sicherheitsrichtlinien implementieren**:
     - Passwortrichtlinien
     - Konto-Sperrrichtlinien
     - Audit-Richtlinien
   - **Delegation minimieren**
   - **Verwaltungskonten schützen**
   - **Protokollierung aktivieren und überwachen**

## 6. Einrichtung der Zertifizierungsstelle (CA)
   - **CA-Rolle hinzufügen**
   - **Konfiguration der CA**:
     - **Standalone Root CA**: Erstellen und Konfigurieren der Root-CA.
     - **Ausstellen von Zertifikaten**: Konfigurieren von Zertifikatsvorlagen.

## 7. Einrichtung der PAW im VLAN 30
   - **Installation und Konfiguration der PAW**: Einrichten der PAW mit restriktiven Richtlinien.
   - **Sicherheitsrichtlinien implementieren**:
     - Einschränkung der Netzwerkzugriffe.
     - Einschränkung der Softwareinstallationen.

## 8. Konfiguration von LDAPS
   - **Zertifikat für AD DS**: Zertifikat für den LDAP-Dienst installieren und konfigurieren.
   - **LDAPS aktivieren**: Sicherstellen, dass der LDAP-Dienst über SSL/TLS verfügbar ist.

## 9. Server im VLAN 40 konfigurieren
   - **Windows Server**: Hinzufügen zur Domäne und Konfiguration von Sicherheitsrichtlinien.
   - **Linux Server**: Konfiguration der LDAPS-Anbindung:
     - Installation notwendiger Pakete.
     - Konfiguration von `/etc/sssd/sssd.conf` für LDAPS.
     - Beitritt zur Domäne.

## 10. Einrichtung des GitLab Servers
   - **Installation und Konfiguration des GitLab Servers**: GitLab auf einem internen Server installieren.
   - **SSL-Zertifikat**: Installation eines SSL-Zertifikats von der internen CA.
   - **Interne Verfügbarkeit**: Sicherstellen, dass der GitLab Server nur über interne IP und DNS erreichbar ist.
   - **AD-Integration**: Konfigurieren des GitLab Servers für die Authentifizierung mit AD-Benutzerdaten.

## 11. Sicherheitstests und Validierung
   - **Sicherheitsaudits durchführen**: Regelmäßige Überprüfung der Sicherheitskonfigurationen.
   - **Penetrationstests**: Durchführung von internen und externen Tests auf Schwachstellen.

## 12. Dokumentation und Schulung
   - **Dokumentation der Konfiguration**: Erstellung detaillierter Dokumentationen für alle Konfigurationsschritte.
   - **Schulung der Administratoren**: Schulung zu den neuen Systemen und Sicherheitsrichtlinien.

## 13. Projektabschluss und Übergabe
   - **Übergabe an den Betrieb**: Übergabe der dokumentierten Testumgebung an das Betriebsteam.
   - **Abschlussbericht**: Zusammenfassung des Projekts und der erzielten Ergebnisse.

## Zeitplan (Beispiel)

| Phase                | Aufgabe                                   | Dauer     |
|----------------------|-------------------------------------------|-----------|
| 1. Projektvorbereitung | Ziele definieren, Ressourcen planen       | 1 Woche   |
| 2. Netzwerkdesign      | VLAN-Konfiguration, Subnetzplanung         | 1 Woche   |
| 3. Installation und Konfiguration | Windows Server installieren    | 2 Wochen  |
| 4. Einrichtung AD      | AD-Rolle hinzufügen, konfigurieren        | 1 Woche   |
| 5. Härtung AD          | Sicherheitsrichtlinien implementieren     | 2 Wochen  |
| 6. Einrichtung CA      | CA-Rolle hinzufügen, konfigurieren        | 1 Woche   |
| 7. Einrichtung PAW     | PAW konfigurieren                         | 1 Woche   |
| 8. Konfiguration LDAPS | Zertifikate, LDAPS aktivieren             | 1 Woche   |
| 9. Konfiguration Server | Windows/Linux Server konfigurieren       | 2 Wochen  |
| 10. Einrichtung GitLab  | GitLab Server installieren und konfigurieren | 2 Wochen  |
| 11. Sicherheitstests    | Audits, Penetrationstests                | 2 Wochen  |
| 12. Dokumentation und Schulung | Dokumentation erstellen, Schulung | 1 Woche   |
| 13. Projektabschluss    | Übergabe, Abschlussbericht               | 1 Woche   |

## Fazit

Dieser Projektplan bietet eine umfassende Anleitung zur Einrichtung einer sicheren Testumgebung mit Windows AD auf einem Server 2025 Core, PAW im VLAN 30, Servern im VLAN 40, und einem internen GitLab Server, der über IP und internen DNS-Namen mit SSL von der Zertifizierungsstelle und AD-Benutzerdaten verfügbar ist. Die strukturierte Vorgehensweise stellt sicher, dass alle Aspekte der Sicherheit und Funktionalität abgedeckt werden.