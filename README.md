# Flaggenquiz-2
Ein Flaggenquiz für Kinder: **Flaggenjagd**.

Eine Frage pro Runde, 4 große Antwortknöpfe, 3 Leben. Es geht leicht los und wird mit jeder Runde schwerer (206 Runden nach `flaggenquiz-daten/spielablauf.json`). Nach jeder Antwort steht da, ob sie richtig war, und eine kurze Erklärung.

## Starten

Die Seite lädt JSON-Dateien und braucht deshalb einen kleinen Webserver:

```sh
python3 -m http.server
```

Dann http://localhost:8000 öffnen.

## Aufbau

* `index.html`: das Spiel (eine Datei, ohne Build-Schritt)
* `flaggenquiz-daten/`: Fragen, Länder, Flaggen und Spielablauf, das Datenformat steht in `flaggenquiz-daten/README.md`

Bedienung auch mit der Tastatur: `1`–`4` wählt eine Antwort, `Enter` geht weiter. Der Rekord wird nur im Browser gespeichert.

Flaggen-Grafiken: [flag-icons](https://github.com/lipis/flag-icons), MIT-Lizenz (`flaggenquiz-daten/flaggen/LIZENZ-flag-icons.txt`).
