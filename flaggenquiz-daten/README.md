# Flaggenquiz – Fragen-Daten

Dieser Ordner enthält alles, was ein Multiple-Choice-Flaggenquiz für Kinder braucht: 206 Flaggen, 2.193 fertige Fragen in 9 Fragetypen und 206 Schwierigkeitsstufen. Alle Texte sind auf Deutsch und in Du-Form, die Zielgruppe sind Kinder ab etwa 7 Jahren.

`index.html` ist eine spielbare Vorschau, die die Daten direkt nutzt. Sie dient als Referenz und kann ersetzt werden. Zum lokalen Ansehen braucht sie einen kleinen Webserver, z. B. `python3 -m http.server` in diesem Ordner, dann http://localhost:8000 öffnen.

## Dateien

| Datei / Ordner | Inhalt |
|---|---|
| `fragen.json` | Alle Fragen (`fragen`) plus Kennzahlen (`info`) |
| `laender.json` | 206 Länder mit Name, Rang (Schwierigkeit), Farben, Farbanteilen, Sternen, ähnlichen Flaggen |
| `spielablauf.json` | Empfohlene Reihenfolge für ein Spiel: 206 Runden, Runde *n* = Land mit Rang *n* |
| `flaggen/*.svg` | Original-Flaggen im Format 4:3 (aus dem Projekt *flag-icons*, MIT-Lizenz, siehe `flaggen/LIZENZ-flag-icons.txt`) |
| `varianten/*.png` | Veränderte Flaggen für „Welche Farbe fehlt?“ und „Fehler finden“ (640×480) |

## Länder

* 193 UN-Mitgliedsstaaten (`kategorie: "staat"`)
* 4 Staaten ohne UN-Vollmitgliedschaft: Vatikanstadt, Palästina, Kosovo, Taiwan (`"staat_ohne_un"`)
* 9 Gebiete mit eigener, bei Kindern oft bekannter Flagge: England, Schottland, Wales, Nordirland, Grönland, Färöer, Puerto Rico, Hongkong, Westsahara (`"gebiet"`)

Wer nur Staaten möchte, filtert `kategorie != "gebiet"` und kommt auf 197, genau wie das Zeit-Quiz.

Der **Rang** (1–206) ist die Reihenfolge nach Bekanntheit für ein Kind in Deutschland: Nachbarländer, Fußballnationen und Urlaubsländer zuerst, kleine Inselstaaten zuletzt. Leicht zu merkende Flaggen (Japan, Schweiz, Kanada) stehen weiter vorn.

## Aufbau einer Frage

```json
{
  "id": "farbe_fehlt_it_1",
  "typ": "farbe_fehlt",
  "stufe": 9,
  "land": "it",
  "kategorie": "staat",
  "frage": "Bei der Flagge von Italien fehlt eine Farbe. Welche Farbe gehört an die graue Stelle?",
  "bild": { "datei": "varianten/it_ohne_gruen.png", "transform": "keine" },
  "antworten": [
    { "text": "Schwarz",  "farbe_hex": "#000000" },
    { "text": "Blau",     "farbe_hex": "#0033a0" },
    { "text": "Grün",     "farbe_hex": "#009a44" },
    { "text": "Hellblau", "farbe_hex": "#6cace4" }
  ],
  "richtig": 2,
  "aufloesung": "Richtig wäre Grün. Das ist die Flagge von Italien. Ihre Farben sind Grün, Rot und Weiß."
}
```

| Feld | Bedeutung |
|---|---|
| `id` | Eindeutige ID: `<typ>_<ländercode>_<nr>` |
| `typ` | Einer der 9 Fragetypen (siehe unten) |
| `stufe` | Schwierigkeit 1–206 (1 = am leichtesten) |
| `land` | Ländercode der Frage (Schlüssel für `laender.json`) |
| `frage` | Fragetext für das Kind |
| `hinweis` | Optionaler Zusatz unter der Frage |
| `bild` | Das große Bild über der Frage (fehlt bei `flagge_waehlen`, `richtig_herum`, `gespiegelt`, `farbanteile`) |
| `farbkreis` | Nur bei `farbanteile`: Liste `{farbe, hex, anteil}`, Anteile ergeben zusammen 1, nach Größe sortiert |
| `antworten` | Immer genau 4. Jede Antwort hat `text` und/oder `bild`, Farbantworten zusätzlich `farbe_hex` für einen Farbtupfer, Länderantworten `land` |
| `richtig` | Index (0–3) der richtigen Antwort in `antworten` |
| `aufloesung` | Satz, der nach dem Antworten angezeigt wird |

