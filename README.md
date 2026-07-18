# Zeiterfassung

Persönliche Zeiterfassungs-App als installierbare PWA (Progressive Web App) — läuft komplett lokal im Browser, keine Server-Backend nötig. Alle Daten (Einträge, Einstellungen) werden im `localStorage` des Geräts gespeichert.

Enthält: Ein-/Ausstempeln mit Timer, manuelle Einzeltag- und Zeitraum-Erfassung, Urlaubskonto mit Altbestand, Saldo-Übertrag, Arbeitszeitmodell pro Wochentag, Monats-/Wochen-/Jahresberichte, CSV/PDF-Export und JSON-Backup.

## Projektstruktur

```
.
├── index.html              # Die komplette App (HTML/CSS/JS in einer Datei)
├── manifest.json           # PWA-Manifest (Name, Icons, Farben)
├── favicon.ico
├── icons/                  # App-Icons in allen benötigten Größen
├── _headers                # Cloudflare Pages: Cache-Header
└── .github/workflows/      # GitHub Actions: automatisches Pages-Deployment
```

## Lokal testen

Da die App reines HTML/CSS/JS ohne Build-Schritt ist, reicht ein einfacher lokaler Webserver (z. B. wegen `manifest.json`/Icon-Pfaden, die mit `file://` nicht zuverlässig laden):

```bash
npx serve .
# oder
python3 -m http.server 8080
```

Danach im Browser öffnen und optional über "Zum Home-Bildschirm hinzufügen" (iOS) bzw. "App installieren" (Android/Desktop-Chrome) installieren.

## Deployment auf GitHub Pages

1. Repo auf GitHub anlegen und diesen Ordner pushen:
   ```bash
   git init
   git add .
   git commit -m "Initial commit"
   git branch -M main
   git remote add origin https://github.com/<dein-user>/<dein-repo>.git
   git push -u origin main
   ```
2. Im Repo unter **Settings → Pages** als Quelle **GitHub Actions** auswählen.
3. Der mitgelieferte Workflow (`.github/workflows/deploy-pages.yml`) deployed bei jedem Push auf `main` automatisch.
4. Die App ist danach unter `https://<dein-user>.github.io/<dein-repo>/` erreichbar.

> Hinweis: Läuft die Seite nicht im Root (sondern unter einem Unterpfad wie `/<dein-repo>/`), müssen die absoluten Pfade in `index.html` (`/manifest.json`, `/icons/...`) sowie `start_url`/`scope` in `manifest.json` auf den Unterpfad angepasst werden — sonst laden Icons/Manifest nicht. Bei einer eigenen Domain oder Cloudflare Pages ist das nicht nötig.

## Deployment auf Cloudflare Pages

1. Auf [dash.cloudflare.com](https://dash.cloudflare.com) → **Workers & Pages** → **Create** → **Pages** → **Connect to Git**.
2. Dieses Repository auswählen.
3. Build-Einstellungen: **Framework preset: None**, **Build command: (leer lassen)**, **Build output directory: `/`**.
4. Deployen — Cloudflare erkennt automatisch die `_headers`-Datei für Caching-Regeln.

Alternativ ganz ohne Git, direkt per Drag & Drop des Ordners im Cloudflare-Dashboard unter **Pages → Upload assets**.

## Icons ersetzen

Die Icons in `icons/` sind ein einfacher, generierter Platzhalter (Stoppuhr-Motiv in den App-Farben). Eigene Icons einfach unter denselben Dateinamen/Größen ablegen:

| Datei | Größe | Verwendung |
|---|---|---|
| `icon-16.png` / `icon-32.png` | 16×16 / 32×32 | Browser-Favicon |
| `icon-120.png` / `icon-152.png` / `icon-167.png` / `icon-180.png` | iOS Home-Bildschirm |
| `icon-192.png` / `icon-512.png` | Android/Desktop-PWA (`manifest.json`) |
