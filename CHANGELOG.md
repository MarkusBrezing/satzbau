# Änderungen

## 1.5.1 – Reihenfolge (28.10.2026)
- Reihenfolge der Pläne einstellbar: „Reihenfolge ändern“ über der Liste „Pläne“, dann ↑ / ↓; „Als Nächstes“ folgt dieser Reihenfolge
- Laufendes Training: Übung im Menü ⋯ „Nach oben“ und neu „Nach unten“
- Plan bearbeiten: ↑ beim ersten und ↓ beim letzten Eintrag gesperrt
- Service Worker `satzbau-v5`

Die Versionen 1.3 bis 1.5 erscheinen gemeinsam (28.10.2026).

## 1.5 – Trainingsphasen (F11)
- Trainingsverlauf mit Aufbau- und Erholungsphasen, gemeinsam für alle Pläne, nach Kalenderwochen
- Vorlagen „Ohne Phasen“, „3 + 1“, „4 + 1“, „6 + 1“; Phasen frei einstellbar (Name, Art, Wochen, Gewicht % und Sätze % der Erholung), Zyklusbeginn, Wiederholen
- Erholung: Gewicht 70 % des letzten Aufbau-Topgewichts (auf die Schrittweite gerundet), 50 % der Sätze, keine Steigerung; Ampel und Bestwerte setzen aus
- Nach der Erholung setzt der Vorschlag am letzten Aufbautraining an
- „Erholungswoche jetzt einschieben“, „Phase überspringen“, „Verschiebungen zurücksetzen“
- Hinweis bei mindestens zwei roten Kernübungen (Erholung einschieben bzw. Trainingsplanung öffnen), mit „Später“
- Phase wird am Training gespeichert; Anzeige auf Startbildschirm, im Training, in Übersicht (Phasenkasten, Wochenkopf, Schraffur im Kalender), Verlauf und Gerätedetail
- Diagramme: Erholungstrainings als hohle Punkte, nicht in der Linie, Schalter „Erholung zeigen“
- Plausibilität rechnet in der Erholung mit dem Erholungsziel

## 1.4 – Training bearbeiten, Browser-Datenbank, Datenschutz (F5, F6, F8)
- Abgeschlossene Trainings bearbeiten: Name, Datum, Beginn, Dauer, Notizen, Sätze (Gewicht, Wiederholungen, löschen, ergänzen), Übung ergänzen, entfernen, Gerät tauschen; Prüfung vor dem Speichern; Bestwerte werden neu berechnet
- Speicher: Browser-Datenbank IndexedDB statt localStorage; automatischer Umzug beim ersten Start (alter Stand bleibt unter „kraftbuch.v1.vor-1.4“), Spiegelkopie in localStorage, Rückfall auf localStorage ohne IndexedDB
- Daten: Speicherart, dauerhafter Speicher, belegter Platz, letzte Sicherung; „Dauerhaft anfordern“; „Alle Daten auf diesem Gerät löschen“
- Sicherungserinnerung auf dem Startbildschirm (ab 3 Trainings ohne Sicherung bzw. 14 Tage nach der letzten), mit „Später“
- Datenschutzerklärung (`datenschutz.html`), verlinkt unter Daten
- Schriften Barlow und Barlow Condensed lokal im Ordner `fonts/` statt von Google Fonts; keine Verbindung zu fremden Servern mehr
- Service Worker `satzbau-v4`: Datenschutzseite und Schriften offline verfügbar
- Sicherung (JSON) enthält die App-Version

## 1.3 – Geräte, Plan-Farben, Trainingstage (F1–F4)
- „+ Neues Gerät“ im Reiter Übungen und in der Übungsauswahl: Name, Muskelgruppe, Art, Geräte-Nr., Einstellung, Schrittweite, Wiederholungsbereich, Notiz; eindeutige IDs, Rückfrage bei doppeltem Namen
- Gerätedetail: Name (bei Katalog-Geräten leer = Katalogname), Muskelgruppe und Notiz änderbar
- Geräte ausblenden und wieder einblenden; eigene Geräte ohne Trainings löschen (werden aus Plänen entfernt)
- Plan-Farben (6 Farben, im Plan-Editor wählbar) und Kürzel; Monatskalender mit Plan-Farbe, Kürzel und Legende mit Anzahl; freie Trainings grau
- Mehrere Trainings an einem Tag: geteilte Kalenderzelle, Auswahl beim Antippen
- Plan-Farbe auch in Woche, Verlauf, Planliste und „Als Nächstes“
- Trainingstage einstellbar (vorher fest Mo/Mi/Fr)

## 1.2 – Plausibilitätsprüfung
- Gewicht: hart 0–500 kg (abgelehnt), weich gegen Referenz (Satz davor bzw. Median der letzten 3 Trainings): unter 50 % oder über 150 % bzw. +4 Schritte → Rückfrage „Korrigieren / Stimmt so“
- Ohne Verlauf: Rückfrage ab 250 kg (Maschine), 120 kg (Kabelzug), 150 kg (Freihantel)
- Wiederholungen: hart 0–100, Rückfrage ab 31
- Hinweis bei vermutlichem Weitertippen („11“ vor „127,5“)
- Prüfung beim Verlassen des Feldes, beim Abhaken und vor dem Speichern des Trainings
- Antippen eines Zahlenfelds markiert den Inhalt, neue Eingabe ersetzt den alten Wert
- Unmögliche Altwerte (> 500 kg) zählen nicht für Vorschlag, Ampel, Diagramme und Bestwerte; im Verlauf mit ⚠ markiert; Hinweis beim Import
- Bestwerte nur aus plausiblen oder bestätigten Sätzen
- Zahlen: Tausenderpunkt („1.127,5“) und Text-Reste („12abc“) werden richtig behandelt
- Leere Pläne werden beim Vorschlag „Als Nächstes“ übersprungen

## 1.1 – Android
- Bildschirm bleibt im laufenden Training an (Wake Lock)
- Kein Mitziehen am Seitenrand, kein versehentliches Textmarkieren, kein Doppeltipp-Zoom
- Manifest mit Schnellzugriffen (Training, Übersicht, Verlauf) und Kategorien
- iPhone-spezifische Teile entfernt

## 1.0 – erste Version für GitHub Pages
- Stand aus dem Claude-Artefakt „Satzbau“ übernommen
- Installierbar (Manifest, Icons) und offlinefähig (Service Worker)
- Anfrage auf dauerhaften Browser-Speicher