**`bild.transform`** wird per CSS auf das Bild angewendet:

| Wert | CSS |
|---|---|
| `keine` | – |
| `spiegeln` | `transform: scaleX(-1)` |
| `kopf` | `transform: scaleY(-1)` |
| `drehen` | `transform: rotate(180deg)` |

Die Reihenfolge der Antworten ist bereits gemischt. Wer sie bei jedem Spiel neu mischen will, muss `richtig` mitführen.

## Die 9 Fragetypen

| Typ | Frage | Anzeige | Antworten | Anzahl | ab Stufe |
|---|---|---|---|---|---|
| `land_erkennen` | Welches Land hat diese Flagge? | Flagge | 4 Ländernamen | 206 | 1 |
| `flagge_waehlen` | Welche ist die Flagge von …? | – | 4 Flaggen | 206 | 3 |
| `farbe_fehlt` | Welche Farbe gehört an die graue Stelle? | Flagge mit grau kariertem Loch | 4 Farben mit Farbtupfer | 538 | 4 |
| `farben_zaehlen` | Wie viele verschiedene Farben hat diese Flagge? | Flagge | 4 Zahlen | 168 | 6 |
| `sterne_zaehlen` | Wie viele Sterne siehst du? | Flagge | 4 Zahlen | 70 | 8 |
| `richtig_herum` | Welche Flagge von … ist richtig herum? | – | dieselbe Flagge 4× (normal, gespiegelt, auf dem Kopf, gedreht) | 70 | 12 |
| `fehler_finden` | Das soll die Flagge von … sein. Was ist falsch? | veränderte Flagge | 4 Aussagen | 624 | 15 |
| `gespiegelt` | Eine dieser Flaggen ist spiegelverkehrt. Welche? | – | 4 Flaggen mit Namen, eine gespiegelt | 105 | 30 |
| `farbanteile` | Der Kreis zeigt die Farbanteile einer Flagge. Welche? | Tortendiagramm (wie im Zeit-Quiz) | 4 Ländernamen | 206 | 50 |

Hinweise zu einzelnen Typen:

* **fehler_finden** hat drei Arten von Fehlern: eine Farbe ist vertauscht (Bild aus `varianten/`), die Flagge ist gespiegelt oder sie steht auf dem Kopf (Original-SVG mit `transform`). Die falschen Antworten sind immer nachweislich falsche Aussagen. Farbtausche, bei denen zufällig die Flagge eines anderen Landes entstehen würde, sind aussortiert.
* **sterne_zaehlen** enthält ein paar Trickfragen mit 0 Sternen (Japan, Kanada, Südkorea u. a.). Bei den USA (50) und Brasilien (27) wird nach Wissen gefragt statt nach Zählen.
* **richtig_herum, fehler_finden und gespiegelt** nutzen nur Flaggen, bei denen der Unterschied auch für ein Kind gut zu sehen ist.
* **farbanteile:** Die drei falschen Länder haben immer deutlich andere Farbanteile, sodass die Antwort eindeutig ist.

## Schwierigkeit

`stufe` = Rang des Landes + Aufschlag für den Fragetyp, gedeckelt auf 1–206:

| Typ | Aufschlag |
|---|---|
| land_erkennen | +0 |
| flagge_waehlen, farben_zaehlen | +3 |
| farbe_fehlt, sterne_zaehlen | +5 |
| richtig_herum | +10 |
| fehler_finden | +15 |
| gespiegelt | +25 |
| farbanteile | +35 |

