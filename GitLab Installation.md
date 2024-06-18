# GitLab Installation mit selbstsigniertem SSL-Zertifikat

Diese Anleitung beschreibt die Installation von GitLab CE auf einer Ubuntu-VM im VLAN 10 (Management) mit einem selbstsignierten SSL-Zertifikat.

## Voraussetzungen

- Ubuntu 20.04/22.04 LTS installiert
- Netzwerkzugriff im VLAN 10
- Root- oder Sudo-Zugriff auf das System

## Schritt 1: Systemvorbereitung

1. Aktualisieren Sie das System:
    ```bash
    sudo apt update && sudo apt upgrade -y
    sudo reboot
    ```

## Schritt 2: GitLab-Repository hinzufügen und GitLab installieren

1. GitLab-Repository hinzufügen und GitLab CE installieren:
    ```bash
    curl https://packages.gitlab.com/install/repositories/gitlab/gitlab-ce/script.deb.sh | sudo bash
    sudo EXTERNAL_URL="http://<interne-ip-adresse>" apt-get install gitlab-ce
    ```

## Schritt 3: Selbstsigniertes SSL-Zertifikat erstellen

1. Erstellen Sie ein Verzeichnis für das SSL-Zertifikat:
    ```bash
    sudo mkdir -p /etc/gitlab/ssl
    ```

2. Erstellen Sie ein selbstsigniertes Zertifikat:
    ```bash
    sudo openssl req -newkey rsa:4096 -nodes -keyout /etc/gitlab/ssl/gitlab.key -x509 -days 365 -out /etc/gitlab/ssl/gitlab.crt
    ```

    **Hinweis:** Während des Vorgangs werden Sie nach verschiedenen Informationen gefragt. Geben Sie die entsprechenden Informationen ein. Für den `Common Name (CN)` verwenden Sie die interne IP-Adresse des Servers.

## Schritt 4: GitLab-Konfigurationsdatei bearbeiten

1. Öffnen Sie die GitLab-Konfigurationsdatei:
    ```bash
    sudo nano /etc/gitlab/gitlab.rb
    ```

2. Fügen Sie die SSL-Zertifikatspfad hinzu und setzen Sie die externe URL:
    ```ruby
    external_url "https://<interne-ip-adresse>"
    nginx['ssl_certificate'] = "/etc/gitlab/ssl/gitlab.crt"
    nginx['ssl_certificate_key'] = "/etc/gitlab/ssl/gitlab.key"
    ```

3. Speichern und schließen Sie die Datei.

## Schritt 5: GitLab-Konfiguration neu laden

1. Laden Sie die GitLab-Konfiguration neu:
    ```bash
    sudo gitlab-ctl reconfigure
    ```

## Schritt 6: Firewall-Regeln konfigurieren

Stellen Sie sicher, dass der Zugriff auf GitLab nur aus dem internen Netzwerk möglich ist.

1. Beispiel mit `iptables`:
    ```bash
    sudo iptables -A INPUT -p tcp --dport 80 -s <internes-netzwerk> -j ACCEPT
    sudo iptables -A INPUT -p tcp --dport 443 -s <internes-netzwerk> -j ACCEPT
    sudo iptables -A INPUT -p tcp --dport 80 -j DROP
    sudo iptables -A INPUT -p tcp --dport 443 -j DROP
    ```

## Schritt 7: Zugang zu GitLab

1. Öffnen Sie einen Browser und greifen Sie auf GitLab zu:
    ```
    https://<interne-ip-adresse>
    ```

2. Einrichtung abschließen:
    - Legen Sie das Admin-Passwort fest.
    - Konfigurieren Sie Benutzer und Projekte gemäß Ihren Anforderungen.

## Schritt 8: Dokumentation in GitLab

1. Erstellen Sie ein Repository für die Dokumentation:
    - Erstellen Sie ein neues Projekt in GitLab.
    - Fügen Sie README.md-Dateien hinzu, um die Installationsschritte und weitere Konfigurationsanweisungen zu dokumentieren.

2. Dokumentation fortlaufend pflegen:
    - Aktualisieren Sie die Dokumentation mit jedem Schritt im Projektplan.

