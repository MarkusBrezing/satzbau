# Satzbau – Programmstruktur

Stand: Version 1.5 (28.10.2026). Zeilenangaben beziehen sich auf `index.html` in dieser Version.

## 1. Überblick

Satzbau ist eine Progressive Web App (PWA) ohne Build-Schritt und ohne Fremdbibliotheken. Die gesamte Anwendung (HTML, CSS, JavaScript) steht in **einer Datei** `index.html`. Daten liegen im Browser des Geräts, seit 1.4 in der Browser-Datenbank IndexedDB. Ausgeliefert wird über GitHub Pages; die App lädt nichts von fremden Servern.

```
Browser (Chrome/Android)
 ├─ index.html ........ Oberfläche + Logik (Vanilla JS, ein globaler Zustand S)
 ├─ datenschutz.html .. Datenschutzerklärung
 ├─ fonts/ ............ Barlow, Barlow Condensed (WOFF2, OFL)
 ├─ manifest.webmanifest  Installation als App
 ├─ sw.js ............. Service Worker: offline, Netz zuerst
 ├─ IndexedDB "satzbau" Hauptspeicher: meta, workouts, zustand
 └─ localStorage ...... "kraftbuch.v1" = Spiegelkopie bzw. Rückfall ohne IndexedDB
```

## 2. Dateien und Ordner

### 2.1 Repository `~/satzbau` (öffentlich, GitHub `MarkusBrezing/satzbau`)

| Datei | Größe | Zweck |
|---|---|---|
| `index.html` | ca. 143 KB, 1301 Zeilen | komplette App |
| `datenschutz.html` | 6 KB | Datenschutzerklärung (Verantwortlicher, lokale Speicherung, GitHub Pages, Rechte) |
| `fonts/` | 6 × ca. 22 KB + `OFL.txt` | Barlow 400/500/600, Barlow Condensed 500/600/700, Teilmenge latin |
| `manifest.webmanifest` | 1,3 KB | Name, Farben, Icons, Schnellzugriffe (Training, Übersicht, Verlauf) |
| `sw.js` | 1,5 KB | Service Worker, Cache `satzbau-v4` |
| `icons/` | 4 Dateien | `favicon.svg`, `icon-192.png`, `icon-512.png`, `icon-maskable-512.png` |
| `README.md` | | Funktionen, Veröffentlichung, Installation, Datenhinweise |
| `CHANGELOG.md` | | Versionen 1.0–1.5 |
| `docs/PROGRAMMSTRUKTUR.md` | | dieses Dokument |
| `docs/FEHLENDE-FUNKTIONEN.md` | | Funktionsplanung: umgesetzt, offen, Abweichungen |
| `.nojekyll` | leer | GitHub Pages liefert Dateien unverändert aus |
| `.gitignore` | | Systemdateien, `node_modules/` |

### 2.2 Außerhalb des Repos (privat, nur Mint-VM)

| Pfad | Inhalt |
|---|---|
| `~/satzbau-daten/` | JSON-Sicherungen, Konzept `Satzbau_Variante_B.docx`, Commit-Nachricht für den nächsten Push – **nicht** nach GitHub |
| `~/bin/satzbau-veroeffentlichen.sh` | Commit + Push von `~/satzbau` |
| `~/bin/satzbau-git-einrichten.sh` | einmaliges Einrichten von Git (erledigt) |
| `~/Schreibtisch/Satzbau-auf-GitHub.desktop` | Starter „Satzbau auf GitHub veröffentlichen“ (ruft `satzbau-veroeffentlichen.sh` auf) |

## 3. Aufbau von `index.html`

