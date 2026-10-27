# Satzbau

Trainingstagebuch fürs Fitnessstudio (Geräte nach McFit-Ausstattung). Läuft als installierbare Web-App (PWA) auf Android: eigenes Symbol, eigenes Fenster ohne Adressleiste, auch ohne Netz nutzbar.

## Funktionen

- Trainingspläne (Plan A/B) mit Startvorschlag fürs Gewicht (doppelte Progression)
- Satzeingabe mit +/−-Knöpfen, Pausen-Countdown (je Übungsart oder je Gerät einstellbar)
- Bildschirm bleibt im laufenden Training an
- Ampel je Übung: Auf Kurs, Stagniert, Hinter Plan
- Verlaufsdiagramme je Übung (1RM geschätzt, Topgewicht, Volumen)
- Körpergewicht mit Verlauf
- Übersicht mit drei Kernübungen, Wochen- und Monatsansicht
- Stammdaten je Gerät: Geräte-Nr., Sitzeinstellung, Schrittweite, Wiederholungsbereich, Pause, Kernübung
- Plausibilitätsprüfung für Gewicht und Wiederholungen (Rückfrage bei ungewöhnlichen Werten)
- Import aus Strong (CSV), Sicherung und Wiederherstellung (JSON), Export als CSV für Excel

## 1. Auf GitHub Pages veröffentlichen

1. Neues Repository auf GitHub anlegen (z. B. `satzbau`) und diese Dateien hochladen oder pushen (siehe unten).
2. Im Repository: **Settings → Pages → Build and deployment → Source: Deploy from a branch**, Branch `main`, Ordner `/ (root)`, speichern.
3. Nach ein bis zwei Minuten ist die App unter `https://<benutzername>.github.io/<repository>/` erreichbar.

## 2. Auf Android installieren

1. Die Adresse in **Chrome** öffnen und einmal kurz benutzen.
2. Menü (⋮) → **App installieren** (manchmal „Zum Startbildschirm hinzufügen“ → „Installieren“).
3. Satzbau liegt jetzt wie jede andere App in der App-Liste und auf dem Startbildschirm und öffnet sich ohne Browser-Leiste.
4. Langes Drücken auf das Symbol zeigt Schnellzugriffe: Training, Übersicht, Verlauf.

Erscheint nur „Verknüpfung hinzufügen“ statt „App installieren“, die Seite einmal neu laden und über `https://` öffnen. Die App muss über die GitHub-Pages-Adresse geladen sein, nicht aus einer lokalen Datei.

## Daten und Sicherung

- Alle Daten liegen **nur im Browser dieses Geräts** (`localStorage`). Es gibt keinen Server und keine Synchronisierung zwischen Geräten.
- Löscht man in Chrome die Website-Daten (oder entfernt die App samt Daten), sind die Trainings weg. Deshalb regelmäßig unter **Daten → Sicherung (JSON)** exportieren. Die Datei landet im Ordner „Downloads“.
- Wiederherstellen oder auf ein neues Handy übertragen: **Daten → Sicherung einspielen**.
- **Umzug aus dem Claude-Artefakt:** Dort unter „Daten“ die Sicherung (JSON) exportieren und in der GitHub-Version einspielen.
- Die Schriftarten (Barlow) werden von Google Fonts geladen und danach zwischengespeichert. Wer das nicht möchte, legt die Schriften in einen Ordner `fonts/` und passt den `<link>` in `index.html` an. Ohne Schriften greifen Ersatzschriften.

## Lokal entwickeln (z. B. Linux Mint)

```bash
git clone <repository-url>
cd satzbau
python3 -m http.server 8000
# im Browser: http://localhost:8000
```

Der Service Worker funktioniert nur über `http://localhost` oder `https://`, nicht über `file://`. Ohne Service Worker läuft die App trotzdem, nur eben nicht offline.

### Neues Repository aus diesen Dateien anlegen

```bash
cd satzbau
git init
git add .
git commit -m "Satzbau: erste Version"
git branch -M main
git remote add origin https://github.com/<benutzername>/<repository>.git
git push -u origin main
```

## Aufbau

| Datei | Zweck |
|---|---|
| `index.html` | die ganze App (HTML, CSS, JavaScript in einer Datei) |
| `manifest.webmanifest` | Installation auf Android (Name, Icons, Farben, Schnellzugriffe) |
| `sw.js` | Service Worker für den Offline-Betrieb |
| `icons/` | App-Symbole (PNG und SVG) |
| `.nojekyll` | sagt GitHub Pages, die Dateien unverändert auszuliefern |

## Updates

Der Service Worker holt eigene Dateien zuerst aus dem Netz und nutzt den Cache nur ohne Verbindung. Eine neue Version erscheint daher beim nächsten Öffnen mit Internet. Sieht die App nach einem Update veraltet aus, einmal schließen und neu öffnen. Ändert sich die Dateiliste (neue Icons o. ä.), in `sw.js` den Namen `CACHE` hochzählen.

## Hinweis zum Cloud-Speicher

`index.html` enthält zusätzlich Code für die Datenbank des Claude-Artefakts. Auf GitHub Pages ist dieser Teil ohne Wirkung, die Statusanzeige oben zeigt „Nur auf diesem Gerät“.
