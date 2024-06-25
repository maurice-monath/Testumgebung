## Sicherstellen, dass die NTLMv1- und alten LM-Protokolle gesperrt sind

**Regel-ID:** S-OldNtlm

**Beschreibung:**
Der Zweck besteht darin zu überprüfen, ob NTLMv1 oder LM vom DC verwendet werden können.

**Empfohlene Lösung:**
Nach einer Überprüfung der Nutzung von NTLMv1 (siehe die untenstehenden Links) müssen Sie das LAN-Manager-Authentifizierungslevel auf "Nur NTLMv2-Antwort senden. LM & NTLM verweigern" erhöhen.
Dies kann durch Bearbeiten der Richtlinie "Netzwerksicherheit: LAN-Manager-Authentifizierungslevel" erfolgen, die unter Computerkonfiguration\Windows-Einstellungen\Sicherheitseinstellungen\Lokale Richtlinien\Sicherheitsoptionen zu finden ist.
Die Richtlinie wird nach einem Neustart des Computers angewendet.

Beachten Sie, dass Sie Software, die nicht mit Ntlmv2 kompatibel ist, wie z.B. sehr alte Linux-Stacks oder sehr alte Windows-Versionen vor Windows Vista, möglicherweise nicht mehr verwenden können.
Bitte beachten Sie jedoch, dass Ntlmv2 auf allen Windows-Versionen ab Windows 95 und anderen Betriebssystemen aktiviert werden kann.

### Schritte zur Konfiguration der Gruppenrichtlinie

1. **Öffnen der Gruppenrichtlinienverwaltungskonsole (GPMC):**
   - Melden Sie sich an einem Computer an, auf dem die Gruppenrichtlinienverwaltung installiert ist (z. B. Ihre PAW).
   - Öffnen Sie die GPMC (Gruppenrichtlinienverwaltungskonsole) über Start -> Ausführen -> `gpmc.msc`.

2. **Erstellen einer neuen GPO:**
   - Rechtsklicken Sie auf die Domäne oder die Organisationseinheit (OU), auf die die Richtlinie angewendet werden soll.
   - Wählen Sie "Gruppenrichtlinienobjekt hier erstellen und verknüpfen..." aus.
   - Geben Sie der neuen GPO einen Namen, z. B. "Sperre NTLMv1 und LM".

3. **Bearbeiten der GPO:**
   - Rechtsklicken Sie auf das neu erstellte Gruppenrichtlinienobjekt und wählen Sie "Bearbeiten" aus.
   - Navigieren Sie zu  `Computer Configuration` -> `Policies` -> `Windows Settings` -> `Security Settings` -> `Local Policies` -> `Security Options`.


4. **Konfigurieren der Richtlinie "Netzwerksicherheit: LAN Manager-Authentifizierungslevel":**
   - Suchen Sie die Richtlinie "Netzwerksicherheit: LAN Manager-Authentifizierungslevel".
   - Doppelklicken Sie darauf, um die Eigenschaften zu öffnen.
   - Stellen Sie die Einstellung auf "Nur NTLMv2-Antwort senden. LM und NTLM verweigern".
   - Klicken Sie auf "Übernehmen" und dann auf "OK".

5. **Überprüfen der GPO-Anwendung:**
   - Schließen Sie den Gruppenrichtlinienverwaltungs-Editor.
   - Stellen Sie sicher, dass die GPO mit den gewünschten OUs oder der Domäne verknüpft ist.

6. **Erzwingen der Gruppenrichtlinienaktualisierung:**
   - Sie können die Gruppenrichtlinien auf den Client-Computern aktualisieren, indem Sie `gpupdate /force` auf den Computern ausführen.
   - Alternativ warten Sie, bis die Gruppenrichtlinienaktualisierung automatisch erfolgt (Standardintervall beträgt 90 Minuten).

Durch diese Schritte stellen Sie sicher, dass Ihre Active Directory-Computer korrekt zu WSUS hinzugefügt und verwaltet werden. Die Verwendung von GPOs ermöglicht eine zentrale und konsistente Verwaltung der WSUS-Einstellungen in Ihrer gesamten Domäne.