| Zeilen | Bereich | Inhalt |
|---|---|---|
| 1–11 | Kopf | Meta-Tags, Manifest, Favicon |
| 12–248 | CSS | `@font-face` (lokale Schriften), Farb-Token hell/dunkel inkl. Plan-Farben `--pl-0…6` und Schraffur `--hatch`, Layout, Startbildschirm, Training, Ampel, Übersicht, Pausentimer, Navigation, Verlauf, Übungen, Dialog; ab Zeile 219 Klassen für Plan-Kürzel, Legende, Farbwahl, Phasen, Zyklus-Leiste, Hinweise |
| 250–272 | HTML-Gerüst | Kopfzeile mit Status `#sync`, Inhalt `#view`, Pausenleiste `#rest`, Tabs, Dialog `#sheet`, Toast `#toast`, Datei-Feld `#file` |
| 273–317 | Script-Beginn, Gerätekatalog | `CATALOG` (33 Geräte), `GROUPS`, `PLATE` (Farbe je Muskelgruppe), `TYP`, `DEF_STEP`, `CAT` |
| 318–335 | Hilfen | `$`, `esc`, `clone`, `fmt`, `parseNum`, `norm`, `newId`, `e1rm`, `dur`, `clock` |
| 337–410 | **Zustand und Speicher** | `APP_VERSION`, `S`, `normZyklus`, `normMeta`, `applyState`, `stateJSON`, `mergeStates`; IndexedDB `idbOpen`, `idbWrite`; `lsWrite`, `saveLocal(Now)`, `loadState` (Umzug, Rückfall), `wipeAll` |
| 411–425 | Geräte-Stammdaten | `getEx`, `allExIds`, `visibleExIds`, `exUsage`, `setExMeta`, `metaChanged`, `finished` |
| 426–439 | Verlauf je Übung | `isErh`, `sessionsFor` (optional ohne Erholung), `lastSession`, `roundStep`, `suggest` (phasenabhängig), `bests` |
| 440–470 | Plausibilität | `PLAUS`, `refKg` (phasenabhängig), `plausRange`, `checkSet`, `ctxOf`, `askPlaus`, `badSets`, `nextTemplate` |
| 471–503 | Körpergewicht | `bodyList`, `setBody`, `delBody`, `bodyTrend`, `bodyChart`, `vBody` |
| 504–520 | Progressionsampel | `AMPEL`, `ampel` (ohne Erholung), `ampPill`, `planAmpel` |
| 521–564 | Cloud | `initCloud`, `pull`, `push`, `setSync` – nur im Claude-Artefakt wirksam |
| 565–652 | Aktionen | `startWorkout` (mit Phase), `newBlock`, `prevSet`, Pausentimer, `toast`, `openSheet`/`ask`, `pickExercise`, **`exForm`/`exCreate`** (neues Gerät), **`computePrs`/`recomputePrs`**, `finishWorkout` |
| 653–718 | Import/Export | Strong-CSV, `exportJSON` (mit `appVersion`), `exportCSV`, `importJSON` (inkl. Trainingstage und Zyklus), `saveFile` |
| 719–747 | Startbildschirm | `snoozed`, `backupHint`, `redHint`, `vTraining` |
| 748–775 | Laufendes Training | `vActive` (Phasenzeile, phasenabhängige Vorschläge) |
| 776–838 | Verlauf + Bearbeiten | `vVerlauf`; Entwurf `ED`, `editStart`, `vEditWorkout`, `editSave` |
| 839–898 | Übungen, Diagramme, Kern | `vUebungen` (Ausblenden), `exSeries`, `spark`, `chartEx` (Erholung als hohle Punkte), `getKern`, `miniChart`, `kernCard` |
| 899–919 | Plan-Farben | `WD`, `planTage`, `ensurePlanColors`, `planKurz`, `wColor`, `wKurz`, `HATCH` |
| 920–943 | **Trainingsphasen** | Datumshilfen `dParse`/`dDiff`/`dAdd`/`mondayOf`, `zyPhasen`, `zyVorlage`, **`phaseAm`**, `phaseSnap`, `nextPhase`, `phaseTag` |
| 944–991 | Woche/Monat | `rng`, `periodOf`, `sumW`, `vPeriod` (Plan-Farben, mehrere Trainings, Schraffur, Legende) |
| 992–1028 | Übersicht, Planung | `vUebersicht` (Phasenkasten), **`vPlanung`** (Trainingstage, Zyklus, 12-Wochen-Leiste, Phasen) |
| 1029–1064 | Gerät, Plan | `vExercise` (Name, Gruppe, Notiz, Ausblenden, Löschen), `vTemplate` (Farbe, Kürzel) |
| 1065–1092 | Daten | `fmtBytes`, `storeLabel`, `fillStorageInfo`, `vDaten` |
| 1093–1100 | **`render`** | wählt die Ansicht nach `view.tab`/`view.sub` |
| 1101–1267 | Ereignisse | Aktions-Tabelle `A` (bis Zeile 1219), Klick-, Eingabe-, Fokus-, Änderungs- und Datei-Handler |
| 1268–1274 | Wake Lock | Bildschirm bleibt im laufenden Training an |
| 1275–1298 | Start | `DEFAULT_META`, localStorage-Prüfung, asynchroner Start (`loadState`, Standardpläne, Plan-Farben, `render`, `initCloud`), Service Worker, `storage.persist()` |

