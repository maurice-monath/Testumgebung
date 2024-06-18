# Projektplan: Einrichtung einer sicheren IT-Umgebung mit Proxmox, AD DS, GitLab und Härtung

## Voraussetzungen
- Proxmox-Cluster ist eingerichtet.
- VLANs sind konfiguriert (VLAN 10: Management, VLAN 20: Servers, VLAN 30: Clients).

## Schritt 1: Temporären GitLab-Server einrichten

1. **VM im VLAN 10 (Management) erstellen**
    - Ubuntu LTS installieren
    - GitLab installieren und mit selbstsigniertem SSL-Zertifikat konfigurieren
    - Firewall-Regeln setzen, um internen Zugriff zu beschränken

## Schritt 2: Active Directory Domain Services (AD DS) einrichten

### AD-DS Installation

1. **VMs für DCs und BAM im VLAN 5 (Servers) erstellen**
    - Windows Server 2025 Core installieren
    - AD DS installieren und Domäne konfigurieren
    - Sicherheitseinstellungen und Gruppenrichtlinien gemäß Best Practices anwenden
    - Managment nur über die PAW
    - Nur Ports für ADDS für die anderen VLANS freischalten

### Privileged Access Workstation (PAW)

1. **VM im VLAN 5 (ADDS) erstellen**
    - Windows Server 2025 Desktop installieren
    - PAW für Verwaltung der DCs konfigurieren
    - Firewall-Regeln setzen, um Zugriff auf DCs nur von PAW zu erlauben

## Schritt 3: Produktiven GitLab-Server einrichten

1. **VM im VLAN 20 (Servers) erstellen**
    - Ubuntu LTS installieren
    - GitLab installieren und mit selbstsigniertem SSL-Zertifikat konfigurieren
    - LDAPS einrichten und GitLab für AD-Authentifizierung konfigurieren
    - Firewall-Regeln setzen, um internen Zugriff zu beschränken

## Schritt 4: Weitere Server einrichten.

### Windows Server 2025 

1. **VMs im VLAN 20 (Servers) erstellen**
    - Windows Server 2025 installieren
    - Anwendungen installieren und konfigurieren
    - Sicherheitsmaßnahmen gemäß Best Practices implementieren


### Linux Server (Ubuntu 24.04 LTS)

1. **VMs im VLAN 20 (Servers) erstellen**
    - Ubuntu 24.04 LTS installieren
    - Anwendungen installieren und konfigurieren
    - Sicherheitsmaßnahmen gemäß Best Practices implementieren

## Schritt 5: Dokumentation und Skripte in GitLab

1. **Repository für Dokumentation erstellen**
    - Projekt in GitLab anlegen und Dokumentation hochladen
    - Skripte zur Automatisierung der Installation und Konfiguration in GitLab speichern
    - Dokumentation fortlaufend pflegen und aktualisieren