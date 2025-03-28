---
title: Statistische Gütemaße
style: css/custom.css
---

# Statistische Gütemaße

## Ist die Entscheidungsgrenze wirklich gut gewählt?

```js
const data = FileAttachment("data/user/distribution.csv").csv({
  typed: true,
});
const fixedThreshAlt = 70;
```

Zur Beantwortung dieser Frage und zur Validierung des Kreditvergabesystems werden wir den Gesamtprofit sowie verschiedene statische Gütemaße nutzen.

Die Entscheidungsgrenze nutzen wir zur Vorhersage. Hat eine Person einen Kreditscore größer oder gleich unserer Entscheidungsgrenze, so sagen wir voraus, dass diese Person den Kredit zurückzahlen wird. Bei einem Kreditscore kleiner als unsere Entscheidungsgrenze sagen wir voraus, dass diese Person den Kredit nicht zurückzahlen wird.
Die Entscheidungsgrenze wurde zunächst fest auf 70 gesetzt. Für alle Personen mit einem Score größer oder gleich 70 gehen wir davon aus, dass sie den Kredit zurückzahlen werden (Vorhersage: zahlt zurück). Für alle Personen mit einem Score unter 70 gehen wir davon aus, dass sie den Kredit nicht zurückzahlen werden (Vorhersage: zahlt nicht zurück). Diese Vorhersagen können wir nun mit den vorliegenden Daten vergleichen. 

Erinnerung: Wir arbeiten mit Daten von Personen, bei denen bekannt ist, ob sie ihren Kredit in der Vergangenheit zurückgezahlt haben (Daten: zahlt zurück oder Daten: zahlt nicht zurück) und nutzen dies zum Abgleich mit unserer Vorhersage.


```js
display(
  Plot.plot({
    width: 600,
    height: 200,
    style: {
      fontSize: 18,
    },
    x: {
      label: "Score",
      domain: [0, 99],
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
          sort: "type",
          fillOpacity: (d) => (d.score < fixedThreshAlt ? 0.3 : 1),
          sort: {
            value: "type", 
            reverse: false 
          },
          reverse: true
        })
      ),
      Plot.ruleY([0]),
      Plot.ruleX([fixedThreshAlt - 0.5]),
    ],
  })
);
```

```js
const groupedData = data.reduce((acc, item) => {
  const type = item.type;
  const score = item.score;
  if (!acc[type]) {
    acc[type] = {
      belowThreshAlt: 0,
      aboveThreshAlt: 0,
    };
  }
  if (score < fixedThreshAlt) {
    acc[type].belowThreshAlt += 1;
  } else {
    acc[type].aboveThreshAlt += 1;
  }
  return acc;
}, {});
```

## Die Konfusionsmatrix

Die Anzahl der richtigen und falschen Vorhersagen für beide Personengruppen (“zahlt zurück” und “zahlt nicht zurück”) sind in der folgenden Tabelle dargestellt. Diese Tabelle wird auch als Konfusionsmatrix bezeichnet.




```html
<div class="table-container">
  <table>
    <thead>
      <tr>
        <th></th>
        <th>Vorhersage:<br />Zahlt zurück</th>
        <th>
          Vorhersage:<br />
          Zahlt nicht zurück
        </th>
      </tr>
    </thead>
    <tbody>
      <tr>
        <th>Daten:<br />Zahlt zurück</th>
        <td contenteditable="false" style="background-color: rgba(0, 128, 0, 0.35); color: black;">
          ${groupedData['Zahlt zurück']['aboveThreshAlt']}
        </td>
        <td contenteditable="false">
          ${groupedData['Zahlt zurück']['belowThreshAlt']}
        </td>
      </tr>
      <tr>
        <th>Daten:<br />Zahlt nicht zurück</th>
        <td contenteditable="false">
          ${groupedData['Zahlt nicht zurück']['aboveThreshAlt']}
        </td>
        <td contenteditable="false" style="background-color: rgba(0, 128, 0, 0.35); color: black;">
          ${groupedData['Zahlt nicht zurück']['belowThreshAlt']}
        </td>
      </tr>
      <tr></tr>
    </tbody>
  </table>
</div>

```



<div class="tip" label="Aufgabe 1">
Wie viele Personen erhalten bei einer Entscheidungsgrenze von 70 insgesamt einen Kredit?
</div>

<div class="answer-container">
  <input class="answer-field" rows="3" placeholder="Deine Antwort zu Aufgabe 1..."></textarea>
</div>

<div class="tip" label="Aufgabe 2">
Wie viele Personen erhalten bei einer Entscheidungsgrenze von 70 einen Kredit, obwohl sie nicht zahlungsfähig sind? 
</div>

<div class="answer-container">
  <input class="answer-field" rows="3" placeholder="Deine Antwort zu Aufgabe 2..."></textarea>
</div>


## Bewertung des Entscheidungsmodells
Es gibt verschiedene Gütemaße, die wir zur Bewertung unseres Modells verwenden können:

- <b>Genauigkeit:</b> Die Genauigkeit gibt den prozentualen Anteil der richtigen Vorhersagen an der Gesamtzahl aller Datenpunkte an.
- <b>Positivrate:</b> Die Positivrate gibt den prozentualen Anteil der positiven Vorhersagen (Vorhersage: zahlt zurück) an der Gesamtzahl aller Datenpunkte an.
- <b>Richtig-positiv-Rate:</b> Die Richtig-positiv-Rate gibt den prozentualen Anteil der richtig positiven Vorhersagen (richtig als "zahlt zurück" vorhergesagt) an der Anzahl aller tatsächlich positiven Datenpunkte (Daten: zahlt zurück) an.
- <b>Gewinn:</b> Erzielter Gesamtgewinn der Bank (Erinnerung: die Bank erhält 300€ für jeden zurückgezahlten Kredit und verliert 700€ für jeden nicht zurückgezahlten Kredit).



<div class="tip" label="Aufgabe 3">
Berechne die Werte der vier Gütemaße. Nutze dazu die Werte in der Konfusionsmatrix. Trage deine Ergebnisse in der folgenden Tabelle ein.
</div>

```html
<div class="table-container">
  <table>
    <thead>
      <tr>
        <th>Genauigkeit</th>
        <th>Positivrate</th>
        <th>Richtig-positiv-Rate</th>
        <th>Gewinn</th>
      </tr>
    </thead>
    <tbody>
      <tr>
        <td contenteditable="true"></td>
        <td contenteditable="true"></td>
        <td contenteditable="true"></td>
        <td contenteditable="true"></td>
      </tr>
    </tbody>
  </table>
</div>
```

<div class="tip" label="Aufgabe 4">
Wieviel Prozent der Personen, die zahlungsfähig sind, erhalten auch tatsächlich einen Kredit?
</div>

<div class="answer-container">
  <input class="answer-field" rows="3" placeholder="Deine Antwort zu Aufgabe 4..."></textarea>
</div>

<div class="tip" label="Aufgabe 5">
Wieviel Prozent der Personen im Datensatz erhalten einen Kredit? 
</div>

<div class="answer-container">
  <input class="answer-field" rows="3" placeholder="Deine Antwort zu Aufgabe 5..."></textarea>
</div>

