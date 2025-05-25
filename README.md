# Meine Dotfiles

Dieses Repository enthält meine persönlichen Konfigurationsdateien (Dotfiles) für eine konsistente Arbeitsumgebung auf verschiedenen Linux-Systemen, insbesondere für KDE Plasma basierend auf EndeavourOS.

Die Verwaltung erfolgt über ein *Bare* Git-Repository, das direkt im Home-Verzeichnis (`$HOME`) des Benutzers arbeitet.

---

## Voraussetzungen

- **Git** – muss installiert sein.
- **Shell** – getestet mit **Zsh** (empfohlen). Bash funktioniert eventuell auch, aber einige Einstellungen sind zsh-spezifisch.
- **GitHub-Zugriff**:
  - **HTTPS**: Ein [Personal Access Token (PAT)](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/managing-your-personal-access-tokens) mit dem Scope `repo`.
  - **SSH**: Ein eingerichtetes SSH-Schlüsselpaar, dessen öffentlicher Schlüssel bei GitHub hinterlegt ist.

---

## Installation auf einem neuen System

### 1. Repository klonen (als *Bare Repository*)

```bash
# Mit HTTPS:
git clone --bare https://github.com/oliverschmidt99/dotfiles.git $HOME/.dotfiles

# Oder mit SSH:
git clone --bare git@github.com:oliverschmidt99/dotfiles.git $HOME/.dotfiles
```

---

### 2. `config`-Alias einrichten

```bash
# Temporär für die aktuelle Sitzung:
alias config='/usr/bin/git --git-dir=$HOME/.dotfiles/ --work-tree=$HOME'
```

Die dauerhafte Alias-Definition befindet sich in der ausgecheckten `.zshrc` oder `.bashrc`.

---

### 3. Lokale Dotfiles sichern (vor dem Checkout)

```bash
BACKUP_DIR_FOR_DOTFILES="$HOME/dotfiles_backup_$(date +"%Y%m%d_%H%M%S")"
mkdir -p "$BACKUP_DIR_FOR_DOTFILES"
echo "Backup-Verzeichnis erstellt: $BACKUP_DIR_FOR_DOTFILES"
```

Beispiel für das Verschieben möglicher Konflikt-Dateien:

```bash
mv ~/.zshrc "$BACKUP_DIR_FOR_DOTFILES/zshrc.backup" 2>/dev/null || true
mv ~/.bashrc "$BACKUP_DIR_FOR_DOTFILES/bashrc.backup" 2>/dev/null || true
mv ~/.bash_profile "$BACKUP_DIR_FOR_DOTFILES/bash_profile.backup" 2>/dev/null || true
mv ~/.bash_logout "$BACKUP_DIR_FOR_DOTFILES/bash_logout.backup" 2>/dev/null || true
mv ~/.gitconfig "$BACKUP_DIR_FOR_DOTFILES/gitconfig.backup" 2>/dev/null || true
```

Optional: KDE- und `.config`-Dateien:

```bash
mkdir -p "$BACKUP_DIR_FOR_DOTFILES/.config_backup"
mv ~/.config/kwinrc "$BACKUP_DIR_FOR_DOTFILES/.config_backup/kwinrc" 2>/dev/null || true
mv ~/.config/plasmashellrc "$BACKUP_DIR_FOR_DOTFILES/.config_backup/plasmashellrc" 2>/dev/null || true
mv ~/.config/kglobalshortcutsrc "$BACKUP_DIR_FOR_DOTFILES/.config_backup/kglobalshortcutsrc" 2>/dev/null || true
mv ~/.config/dolphinrc "$BACKUP_DIR_FOR_DOTFILES/.config_backup/dolphinrc" 2>/dev/null || true
mv ~/.config/konsolerc "$BACKUP_DIR_FOR_DOTFILES/.config_backup/konsolerc" 2>/dev/null || true
```

---

### 4. Dotfiles auschecken

```bash
config checkout master
```

Wenn Konflikte auftreten („unversionierte Dateien würden überschrieben“), verschiebe diese manuell ins Backup (siehe oben) und wiederhole den Befehl.

Optional (nur wenn **sicher**):

```bash
config checkout -f master
```

---

### 5. Git-Konfiguration anpassen (empfohlen)

```bash
config config --local status.showUntrackedFiles no
```

---

### 6. Abschluss: Shell neu starten oder Konfig laden

```bash
# Für Zsh:
source ~/.zshrc

# Für Bash:
source ~/.bashrc
```

---

## Dotfiles verwalten

```bash
# Status anzeigen:
config status

# Datei hinzufügen:
config add ~/.pfad/zur/datei

# Änderungen committen:
config commit -m "Meine Commit-Nachricht"

# Änderungen pushen:
config push origin master

# Änderungen von GitHub holen:
config pull origin master
```

---

## Fehlerbehebung

- `config: command not found`  
  → Alias nicht definiert? Siehe Schritt 2 und stelle sicher, dass deine Shell-Konfiguration geladen ist.

- Probleme beim Klonen/Pushen über HTTPS  
  → Stelle sicher, dass du ein gültiges **PAT** verwendest und nicht dein normales Passwort.

- SSH-Authentifizierungsprobleme  
  → Überprüfe deinen SSH-Schlüssel lokal und bei GitHub (unter `SSH and GPG keys`).

---

## Lizenz

Diese Dotfiles sind auf meine persönliche Umgebung zugeschnitten. Verwende sie gerne als Inspiration, aber übernimm Konfigurationen mit Bedacht.
