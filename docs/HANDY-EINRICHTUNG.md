# Jarvis auf dem Handy (und später auf dem Laptop)

Kurz vorweg: Jarvis besteht aus zwei Teilen.

- **Das Gesicht** – eine Webseite (läuft im Browser).
- **Das Gehirn** – die „Bridge“, ein Node-Programm, das Claude Code startet.

Das Gehirn braucht **Node.js** und **Claude Code**. Das geht auf einem
**Android-Handy** (über die App Termux), aber **nicht auf einem iPhone** –
iOS erlaubt keine solchen Hintergrundprogramme.

---

## Android – Schritt für Schritt

1. **Termux installieren** – aus **F-Droid** (<https://f-droid.org>), nicht
   aus dem Play Store (die Play-Store-Version ist veraltet).
2. Termux öffnen und eintippen:

   ```bash
   pkg update && pkg upgrade -y
   pkg install -y nodejs-lts git
   termux-setup-storage
   ```

3. **Claude Code installieren und einloggen:**

   ```bash
   npm install -g @anthropic-ai/claude-code
   claude
   ```

   Beim ersten Start den Login-Link im Browser öffnen und bestätigen.
   Danach mit `/exit` beenden.

4. **Jarvis holen und installieren:**

   ```bash
   git clone https://github.com/einkaufskumpel-coder/jarvis.git
   cd jarvis
   npm install
   ```

5. **Starten:**

   ```bash
   termux-wake-lock        # verhindert, dass Android Termux einschläfert
   JARVIS_CLAUDE_EXECUTABLE="$(command -v claude)" npm start
   ```

   `JARVIS_CLAUDE_EXECUTABLE` sorgt dafür, dass die Bridge dein installiertes
   `claude` benutzt – das mitgelieferte Programm läuft unter Android oft nicht.

6. **Chrome auf dem Handy öffnen** → `http://localhost:5173`
   → **INITIALISE** antippen → Mikrofon erlauben → **„Hey Jarvis“** sagen.

### Nächstes Mal

```bash
cd jarvis
termux-wake-lock
JARVIS_CLAUDE_EXECUTABLE="$(command -v claude)" npm start
```

### Wenn's hakt

- **Seite lädt, aber Jarvis hört nicht:** Es muss Chrome sein, kein
  eingebetteter Browser. Mikrofon in den Chrome-Einstellungen erlauben.
- **Termux bricht ab, wenn das Display ausgeht:** In den Android-Einstellungen
  für Termux die **Akku-Optimierung ausschalten**.
- **Handy wird heiß/langsam:** In Chrome mit **D** (oder über das Menü) die
  Diagnose öffnen; die 3D-Animation ist das Schwerste. Das Gehirn selbst läuft
  auf Anthropics Servern.
- **`npm install` scheitert:** `pkg install -y python make clang` und nochmal
  versuchen.

> Hinweis: Claude Code unter Termux wird von Anthropic nicht offiziell
> unterstützt. Bei vielen läuft es, garantiert ist es nicht.

---

## iPhone

Auf dem iPhone selbst kann das Gehirn nicht laufen. Bis der Laptop da ist:

- Nutze die **Claude-App** mit dem **Sprachmodus** – das ist im Alltag das
  Nächste an Jarvis.
- Sobald der Laptop da ist, läuft Jarvis dort (siehe unten).

Jarvis vom Laptop aufs iPhone „durchzureichen“ ist möglich, aber die Bridge
ist absichtlich nur lokal erreichbar. Ohne eigene Absicherung (Passwort/VPN)
davor wäre dein Claude-Zugang im WLAN offen – das richten wir erst ein, wenn
der Laptop da ist.

---

## Später: Laptop (Windows, Mac oder Linux)

1. **Node.js 20+** von <https://nodejs.org> installieren.
2. **Google Chrome** installieren.
3. Terminal öffnen:

   ```bash
   npm install -g @anthropic-ai/claude-code
   claude                  # einmal einloggen, dann /exit
   git clone https://github.com/einkaufskumpel-coder/jarvis.git
   cd jarvis
   npm install
   npm run setup           # prüft, ob alles passt
   npm start
   ```

4. Chrome → `http://localhost:5173` → **INITIALISE** → **„Hey Jarvis“**.

Jarvis startet **nur lesend** (sucht, schaut nach, erzeugt Bilder). Soll er
wirklich handeln (Handy steuern, Browser bedienen, Dinge senden), starte mit
`npm start -- --writes` – vorher aber `bridge/policy.mjs` lesen.
