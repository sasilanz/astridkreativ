Webseite orion5d.com von bandzoogle zu local gehostet umziehen.
files sind bereits lokal in diesen ordner hier kopiert.
ich möchte eine selbst gehostete kleine und einfache webseite haben, die auf dem raspberry läuft, wo auch bereits der dieti-it.ch und die rapporte.dieti-it.ch laufen.
gerne ebenfalls mit docker, oder ganz einfach mit statischer html. diese seite soll einfach ein paar lieder, bilder hosten, muss nichts gross können, also es ist keine webapp nötig, darf auch ein static html sein, zb mit nginx

github repo:
https://github.com/sasilanz/Dietipi-App (schau das Readme.md an, cloudflare-tunnel und resend für emails)

---

# Orion5d.com — Projekt-Plan

## Entscheidungen
- Kein Kontaktformular — rein statische Seite
- Logo siehe /assets/logo.webp— Text-Titel "Astrid's Kreativecke"
- Bestehender Cloudflare Tunnel wird mitbenutzt (neuer Public Hostname im Dashboard)
- Statisches HTML + Nginx + Docker + Cloudflare Tunnel

## Referenz-Infrastruktur
- Dietipi-App Repo: https://github.com/sasilanz/Dietipi-App
- Pattern: compose.yml (Basis) + compose.dev.yml (lokal) + compose.prod.yml (mit cloudflared)
- Raspberry Pi User: pi@dietipi

---

## Plan — Schritt für Schritt

### Schritt 1: Git-Repository initialisieren ⬜
- `git init` im Projektordner ~/dev/orion5d
- `.gitignore` erstellen (.env, .DS_Store, etc.)
- GitHub-Repo erstellen und als Remote verbinden

### Schritt 2: Statische HTML-Seiten erstellen ⬜

**`index.html`** — Hauptseite:
- Header mit Text-Titel "Astrid's Kreativecke"
- Musik-Sektion: HTML5 `<audio>` Player mit 7 MP3s
- About-Sektion: Bio von Astrid Lanz - muss noch auf Deutsch übersetzt werden
- Navigation zu Zeichnungen-Seite

**`zeichnungen.html`** — Galerie:
- Bildergalerie mit 13 Zeichnungen aus `assets/img/`
- Lightbox (reines CSS/JS, keine externen Abhängigkeiten)
- Zurück-Link zur Hauptseite

**`css/style.css`** — Design:
- Dunkelblau (#2b2d42) Hintergrund, Türkis (#29b8c0) Akzente
- Schrift: Fira Sans (Google Fonts)
- Responsives Layout

### Schritt 3: Docker-Setup ⬜

**`Dockerfile`** — Nginx mit statischen Dateien
**`nginx.conf`** — Gzip, Cache-Header, Port 80
**`compose.yml`** — Basis (web Service + Netzwerk)
**`compose.dev.yml`** — Lokal: Port 8080, Volume-Mount für Live-Reload
**`compose.prod.yml`** — Produktion: cloudflared Service

### Schritt 4: Lokal testen ⬜
```bash
docker compose -f compose.yml -f compose.dev.yml up --build
# → http://localhost:8080
```
- Alle Musik-Dateien spielen ab
- Alle Bilder laden korrekt
- Galerie mit Lightbox funktioniert
- Responsiv auf Mobile testen

### Schritt 5: GitHub-Repo pushen ⬜
- Erster Commit mit allen Dateien
- Push zu GitHub

### Schritt 6: Cloudflare Tunnel konfigurieren ⬜
- Im Cloudflare Dashboard: Public Hostname `orion5d.com` → Service `http://web:80`
- DNS auf Cloudflare Tunnel zeigen lassen

### Schritt 7: Auf Raspberry Pi deployen ⬜
```bash
git clone <repo-url> /home/pi/Orion5d
cd /home/pi/Orion5d
# .env mit CLOUDFLARE_TUNNEL_TOKEN anlegen
docker compose -f compose.yml -f compose.prod.yml up -d --build
```

### Schritt 8: DNS umstellen ⬜
- orion5d.com DNS von Bandzoogle auf Cloudflare umstellen

---

## Projektstruktur (Ziel)
```
Orion5d/
├── index.html
├── drawings.html
├── css/
│   └── style.css
├── assets/
│   ├── img/          (13 Zeichnungen)
│   ├── music/        (7 MP3s)
│   └── files/
├── aboutme/
│   └── aboutme.md
├── nginx.conf
├── Dockerfile
├── compose.yml
├── compose.dev.yml
├── compose.prod.yml
├── .gitignore
├── .env.example
└── Claude.md
```
