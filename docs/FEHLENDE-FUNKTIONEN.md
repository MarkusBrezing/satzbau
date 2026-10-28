# Satzbau – Funktionsplanung

Stand: Version 1.5 (28.10.2026). Ergänzt `PROGRAMMSTRUKTUR.md`. Prioritäten: **A** = nötig für eine öffentliche Version, **B** = wichtig, **C** = später.

## Übersicht

| Nr. | Funktion | Stand | Prio |
|---|---|---|---|
| F1 | Eigene Geräte verwalten | ✅ umgesetzt in 1.3 | A |
| F2 | Pläne in der Monatsübersicht farbig | ✅ umgesetzt in 1.3 | B |
| F3 | Trainingstage einstellbar | ✅ umgesetzt in 1.3 | B |
| F4 | Mehrere Trainings an einem Tag im Kalender | ✅ umgesetzt in 1.3 | B |
| F5 | Abgeschlossenes Training bearbeiten | ✅ umgesetzt in 1.4 | B |
| F6 | Datenbank im Browser (IndexedDB) | ✅ umgesetzt in 1.4 | A |
| F7 | Abgleich über eigenes Google Drive (Variante B) | offen – geplant für 1.6 | B |
| F8 | Datenschutzerklärung, Schriften lokal | ✅ umgesetzt in 1.4 | A |
| F9 | Englische Oberfläche | offen | C |
| F10 | Play-Store-Version (Trusted Web Activity) | offen | C |
| F11 | Trainingsverlauf mit Phasen (Aufbau, Erholung) | ✅ umgesetzt in 1.5 (ohne Kraftphase) | B |
| F12 | Kraftphase im Trainingsverlauf | offen | C |
| F13 | Studio-Profil (Katalog je Studiokette) | offen | C |
| F14 | Aufräumen: Cloud-Code des Claude-Artefakts entfernen | offen | C |
| R1 | Rechtliches vor breiter Veröffentlichung prüfen (Impressum, Postanschrift, Datenschutzerklärung) | offen | A |

Was die umgesetzten Funktionen im Einzelnen tun, steht in `CHANGELOG.md`. Wo sie im Code stehen, steht in `PROGRAMMSTRUKTUR.md`.

---

## Umgesetzt – Abweichungen vom ursprünglichen Plan

| Nr. | Plan | Umsetzung |
|---|---|---|
| F1 | Pause im Formular „Neues Gerät“ | Pause wird in der Gerätedetailansicht eingestellt; das Formular bleibt kurz |
| F1 | „Nur meine Geräte“ | als Ausblenden einzelner Geräte plus Schalter „Ausgeblendete zeigen“ |
| F2 | Farb-Token `--plan-1` … `--plan-6` | Token heißen `--pl-1` … `--pl-6`, `--pl-0` = Grau für Trainings ohne Plan; zusätzlich frei wählbares Kürzel je Plan |
| F3 | Auswahl unter Daten oder Übersicht | eigene Ansicht „Trainingsplanung“ (Übersicht → „Trainingstage und Trainingsverlauf“, auch unter Daten erreichbar) |
| F6 | localStorage-Kopie bleibt unverändert als Rückfall | localStorage wird zusätzlich laufend als Spiegelkopie geschrieben; der Stand vor dem Umzug bleibt unter `kraftbuch.v1.vor-1.4` |
| F6 | Sicherungserinnerung nach 14 Tagen | Erinnerung ab 3 Trainings ohne Sicherung bzw. 14 Tage nach der letzten, mit „Später“ (3 Tage Ruhe) |
| F11 | `verschiebung:[…]` im Zyklus | Anker `anker:[{ab, phase}]`: ab dem Datum beginnt die genannte Phase |
| F11 | „Erholungswoche einschieben“ verschiebt den Zyklus | die nächste Erholungsphase beginnt sofort, danach läuft der Zyklus normal weiter |
| F11 | Kraftphase (optional) | nicht umgesetzt, siehe F12 |
| F11 | Hinweis „Ampel zweimal rot“ | ab zwei roten Kernübungen, mit „Später“ (7 Tage Ruhe) |