## 4. Zustand und Datenmodell

Ein globales Objekt `S` hält alles. `saveLocal` speichert mit 300 ms Verzögerung, `saveLocalNow` sofort; beim Verbergen oder Schließen der Seite wird eine anstehende Speicherung sofort ausgeführt. Gespeichert wird erst, wenn der Start abgeschlossen ist (`booted`).

```
S = {
  meta: {                                // IndexedDB: Bereich "meta", Schlüssel "meta"
    kind: 'meta',
    templates: [ { id, name, farbe:1–6, kurz?, items: [ { ex, sets, repMin, repMax } ] } ],
    exercises: { <ex-id>: { name?, group?, typ?, geraetNr, einstellung, schritt,
                            repMin, repMax, pause, notiz?, hidden? } },
    restSec: 90, restTyp: { maschine|kabel|frei: Sekunden },
    kern: [ <ex-id> ×3 ], kernSet,
    body: [ { d: 'JJJJ-MM-TT', kg } ],
    planTage: [ 0–6 ],                   // Wochentage nach getDay(), Standard [1,3,5]
    zyklus: { aktiv, start:'JJJJ-MM-TT', wiederholen,
              phasen: [ { id, name, typ:'aufbau'|'erholung', wochen, gewichtFaktor, saetzeFaktor } ],
              anker:  [ { ab:'JJJJ-MM-TT', phase:<Index> } ] }
  },
  workouts: { <id>: Workout },           // IndexedDB: Bereich "workouts", Schlüssel id
  active:   Workout | null,              // ┐
  pending, deleted, metaDirty,           // │ IndexedDB: Bereich "zustand", Schlüssel "zustand"
  lastBackup: Zeitstempel | null,        // │
  snooze: { backup?, rot? }              // ┘ „Später“ bei Hinweisen
}

Workout = { id, kind:'workout', name, templateId, start, end, notes, prs:[…], source?, edited?,
            phase?: { typ, name, woche, wochen, gewichtFaktor, saetzeFaktor },
            exercises: [ { ex, repMin, repMax, notes,
                           sets: [ { kg, reps, done, warm?, ok?, okR? } ] } ] }
```

| Feld | Bedeutung |
|---|---|
| `farbe`, `kurz` | Plan-Farbe (Token `--pl-<farbe>`) und Kürzel im Kalender; fehlende Farben vergibt `ensurePlanColors` |
| `hidden` | Gerät ausgeblendet (nicht in Liste und Auswahl; Pläne und Verlauf bleiben) |
| `phase` | beim Start des Trainings festgehaltene Phase; Auswertungen bleiben richtig, auch wenn der Zyklus später geändert wird |
| `edited` | Zeitpunkt der letzten nachträglichen Bearbeitung |
| `ok` / `okR` | ungewöhnliches Gewicht bzw. ungewöhnliche Wiederholungen vom Nutzer bestätigt |

Die JSON-Sicherung (`exportJSON`) enthält `{ app:'satzbau', version:1, appVersion, exportedAt, meta, workouts:[…] }`.

## 5. Speicher (`loadState`, `idbWrite`)

| Fall beim Start | Verhalten |
|---|---|
| IndexedDB hat Daten | laden; ist die localStorage-Kopie neuer (Rückfall-Betrieb), werden beide zusammengeführt |
| IndexedDB leer, localStorage hat Daten | **Umzug**: übernehmen, alten Stand unter `kraftbuch.v1.vor-1.4` sichern, Meldung anzeigen |
| beides leer | Standardpläne A/B |
| IndexedDB nicht verfügbar | Rückfall auf localStorage (`store='ls'`); funktioniert auch das nicht, Warnbanner (`store='none'`) |

