# Codex Account Swapper

Automatischer OAuth-Login für mehrere OpenAI/Codex-Accounts. Unterstützt mehrere Accounts in einer `codex.env`, automatisches OTP-Abrufen über jeden IMAP-kompatiblen Mailserver und interaktiven Fallback auf manuelle Eingabe.

## Voraussetzungen

- Python 3.10+
- [Playwright](https://playwright.dev/python/) mit Chromium
- Codex CLI (`codex` im PATH)
- Optional: IMAP-Zugang für automatischen OTP-Abruf

```bash
pip install playwright
playwright install chromium
```

## Einrichtung

### 1. Accounts konfigurieren

`codex.env` im Script-Verzeichnis befüllen:

```
email=user1@example.com
email2=user2@example.com
password=deinPasswort
# workspace=business   # oder: personal
```

### 2. Umgebungsvariablen setzen

Alle Konfiguration erfolgt über Env-Vars — keine Konfigurationsdateien nötig.

| Variable | Beschreibung | Standard |
|----------|-------------|---------|
| `CODEX_STORE_DIR` | Verzeichnis mit `codex.env` und `state.json` | Script-Verzeichnis |
| `IMAP_HOST` | IMAP-Server (z. B. `imap.gmail.com`, `imap.ionos.de`) | — |
| `IMAP_PORT` | IMAP-Port (SSL) | `993` |
| `IMAP_USER` | IMAP-Benutzername / E-Mail-Adresse | — |
| `IMAP_PASSWORD` | IMAP-Passwort | — |
| `IMAP_OTP_SENDER` | Absender-Filter für OTP-E-Mails | `openai.com` |

Beispiel für Gmail:
```bash
export IMAP_HOST=imap.gmail.com
export IMAP_USER=mein@gmail.com
export IMAP_PASSWORD=app-passwort
```

Ohne `IMAP_HOST`/`IMAP_USER`/`IMAP_PASSWORD` wird das OTP beim Login manuell abgefragt.

### 3. Script ausführbar machen

```bash
chmod +x codex-login
```

## Verwendung

```bash
# Interaktives Menü
./codex-login

# Account einloggen (Auswahl per Nummer)
./codex-login login

# Bestimmten Account einloggen
./codex-login login user@example.com

# Alle Accounts der Reihe nach einloggen
./codex-login login-all

# Status und Rate-Limits anzeigen
./codex-login status

# Access-Token erneuern
./codex-login refresh
```

## Dateien

| Datei | Beschreibung |
|-------|-------------|
| `codex-login` | Haupt-Script |
| `codex.env` | Account-Konfiguration (Vorlage — echte Daten nicht einchecken) |
| `state.json` | Automatisch generiert — Login-Zeiten & Limits (von `.gitignore` ausgeschlossen) |

## Hinweise

- `state.json` wird automatisch angelegt und ist per `.gitignore` ausgeschlossen.
- Der Chromium-Browser öffnet sich sichtbar (nicht headless), um Bot-Erkennung zu vermeiden.
- IMAP-OTP funktioniert mit jedem Standard-IMAP-Server (Gmail, Outlook, IONOS, etc.).
- Kein OTP-Abruf nötig, wenn der Account keine 2FA hat.
