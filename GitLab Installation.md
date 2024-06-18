# GitLab Installation mit selbstsigniertem SSL-Zertifikat und Exchange Server für E-Mails

Diese Anleitung beschreibt die Installation von GitLab CE auf einer Ubuntu-VM im VLAN 10 (Management) mit einem selbstsignierten SSL-Zertifikat und die Konfiguration eines Exchange Servers für den E-Mail-Versand.

## Voraussetzungen

- Ubuntu 20.04/22.04 LTS installiert
- Netzwerkzugriff im VLAN 10
- Root- oder Sudo-Zugriff auf das System
- Zugriff auf einen Exchange Server für die E-Mail-Konfiguration

## Schritt 1: Systemvorbereitung

1. Aktualisieren Sie das System:
    ```bash
    sudo apt update && sudo apt upgrade -y
    sudo reboot
    ```

## Schritt 2: GitLab-Abhängigkeiten installieren

1. Installieren Sie die benötigten Abhängigkeiten:
    ```bash
    sudo apt install -y curl openssh-server ca-certificates tzdata perl
    ```

## Schritt 3: GitLab-Repository hinzufügen und GitLab installieren

1. GitLab-Repository hinzufügen und GitLab CE installieren:
    ```bash
    curl https://packages.gitlab.com/install/repositories/gitlab/gitlab-ce/script.deb.sh | sudo bash
    sudo EXTERNAL_URL="http://<interne-ip-adresse>" apt-get install gitlab-ce
    ```

## Schritt 4: Selbstsigniertes SSL-Zertifikat erstellen

1. Erstellen Sie ein Verzeichnis für das SSL-Zertifikat:
    ```bash
    sudo mkdir -p /etc/gitlab/ssl
    ```

2. Erstellen Sie ein selbstsigniertes Zertifikat:
    ```bash
    sudo openssl req -newkey rsa:4096 -nodes -keyout /etc/gitlab/ssl/gitlab.key -x509 -days 365 -out /etc/gitlab/ssl/gitlab.crt
    ```

    **Hinweis:** Während des Vorgangs werden Sie nach verschiedenen Informationen gefragt. Geben Sie die entsprechenden Informationen ein. Für den `Common Name (CN)` verwenden Sie die interne IP-Adresse des Servers.

## Schritt 5: GitLab-Konfigurationsdatei bearbeiten

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

3. Konfigurieren Sie den E-Mail-Versand über den Exchange Server:
    ```ruby
    gitlab_rails['smtp_enable'] = true
    gitlab_rails['smtp_address'] = "exchange.server.domain"
    gitlab_rails['smtp_port'] = 587
    gitlab_rails['smtp_user_name'] = "gitlab@example.com"
    gitlab_rails['smtp_password'] = "your_password"
    gitlab_rails['smtp_domain'] = "example.com"
    gitlab_rails['smtp_authentication'] = "login"
    gitlab_rails['smtp_enable_starttls_auto'] = true
    gitlab_rails['smtp_tls'] = false
    gitlab_rails['smtp_openssl_verify_mode'] = 'peer'
    gitlab_rails['gitlab_email_from'] = 'gitlab@example.com'
    gitlab_rails['gitlab_email_reply_to'] = 'noreply@example.com'
    ```

4. Speichern und schließen Sie die Datei.

## Schritt 6: GitLab-Konfiguration neu laden

1. Laden Sie die GitLab-Konfiguration neu:
    ```bash
    sudo gitlab-ctl reconfigure
    ```

## Schritt 7: Firewall-Regeln konfigurieren

Stellen Sie sicher, dass der Zugriff auf GitLab nur aus dem internen Netzwerk möglich ist.

1. Beispiel mit `iptables`:
    ```bash
    sudo iptables -A INPUT -p tcp --dport 80 -s <internes-netzwerk> -j ACCEPT
    sudo iptables -A INPUT -p tcp --dport 443 -s <internes-netzwerk> -j ACCEPT
    sudo iptables -A INPUT -p tcp --dport 80 -j DROP
    sudo iptables -A INPUT -p tcp --dport 443 -j DROP
    ```

## Schritt 8: Zugang zu GitLab

1. Öffnen Sie einen Browser und greifen Sie auf GitLab zu:
    ```
    https://<interne-ip-adresse>
    ```

2. Einrichtung abschließen:
    - Legen Sie das Admin-Passwort fest.
    - Konfigurieren Sie Benutzer und Projekte gemäß Ihren Anforderungen.

## Schritt 9: Dokumentation in GitLab

1. Erstellen Sie ein Repository für die Dokumentation:
    - Erstellen Sie ein neues Projekt in GitLab.
    - Fügen Sie README.md-Dateien hinzu, um die Installationsschritte und weitere Konfigurationsanweisungen zu dokumentieren.

2. Dokumentation fortlaufend pflegen:
    - Aktualisieren Sie die Dokumentation mit jedem Schritt im Projektplan.

Mit diesen Schritten haben Sie einen provisorischen GitLab-Server eingerichtet, der nur intern verfügbar ist, über ein selbstsigniertes SSL-Zertifikat gesichert ist und den Exchange Server für den E-Mail-Versand verwendet. Sie können nun die weiteren Schritte dokumentieren und fortfahren.