`idbWrite` schreibt Meta und Zustand in einer Transaktion und nur die geänderten Trainings (Vergleich mit dem zuletzt geschriebenen Stand `savedW`); gelöschte Trainings werden entfernt. Läuft schon ein Schreibvorgang, folgt ein weiterer direkt danach. Die Spiegelkopie in localStorage wird bei jeder Speicherung mitgeschrieben, bis sie zu groß wird (`mirrorOk=false`).

## 6. Programmablauf

### 6.1 Start
1. localStorage prüfen (`lsOk`).
2. `await loadState()` – siehe Abschnitt 5.
3. Standardpläne setzen, falls leer; `ensurePlanColors()`; `booted=true`; ggf. speichern.
4. Tab aus der Adresse (`#training`, `#uebersicht`, …), `render()`, Umzugsmeldung, `initCloud()`.
5. Service Worker registrieren, dauerhaften Speicher anfordern.

### 6.2 Darstellung
`render()` wählt nach `view.tab` und `view.sub` eine Ansichtsfunktion `v…()`, die einen HTML-String liefert. Kein Framework, kein virtuelles DOM. Bei Eingaben wird gezielt **nicht** neu gerendert, damit der Fokus erhalten bleibt.

| `view.tab` | Ansicht | Unteransichten (`view.sub`) |
|---|---|---|
| `training` | `vTraining` / `vActive` | `t:<plan-id>` → `vTemplate` |
| `uebersicht` | `vUebersicht` | `planung` → `vPlanung`; Woche/Monat über `rng` |
| `verlauf` | `vVerlauf` | `w:<id>` aufgeklapptes Training, `we:<id>` → `vEditWorkout` |
| `uebungen` | `vUebungen` | `ex:<ex-id>` → `vExercise` |
| `daten` | `vDaten` | – |

### 6.3 Ereignisse
Alle Schaltflächen tragen `data-a="<aktion>"`; ein Klick-Handler ruft die Funktion aus der Tabelle `A` auf.

| Gruppe | Aktionen |
|---|---|
| Navigation | `tab`, `back`, `w-goto`, `ex-open`, `w-open`, `metric`, `chart-erh`, `rng-mode`, `rng-nav`, `day-open`, `planung` |
| Training | `start`, `start-empty`, `tick`, `kg-step`, `rep-step`, `set-add`, `set-del`, `finish`, `discard` |
| Übung im Training | `blk-add`, `blk-menu`, `blk-notes`, `blk-swap`, `blk-up`, `blk-rm`, `pick` |
| Geräte | `ex-new`, `ex-create`, `ex-hide`, `ex-del`, `ex-showhidden`, `gear`, `gear-save`, `kern-toggle` |
| Training bearbeiten | `w-edit`, `we-save`, `we-cancel`, `we-set-add`, `we-set-del`, `we-blk-add`, `we-blk-del`, `we-swap` |
| Pläne | `tpl-new`, `tpl-edit`, `tpl-del`, `tpl-farbe`, `ti-add`, `ti-up`, `ti-down`, `ti-del` |
| Planung | `tag-toggle`, `zy-vorlage`, `zy-einschub`, `zy-skip`, `zy-reset`, `zy-add`, `zy-del`, `zy-up`, `zy-down` |
| Pause | `rest-add`, `rest-stop` |
| Körpergewicht | `body-quick`, `body-save`, `body-del` |
| Daten | `file`, `exp-json`, `exp-csv`, `copy`, `w-del`, `persist`, `wipe`, `snooze` |
| Dialoge | `ask`, `close` |

| Ereignis | Zweck |
|---|---|
| `input` | Felder sofort übernehmen: `data-f` (Training), `data-ef` (Bearbeiten-Entwurf), `data-xf` (Gerät), `data-tf`/`data-tif` (Plan) |
| `focusin` | Zahlenfeld: alten Wert merken (`data-prev`), Inhalt markieren |
| `change` | Plausibilitätsprüfung im Training; Zyklus-Felder `data-zf`/`data-zp`; Pausen `data-mt` |
| `change` auf `#file` | JSON- oder Strong-Import |
| `visibilitychange`, `pagehide` | Wake Lock; anstehende Speicherung sofort ausführen |

