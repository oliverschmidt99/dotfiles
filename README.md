# Meine Dotfiles

Dieses Repository enthält meine persönlichen Konfigurationsdateien (Dotfiles) für eine konsistente Arbeitsumgebung auf verschiedenen Linux-Systemen, insbesondere für KDE Plasma basierend auf EndeavourOS.

Die Verwaltung erfolgt über ein "Bare" Git-Repository, das im Home-Verzeichnis des Benutzers operiert.

## Voraussetzungen

* **Git**: Muss auf dem System installiert sein.
* **Shell**: Getestet mit Zsh (empfohlen, da meine Konfigurationen darauf ausgelegt sind, z.B. `.zshrc`). Bash wird möglicherweise auch funktionieren, aber einige Shell-spezifische Einstellungen sind für Zsh.
* **Authentifizierung bei GitHub**:
    * Für HTTPS-URLs (empfohlen für den ersten Clone, wenn SSH nicht eingerichtet ist): Ein [Personal Access Token (PAT)](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/managing-your-personal-access-tokens) mit dem Scope `repo`.
    * Für SSH-URLs: Ein auf dem lokalen Rechner eingerichtetes SSH-Schlüsselpaar, dessen öffentlicher Schlüssel zu deinem GitHub-Account hinzugefügt wurde.

## Installation auf einem neuen System

Diese Schritte beschreiben, wie du diese Dotfiles auf einem neuen System einrichtest.

### 1. Repository klonen (als Bare-Repository)

Wir klonen das Repository als "Bare"-Repository. Das bedeutet, der `.git`-Ordner selbst ist der Klon, und es gibt keinen separaten Arbeitsbaum für das Repository selbst. Wir werden später Git anweisen, dein `$HOME`-Verzeichnis als Arbeitsbaum zu verwenden.

Öffne ein Terminal und wähle **eine** der folgenden Methoden:

* **Mit HTTPS (empfohlen, wenn SSH nicht eingerichtet ist):**
    ```bash
    git clone --bare [https://github.com/oliverschmidt99/dotfiles.git](https://github.com/oliverschmidt99/dotfiles.git) $HOME/.dotfiles
    ```
    Du wirst nach deinem GitHub-Benutzernamen und einem Passwort gefragt. Gib hier dein **Personal Access Token (PAT)** als Passwort ein.

* **Mit SSH (wenn SSH-Schlüssel eingerichtet sind):**
    ```bash
    git clone --bare git@github.com:oliverschmidt99/dotfiles.git $HOME/.dotfiles
    ```

### 2. Den `config`-Alias einrichten

Um die Interaktion mit dem Bare-Repository zu vereinfachen, verwenden wir einen Alias namens `config`.

* **Temporär für die aktuelle Sitzung (notwendig für die nächsten Schritte):**
    Definiere den Alias direkt in deiner aktuellen Terminalsitzung:
    ```bash
    alias config='/usr/bin/git --git-dir=$HOME/.dotfiles/ --work-tree=$HOME'
    ```

* **Permanent:**
    Die `config`-Alias-Definition sollte bereits Teil deiner `~/.zshrc` (oder `~/.bashrc`) in diesem Repository sein. Sobald die Dateien ausgecheckt sind und du deine Shell neu startest oder die Konfigurationsdatei sourct, wird der Alias permanent verfügbar sein.

### 3. Bestehende lokale Dotfiles sichern

**WICHTIG:** Dieser Schritt ist entscheidend, um den Verlust vorhandener lokaler Konfigurationen auf dem neuen System zu vermeiden!

Erstelle ein Backup-Verzeichnis:
```bash
mkdir -p ~/dotfiles_backup_$(date +"%Y%m%d_%H%M%S")
```
Verschiebe nun alle lokalen Dateien, die mit denen im Repository kollidieren könnten, in dieses Backup-Verzeichnis. Hier sind einige Beispiele (passe diese Liste an, falls du andere wichtige lokale Konfigs hast, die durch das Repository überschrieben würden):

# Beispiel: Shell-Konfigurationen
```bash
mv ~/.zshrc ~/dotfiles_backup_$(date +"%Y%m%d_%H%M%S")/zshrc.backup 2>/dev/null || true
mv ~/.bashrc ~/dotfiles_backup_$(date +"%Y%m%d_%H%M%S")/bashrc.backup 2>/dev/null || true
mv ~/.bash_profile ~/dotfiles_backup_$(date +"%Y%m%d_%H%M%S")/bash_profile.backup 2>/dev/null || true
mv ~/.bash_logout ~/dotfiles_backup_$(date +"%Y%m%d_%H%M%S")/bash_logout.backup 2>/dev/null || true
```

# Beispiel: Git-Konfiguration
```bash
mv ~/.gitconfig ~/dotfiles_backup_$(date +"%Y%m%d_%H%M%S")/gitconfig.backup 2>/dev/null || true
```

# Beispiel: KDE/Plasma und andere .config-Dateien (passe dies an deine Repository-Inhalte an!)
# Es ist oft einfacher, die Fehlermeldung von 'config checkout' abzuwarten (siehe nächster Schritt)
# und dann gezielt die dort genannten Dateien zu verschieben.
# Beispielhaft:
```bash
# mv ~/.config/kwinrc ~/dotfiles_backup_$(date +"%Y%m%d_%H%M%S")/kwinrc.backup 2>/dev/null || true
# mv ~/.config/plasmashellrc ~/dotfiles_backup_$(date +"%Y%m%d_%H%M%S")/plasmashellrc.backup 2>/dev/null || true
```
