# GitLab Installation und Registrierung deaktivieren

## Installation von GitLab

1. Führen Sie das Installationsskript aus:

    ```bash
    curl https://packages.gitlab.com/install/repositories/gitlab/gitlab-ce/script.deb.sh | sudo bash
    ```

2. Aktualisieren Sie die Paketquellen auf Ubuntu 22.04 (Jammy):

    ```bash
    sudo sed -i "s/noble/jammy/g" /etc/apt/sources.list.d/gitlab_gitlab-*e.list
    sudo apt update && sudo apt upgrade -y
    ```

3. Installieren Sie GitLab:

    ```bash
    sudo EXTERNAL_URL="https://gitlab.test.azubi.gkd-re.de" apt-get install gitlab-ce
    ```

## GitLab SSL-Konfiguration

1. Öffnen Sie die Datei `/etc/gitlab/gitlab.rb` und fügen oder bearbeiten Sie folgende Zeilen:

    ```ruby
    external_url "https://gitlab.test.azubi.gkd-re.de"
    nginx['redirect_http_to_https'] = true
    nginx['ssl_certificate'] = "/etc/gitlab/ssl/gitlab.test.azubi.gkd-re.de-bundle.crt"
    nginx['ssl_certificate_key'] = "/etc/gitlab/ssl/gitlab.test.azubi.gkd-re.de.key"
    ```

2. Wenden Sie die Änderungen an:

    ```bash
    sudo gitlab-ctl reconfigure
    ```

## Registrierung bei GitLab deaktivieren

1. Öffnen Sie die GitLab-Konfigurationsdatei (`gitlab.rb`) mit einem Texteditor:

    ```bash
    sudo nano /etc/gitlab/gitlab.rb
    ```

2. Fügen Sie die folgende Zeile hinzu oder bearbeiten Sie sie, falls sie bereits vorhanden ist:

    ```ruby
    gitlab_rails['gitlab_signup_enabled'] = false
    ```

3. Wenden Sie die Änderungen an:

    ```bash
    sudo gitlab-ctl reconfigure
    ```

4. Falls erforderlich, starten Sie GitLab neu:

    ```bash
    sudo gitlab-ctl restart
    ```

## Bestätigen der Änderung

1. Melden Sie sich bei Ihrer GitLab-Instanz an und prüfen Sie, ob der Registrierungslink auf der Anmeldeseite entfernt wurde.

## Weitere Einstellungen (optional)

Falls Sie auch das Sign-in von neuen Benutzern über externe Provider (wie z.B. Google oder GitHub) verhindern möchten, können Sie diese Option ebenfalls in der `gitlab.rb` Datei konfigurieren:

    ```ruby
    gitlab_rails['omniauth_allow_single_sign_on'] = false
    gitlab_rails['omniauth_block_auto_created_users'] = true
    ```

Diese zusätzlichen Optionen verhindern die automatische Erstellung von Benutzern durch externe Anmeldedienste und können für zusätzliche Sicherheit sorgen.

Nachdem Sie diese Schritte ausgeführt haben, sollte die Registrierung neuer Benutzer auf Ihrer GitLab-Instanz deaktiviert sein.