### 6.4 Training
```
startWorkout(plan) → Phase heute (phaseAm) → S.active mit w.phase
  newBlock: Satzzahl (in der Erholung × saetzeFaktor), Gewicht aus suggest(…, phase)
  ├─ Eingabe / ± / Abhaken (tick → checkSet(…, ctxOf(w)) → ggf. askPlaus → done → Pause)
  └─ Beenden (finishWorkout)
       ├─ checkSet für alle abgehakten Sätze
       ├─ nur abgehakte Sätze behalten, end setzen
       ├─ computePrs (keine Bestwerte in der Erholung)
       └─ speichern, Zusammenfassung
```

### 6.5 Training bearbeiten
`editStart` kopiert das Training in den Entwurf `ED` (mit Rohtext je Feld). `editSave` prüft Datum, Dauer, Zahlen und Plausibilität (Referenz: Trainings **vor** diesem Training), übernimmt Sätze, aktualisiert ggf. die Phase (bei geändertem Datum) und berechnet mit `recomputePrs` die Bestwerte aller Trainings neu.

## 7. Fachlogik

### 7.1 Gewichtsvorschlag – `suggest(ex, repMax, phase)`
Grundlage ist das Topgewicht des letzten **Aufbau**-Trainings der Übung (`lastSession` überspringt Erholungstrainings). Erreichen alle Sätze mit diesem Gewicht `repMax`, folgt Topgewicht + `schritt`, sonst gleiches Gewicht. In der Erholung: Topgewicht × `gewichtFaktor`, gerundet auf `schritt`.

### 7.2 Ampel – `ampel(ex)`
Letzte bis zu 6 Aufbau-Trainings, geschätztes 1RM (Epley, Wdh. ≤ 15) und Topgewicht. Grün: besser (mehr Gewicht oder 1RM ≥ +1 %); Gelb: einmal kein Fortschritt; Rot: 1RM ≤ −5 % oder 2× in Folge kein Fortschritt.

### 7.3 Plausibilität – `checkSet(b, si, raw, prevRaw, ctx)`

| Regel | Wert (`PLAUS`) | Ergebnis |
|---|---|---|
| Gewicht unmöglich | < 0 oder > 500 kg | abgelehnt |
| Referenz | Satz davor im Training, sonst Median Topgewicht der letzten 3 Aufbau-Trainings vor `ctx.before`; in der Erholung × `gewichtFaktor` („Erholungsziel“) | – |
| Gewicht ungewöhnlich | < 50 % oder > max(150 %, Referenz + 4 Schritte) | Rückfrage |
| Ohne Referenz | > 250 kg Maschine / 120 kg Kabel / 150 kg Freihantel | Rückfrage |
| Wiederholungen | > 100 unmöglich, > 30 ungewöhnlich | abgelehnt / Rückfrage |
| Weitertippen | neue Eingabe enthält den alten Wert | Zusatzhinweis |

### 7.4 Trainingsphasen – `phaseAm(datum)`
Zykluslänge L = Summe der Phasenwochen × 7 Tage. Maßgeblich ist der letzte Anker mit `ab ≤ datum` (sonst Zyklusbeginn mit Phase 0). Position = Tage vor der Ankerphase + Tage seit dem Anker; bei `wiederholen` modulo L, sonst endet der Zyklus. Ergebnis: Phase, Woche in der Phase, Beginn und Ende (ein späterer Anker kürzt das Ende).

| Aktion | Wirkung |
|---|---|
| Vorlage „n + 1“ | n Wochen Aufbau, 1 Woche Erholung (70 %, 50 %); Beginn Montag der laufenden Woche, wenn vorher aus |
| Erholungswoche einschieben | Anker heute → nächste Erholungsphase |
| Phase überspringen | Anker heute → folgende Phase |
| Verschiebungen zurücksetzen | alle Anker löschen |

