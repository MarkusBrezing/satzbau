# Satzbau

Trainingstagebuch fürs Fitnessstudio (Geräte nach McFit-Ausstattung, eigene Geräte ergänzbar). Läuft als installierbare Web-App (PWA) auf Android: eigenes Symbol, eigenes Fenster ohne Adressleiste, auch ohne Netz nutzbar. Ohne Konto, ohne Tracking, alle Daten bleiben auf dem Gerät.

App: https://markusbrezing.github.io/satzbau/ · Version 1.5.1 · [Änderungen](CHANGELOG.md) · [Datenschutz](datenschutz.html)

## Funktionen

- Trainingspläne (Plan A/B) mit Startvorschlag fürs Gewicht (doppelte Progression), Plan-Farbe und Kürzel
- Trainingsverlauf mit Aufbau- und Erholungsphasen (Vorlagen 3 + 1, 4 + 1, 6 + 1 oder eigene Einteilung), Erholung einschieben oder Phase überspringen
- Satzeingabe mit +/−-Knöpfen, Pausen-Countdown (je Übungsart oder je Gerät einstellbar)
- Bildschirm bleibt im laufenden Training an
- Ampel je Übung: Auf Kurs, Stagniert, Hinter Plan
- Verlaufsdiagramme je Übung (1RM geschätzt, Topgewicht, Volumen)
- Körpergewicht mit Verlauf
- Übersicht mit drei Kernübungen, Wochen- und Monatsansicht (Plan-Farben, mehrere Trainings pro Tag, Erholungsphasen schraffiert), einstellbare Trainingstage
- Eigene Geräte anlegen, bearbeiten, ausblenden und löschen; Stammdaten je Gerät: Name, Muskelgruppe, Geräte-Nr., Sitzeinstellung, Schrittweite, Wiederholungsbereich, Pause, Notiz, Kernübung
- Abgeschlossene Trainings nachträglich bearbeiten (Sätze, Datum, Dauer, Übungen)
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

- Alle Daten liegen **nur im Browser dieses Geräts**: in der Browser-Datenbank IndexedDB („satzbau“), zusätzlich als Spiegelkopie in `localStorage` („kraftbuch.v1“). Ohne IndexedDB arbeitet die App nur mit `localStorage`. Es gibt keinen Server und keine Synchronisierung zwischen Geräten.
- Beim ersten Start von Version 1.4 oder neuer werden vorhandene Daten aus `localStorage` automatisch in die Datenbank übernommen; der alte Stand bleibt unter „kraftbuch.v1.vor-1.4“ erhalten.
- Unter **Daten** zeigt die App Speicherart, dauerhaften Speicher, belegten Platz und die letzte Sicherung. Fehlt eine Sicherung, erinnert der Startbildschirm daran.
- Löscht man in Chrome die Website-Daten (oder entfernt die App samt Daten), sind die Trainings weg. Deshalb regelmäßig unter **Daten → Sicherung (JSON)** exportieren. Die Datei landet im Ordner „Downloads“.
- Wiederherstellen oder auf ein neues Handy übertragen: **Daten → Sicherung einspielen**.
- **Umzug aus dem Claude-Artefakt:** Dort unter „Daten“ die Sicherung (JSON) exportieren und in der GitHub-Version einspielen.
- Die Schriftarten Barlow und Barlow Condensed liegen im Ordner `fonts/` (SIL Open Font License, siehe `fonts/OFL.txt`). Die App lädt nichts von fremden Servern.
- **Alle Daten löschen:** Daten → „Alle Daten auf diesem Gerät löschen“ (zweifache Rückfrage).

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
| `datenschutz.html` | Datenschutzerklärung |
| `fonts/` | Schriften (WOFF2) und Lizenz `OFL.txt` |
| `manifest.webmanifest` | Installation auf Android (Name, Icons, Farben, Schnellzugriffe) |
| `sw.js` | Service Worker für den Offline-Betrieb |
| `icons/` | App-Symbole (PNG und SVG) |
| `.nojekyll` | sagt GitHub Pages, die Dateien unverändert auszuliefern |
| `docs/PROGRAMMSTRUKTUR.md` | Aufbau des Codes, Datenmodell, Abläufe, Erweiterungspunkte |
| `docs/FEHLENDE-FUNKTIONEN.md` | offene Funktionen mit Ist-Stand, Soll und Umsetzungsidee |

## Updates

Der Service Worker holt eigene Dateien zuerst aus dem Netz und nutzt den Cache nur ohne Verbindung. Eine neue Version erscheint daher beim nächsten Öffnen mit Internet. Sieht die App nach einem Update veraltet aus, einmal schließen und neu öffnen. Ändert sich die Dateiliste (neue Icons, Schriften o. ä.), in `sw.js` die Liste `SHELL` anpassen und den Namen `CACHE` hochzählen (derzeit `satzbau-v5`).

## Hinweis zum Cloud-Speicher

`index.html` enthält zusätzlich Code für die Datenbank des Claude-Artefakts. Auf GitHub Pages ist dieser Teil ohne Wirkung, die Statusanzeige oben zeigt „Nur auf diesem Gerät“.
