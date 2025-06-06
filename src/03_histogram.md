---
title: Daten verstehen
style: css/custom.css
---

# Die Daten verstehen

Für Banken ist es relevant, möglichst genau vorherzusagen, ob ein neuer Kunde einen Kredit mit hoher Wahrscheinlichkeit zurückzahlt oder nicht. Im Finanzwesen wird dazu oft mit mathematischen Modellen gearbeitet, die jeder Person einen Kreditscore (z. B. der Schufa-Score) zuordnen. Wir gehen davon aus, dass zur Berechnung des Kreditscores verlässliche Daten verarbeitet wurden und nehmen den Kreditscore als ein sinnvolles Maß für die Kreditwürdigkeit einer Person an.

In diesem Lernmodul verwenden wir fiktive Daten von zahlreichen Kreditanwärter\*innen, die aber genau so in der Vergangenheit hätten zustande kommen können. Für diese Personen wurde der Kreditscore zwischen 0 (Kredit wird eher nicht zurückgezahlt) und 100 (Kredit wird sehr wahrscheinlich zurückgezahlt) berechnet. Außerdem ist bekannt, ob jede dieser Personen ihren Kredit in der Vergangenheit tatsächlich zurückgezahlt hat oder nicht.

Bevor wir mit einem größeren Datensatz arbeiten, wirst du zunächst die Struktur der Daten erkunden und wie sie mit Hilfe von Punktdiagrammen visualisiert werden können. In den Diagrammen repräsentiert jeder Punkt eine Person.

## Datensatz 1

Hier siehst du eine Tabelle mit fiktiven, aber realistischen Daten.

```js
const names1 = FileAttachment("data/user/random_user_1.csv").csv({
    typed: true,
});
const names2 = FileAttachment("data/user/random_user_2.csv").csv({
    typed: true,
});
const names3 = FileAttachment("data/user/random_user_3.csv").csv({
    typed: true,
});

function createTable(data) {
    return Inputs.table(data, {
        width: {
            name: 200,
            type: 200,
            score: 200,
        },
        columns: ["name", "type", "score"],
        header: {
            name: "Name",
            type: "Zahlungsfähigkeit in Vergangenheit",
            score: "Kreditscore",
        },
        align: {
            name: "left",
            type: "left",
            score: "left",
        },
        rows: 10,
        maxWidth: 800,
        multiple: false,
    });
}

function createPlot(data) {
    return Plot.plot({
        height: 200,
        width: 200,
        x: {
            label: "Score",
        },
        color: {
            legend: true,
            scheme: "Paired",
        },
        marks: [
            Plot.dot(
                data,
                Plot.stackY2({
                    x: "score",
                    fill: "type",
                    sort: {
                        value: "type",
                        reverse: false,
                    },
                    reverse: true,
                })
            ),
            Plot.ruleY([0]),
        ],
    });
}
```

```js
display(createTable(names1));
```

<div class="tip" label="Aufgabe 1">
Welches der folgenden Punktdiagramme A, B oder C repräsentiert die Daten aus der Tabelle? Du kannst die Tabelle nach Namen, Kreditscore oder Zahlungsfähigkeit sortieren, indem du auf den Titel der jeweiligen Spalte klickst. Scrolle durch die Tabelle, um alle Einträge zu sehen. Notiere deine Antwort.
</div>

<div class="grid grid-cols-3">
  <div class="card" style="max-width: 200px; "><h2>Option A</h2>${createPlot(names1)}</div>
  <div class="card" style="max-width: 200px; "><h2>Option B</h2>${createPlot(names2)}</div>
  <div class="card" style="max-width: 200px; "><h2>Option C</h2>${createPlot(names3)}</div>
</div>

## Datensatz 2

Hier siehst du eine weitere Tabelle.

```js
display(createTable(names2));
```

<div class="tip" label="Aufgabe 2">
 Welches Punktdiagramm repräsentiert die Daten in der Tabelle? Notiere deine Antwort.
</div>

<div class="grid grid-cols-3">
  <div class="card" style="max-width: 200px; "><h2>Option A</h2>${createPlot(names1)}</div>
  <div class="card" style="max-width: 200px; "><h2>Option B</h2>${createPlot(names2)}</div>
  <div class="card" style="max-width: 200px; "><h2>Option C</h2>${createPlot(names3)}</div>
</div>

## Datensatz 3

Hier siehst du eine weitere Tabelle.

```js
display(createTable(names3));
```

<div class="tip" label="Aufgabe 3">
Welches Punktdiagramm repräsentiert die Daten in der Tabelle? Notiere deine Antwort.
</div>

<div class="grid grid-cols-3">
  <div class="card" style="max-width: 200px; "><h2>Option A</h2>${createPlot(names1)} </div>
  <div class="card" style="max-width: 200px; "><h2>Option B</h2>${createPlot(names3)} </div>
  <div class="card" style="max-width: 200px; "><h2>Option C</h2>${createPlot(names2)} </div>
</div>

## Überprüfe nun deine Antworten

```js
display(html`
    <div class="table-container">
        <table id="histogramtable">
            <thead>
                <tr>
                    <th></th>
                    <th>Richtige Option (A, B oder C)</th>
                </tr>
            </thead>
            <tbody>
                <tr>
                    <th>Aufgabe 1</th>
                    <td contenteditable="true" data-correct="A"></td>
                </tr>
                <tr>
                    <th>Aufgabe 2</th>
                    <td contenteditable="true" data-correct="B"></td>
                </tr>
                <tr>
                    <th>Aufgabe 3</th>
                    <td contenteditable="true" data-correct="B"></td>
                </tr>
            </tbody>
        </table>
    </div>
`);
```

<button id="validateButton" class="btn btn-primary">Ergebnis überprüfen</button>

```js
import { eventListenerValidationString } from "./js/validateInput.js";

eventListenerValidationString(
    "#histogramtable td[contenteditable]",
    "validateButton"
);
```
