# Änderungen

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