---

## F7 Abgleich über eigenes Google Drive

Konzept: `~/satzbau-daten/Satzbau_Variante_B.docx`. Die App bleibt lokal (IndexedDB). Wer will, verbindet sein Google-Konto; beim Beenden eines Trainings wird die Datei `satzbau.json` im versteckten App-Ordner (`appDataFolder`) des eigenen Drive abgeglichen.

| Schritt | Wer |
|---|---|
| Google-Cloud-Projekt, Drive API, OAuth-Zustimmungsbildschirm (Bereich `drive.appdata`), Client-ID für `https://markusbrezing.github.io` | Markus |
| Drive-Verbindung, Zusammenführen nach ID und Änderungszeit, Lösch-Vermerke, „Jetzt abgleichen“, Trennen/Löschen | Code |
| Datenschutzerklärung um Google-Anmeldung und Drive ergänzen | Code + Prüfung |
| Google-Prüfung des Zustimmungsbildschirms beantragen | Markus |

Vorbereitet in 1.4/1.5: bearbeitete Trainings tragen `edited` (Änderungszeit), der Zustand hat Lösch-Vermerke (`deleted`), die Sicherung enthält `appVersion`.

## F9 Englische Oberfläche
Alle Texte stehen heute direkt in den Ansichtsfunktionen. Nötig: Texttabelle `T.de` / `T.en`, Sprachwahl unter Daten, Zahlen- und Datumsformate über `Intl` nach Sprache.

## F10 Play-Store-Version
Trusted Web Activity mit Bubblewrap. Voraussetzungen: eigene Domain oder Nutzer-Repo `MarkusBrezing.github.io` für `/.well-known/assetlinks.json`, Play-Konto (25 $), geschlossener Test mit 12 Testern über 14 Tage.

## F12 Kraftphase
Phasentyp „Kraft“: eigener Wiederholungsbereich (z. B. 4–6), Startgewicht = letztes Aufbau-Topgewicht + 1 Schritt, Progression an. Offen ist, wie die doppelte Progression zwischen verschiedenen Wiederholungsbereichen weiterrechnet.

## F13 Studio-Profil
Katalog je Studiokette (McFit, Clever Fit, FitX …) statt fest McFit; Auswahl beim ersten Start. Bis dahin: nicht vorhandene Geräte ausblenden, fehlende als eigenes Gerät anlegen.

## F14 Cloud-Code entfernen
`initCloud`, `pull`, `push`, `COL` und die Texte dazu sind nur im Claude-Artefakt wirksam. Auf GitHub Pages ohne Funktion. Entfernen spart etwa 45 Zeilen, setzt aber voraus, dass das Artefakt nicht mehr genutzt wird.

## R1 Rechtliches
Die Datenschutzerklärung (`datenschutz.html`) ist ein Entwurf. Vor breiter Veröffentlichung prüfen: Postanschrift des Verantwortlichen, Impressumspflicht, Formulierungen zu GitHub Pages. Bei F7 muss die Erklärung vor dem Start erweitert werden.

---

## Reihenfolge

| Version | Inhalt | Stand |
|---|---|---|
| 1.3 | F1 eigene Geräte, F2 Plan-Farben, F3 Trainingstage, F4 mehrere Trainings/Tag | ✅ |
| 1.4 | F5 Training bearbeiten, F6 IndexedDB, F8 Datenschutz/Schriften | ✅ |
| 1.5 | F11 Trainingsphasen (Aufbau/Erholung) | ✅ |
| 1.6 | F7 Drive-Abgleich | offen |
| später | F9 Englisch, F10 Play Store, F12 Kraftphase, F13 Studio-Profil, F14 Aufräumen | offen |
