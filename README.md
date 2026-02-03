# Supabase QA-Update Script (GitHub-sicher)

Dieses Repository enthält ein Python-Script, das Datensätze in einer Supabase-Tabelle lädt, pro Frage mit DeepSeek prüft und optional Updates zurückschreibt.

## Sicherheitsprinzip

Dieses Script ist so angepasst, dass **keine Zugangsdaten im Code** stehen. Alle Secrets kommen ausschließlich aus:

1. **Umgebungsvariablen** (empfohlen), oder
2. einer lokalen Datei **secrets.json** (wird per `.gitignore` ausgeschlossen).

## Voraussetzungen

- Python 3.10+ empfohlen
- Pakete:
  - `requests`
  - `colorama` (optional, für Farben)

Installation:

```bash
python -m venv .venv
# Windows: .venv\Scripts\activate
source .venv/bin/activate
pip install -U pip
pip install requests colorama
```

## Konfiguration

### Option A: Umgebungsvariablen (empfohlen)

Linux/macOS:

```bash
export SUPABASE_URL='https://xxxx.supabase.co'
export SUPABASE_KEY='...'
export DEEPSEEK_API_KEY='sk-...'
# optional
export DEEPSEEK_BASE_URL='https://api.deepseek.com'
export DEEPSEEK_MODEL='deepseek-chat'
```

Windows PowerShell:

```powershell
setx SUPABASE_URL "https://xxxx.supabase.co"
setx SUPABASE_KEY "..."
setx DEEPSEEK_API_KEY "sk-..."
# optional
setx DEEPSEEK_BASE_URL "https://api.deepseek.com"
setx DEEPSEEK_MODEL "deepseek-chat"
```

### Option B: Lokale secrets.json (nur lokal)

1. Erzeuge eine leere Vorlage:

```bash
python update_sanitized.py --reset-secrets
```

2. Trage dann lokal in `secrets.json` die Werte ein.

Hinweis: `secrets.example.json` dient nur als Beispiel und enthält keine echten Secrets.

## Nutzung

Beispiel: 20 Datensätze aus Tabelle `deutsch` prüfen:

```bash
python update_sanitized.py --table deutsch --limit 20
```

Nur einen Datensatz per ID:

```bash
python update_sanitized.py --only-id 123
```

Filter:

```bash
python update_sanitized.py --filter Sprache=Deutsch --limit 50
```

Automatisch ohne Bestätigungen:

```bash
python update_sanitized.py --auto
```

## Supabase-Key: Empfehlung

Wenn du das Script auf einem Server nutzt, verwende einen Key mit minimal notwendigen Rechten. Ein `service_role` Key ist sehr mächtig und sollte:

- nicht in Client-Anwendungen landen,
- nur serverseitig genutzt werden,
- sauber geschützt und regelmäßig rotiert werden.