Zusätzlich werden die falschen Antworten mit der Stufe schwerer. Auf niedrigen Stufen kommen nur bekannte Länder mit deutlich unterschiedlichen Flaggen vor (Deutschland gegen USA, Norwegen und Polen). Ab Stufe 73 ist immer mindestens ein echter Verwechsler dabei, und die Ablenker werden immer ähnlicher: Tschad gegen Venezuela, Liechtenstein und Andorra, El Salvador gegen Griechenland, Nicaragua und Island, Jemen gegen Irak, Sudan und Syrien. Flaggen, die in der 4:3-Darstellung praktisch gleich aussehen (Indonesien/Monaco, Tschad/Rumänien), erscheinen nie gemeinsam als Antworten.

Weil viele schwere Fragetypen bei hohen Rängen an die Obergrenze stoßen, liegen auf Stufe 206 besonders viele Fragen (149).

## Spielablauf

**Variante A, fertig:** `spielablauf.json` enthält 206 Runden. In Runde *n* kommt das Land mit Rang *n* dran, mit wechselndem Fragetyp. Jeder Typ erscheint frühestens ab seiner Einstiegsstufe. Das ergibt ein Spiel wie bei der Zeit: jede Runde eine neue Flagge, 3 Leben.

**Variante B, jedes Mal anders:** Für Runde *n* zufällig eine Frage wählen, deren `stufe` zwischen *n − 5* und *n + 5* liegt und deren Land in diesem Spiel noch nicht vorkam. So hat jedes Spiel andere Fragen, die Kurve bleibt gleich.

## Weitere Daten in `laender.json`

```json
{
  "code": "it", "name": "Italien", "von": "von Italien", "rang": 4, "kategorie": "staat",
  "kontinent": "Europa", "flagge": "flaggen/it.svg",
  "farben": [{ "farbe": "Grün", "hex": "#009246", "anteil": 0.3336 }],
  "farbkreis": [{ "farbe": "Grün", "hex": "#009246", "anteil": 0.334 }],
  "sterne": null,
  "aehnliche_flaggen": ["mx", "ie", "pe", "dz", "ng", "mt", "sr", "bi"]
}
```

* `von` ist die kindgerechte Form mit Artikel („von der Schweiz“, „vom Irak“, „von den USA“). Damit lassen sich eigene Fragen bauen.
* `aehnliche_flaggen` sind die 8 ähnlichsten Flaggen (nach Aufbau und Farben), nützlich für eigene Ablenker.
* `sterne` ist nur bei Flaggen gesetzt, für die es eine Sterne-Frage gibt.
* Farbnamen: Rot, Dunkelrot, Orange, Gelb, Grün, Hellblau, Blau, Schwarz, Weiß (in Wappen selten auch Grau, Braun, Lila, Rosa).

## Gut zu wissen

* Die Flaggen stammen aus *flag-icons* (Stand Oktober 2026). Syrien ist schon mit der neuen Flagge mit drei roten Sternen dabei. Afghanistan wird dort noch mit der schwarz-rot-grünen Flagge der früheren Republik gezeigt.
* Alle Flaggen werden im Format 4:3 angezeigt, auch wenn das echte Seitenverhältnis anders ist (die Schweiz und der Vatikan sind eigentlich quadratisch, Nepal ist keine Rechteckflagge und hat deshalb einen transparenten Hintergrund).
* Lizenz der Flaggen-Grafiken: MIT. Die Lizenzdatei muss bei einer Veröffentlichung mitgeliefert werden.

## Prompt für Claude Code

> Baue ein Flaggenquiz für meinen 7-jährigen Sohn als Web-App. Im Ordner `flaggenquiz-daten` liegen alle Fragen, Flaggen und eine `README.md`, die das Datenformat erklärt. Lies zuerst die README. Spielprinzip wie das Flaggenquiz der Zeit: eine Frage pro Runde, 4 Antwortknöpfe, 3 Leben, mit jeder Runde schwerer. Nutze `spielablauf.json` für die Reihenfolge. Zeige nach jeder Antwort, ob sie richtig war, und den Text aus `aufloesung`. Die Gestaltung soll kindgerecht sein: große Knöpfe, gut lesbare Schrift, kurze Erfolgsmeldungen. `index.html` ist eine funktionierende Vorschau, die du als Ausgangspunkt nehmen kannst.