### 7.5 Plan-Farben – `ensurePlanColors`, `wColor`, `wKurz`
Pläne ohne gültige Farbe bekommen die erste freie Farbe 1–6. Trainings ohne Plan oder mit gelöschtem Plan erscheinen grau (`--pl-0`). Kürzel: eigenes Kürzel, sonst letztes Wort bis 2 Zeichen („Plan A“ → „A“), sonst Anfangsbuchstaben.

### 7.6 Import/Export

| Funktion | Format |
|---|---|
| `importStrong` | Strong-CSV: Trennzeichen-Erkennung, lb → kg, Zuordnung über `mapExercise` (auch ausgeblendete Geräte) |
| `importJSON` | Satzbau-/Kraftbuch-Sicherung; Pläne, Stammdaten, Trainingstage und Zyklus werden übernommen, Trainings nach ID ergänzt |
| `exportJSON` | vollständige Sicherung mit `appVersion`; setzt `lastBackup` |
| `exportCSV` | `Datum;Uhrzeit;Training;Übung;Satz;kg;Wdh;Dauer_min`, UTF-8 mit BOM |

## 8. Service Worker (`sw.js`)

| Anfrage | Strategie |
|---|---|
| eigene Dateien | Netz zuerst, Kopie in Cache `satzbau-v4`; offline aus dem Cache, Navigation fällt auf `index.html` zurück |
| fremde Server | werden nicht angefragt und nicht abgefangen |

Beim Installieren werden `SHELL` (App, Datenschutzseite, Manifest, Icons, Schriften) vorab geladen. Ändert sich die Dateiliste, `SHELL` anpassen und `CACHE` hochzählen.

## 9. Entwicklung und Veröffentlichung

```bash
cd ~/satzbau
python3 -m http.server 8000          # Test: http://localhost:8000
bash ~/bin/satzbau-veroeffentlichen.sh   # Commit + Push (oder Desktop-Starter)
```

`satzbau-veroeffentlichen.sh` nimmt die Commit-Nachricht aus `~/satzbau-daten/commit-nachricht.txt` (danach umbenannt in `….erledigt-<Datum>`), sonst aus dem Aufrufargument, sonst wird sie abgefragt.

| Ziel | Adresse |
|---|---|
| App | https://markusbrezing.github.io/satzbau/ |
| Repository | https://github.com/MarkusBrezing/satzbau |
| Pages-Einstellungen | Branch `main`, Ordner `/ (root)` |

## 10. Erweitern – wo ändern?

| Vorhaben | Stelle |
|---|---|
| Neues Gerät im Katalog | `CATALOG`: `['id','Name','Gruppe',Art,['alias',…]]`, Art = `'maschine'`, `'kabel'` oder `'frei'` |
| Neue Schaltfläche | Element mit `data-a="name"`, Funktion `name:el=>{…}` in `A` |
| Neue Ansicht | Funktion `vName()` → HTML-String, Zweig in `render()` oder Unteransicht über `view.sub` |
| Grenzwerte Plausibilität | `PLAUS` |
| Ampel-Schwellen | `AMPEL` |
| Plan-Farben | Token `--pl-1…6` (hell und dunkel), `PLAN_FARBEN` |
| Phasentypen | `normZyklus`, `phaseSnap`, `suggest`, `newBlock`, `vPlanung` |
| Startpläne neuer Installationen | `DEFAULT_META` |
| Speicherformat ändern | `normMeta`/`normZyklus`, `applyState`, `stateJSON`, `idbWrite`, `exportJSON`/`importJSON` – Abwärtskompatibilität beachten |

## 11. Tests

Die Funktionen von 1.3–1.5 wurden im Browser (Chromium, 380 px breit, hell und dunkel) geprüft: Umzug und Rückfall des Speichers, Speichern bei sofortigem Neuladen, Geräte anlegen/bearbeiten/ausblenden/löschen, Plan-Farben und Kalender, Trainingstage, Training bearbeiten mit Neuberechnung der Bestwerte, Trainingsphasen mit festgelegtem Datum, Offline-Betrieb, Datenschutzseite, keine Anfragen an fremde Server. Offene Punkte stehen in `FEHLENDE-FUNKTIONEN.md`.
