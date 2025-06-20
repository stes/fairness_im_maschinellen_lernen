---
title: Variable Entscheidungsgrenze
style: css/custom.css
---

# Variable Entscheidungsgrenze

<!-- Include Font Awesome -->
<link href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/5.15.4/css/all.min.css" rel="stylesheet">


```js
const data = FileAttachment("data/user/distribution.csv").csv({
  typed: true,
});
```

Jetzt kannst du die Entscheidungsgrenze variieren und die aus deiner Sicht optimale Entscheidungsgrenze festlegen.

<div class="tip" label="Aufgabe">
   <i class="fas fa-pencil-alt"></i>
Welcher Wert stellt aus deiner Sicht eine optimale Entscheidungsgrenze dar? Notiere den Wert und begründe deine Wahl.
</div>

Entscheidungsgrenze:

```js
const threshAlt = view(
  Inputs.range([0, 100], {
    step: 1,
    label: "",
  })
);
```

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
          fillOpacity: (d) => (d.score < threshAlt ? 0.3 : 1),
                  sort: {
          value: "type", 
          reverse: false 
        },
        reverse: true
        })
      ),
      Plot.ruleY([0]),
      Plot.ruleX([threshAlt - 0.5]),
    ],
  })
);
```

Unsere Vorhersage bei einer Entscheidungsgrenze von ${threshAlt}:

```js
import { calculateMetrics } from "./js/calculateMetrics.js";
```

```js
const {
  grp: groupedData,
  n_true_positive,
  n_false_positive,
  n_false_negative,
  n_true_negative,
  total: n_total,
  total_positive: n_total_true,
  precision,
  recall,
  positive_rate,
  true_positive_rate,
  gewinn,
  accuracy,
} = calculateMetrics(data, "", threshAlt);
```


```html
<div class="table-container">
  <table>
    <thead>
      <tr>
        <th></th>
        <th>
          Vorhersage:<br />
          zahlt zurück
        </th>
        <th>
          Vorhersage:<br />
          zahlt nicht zurück
        </th>
        <th>Summe</th>
      </tr>
    </thead>
    <tbody>
      <tr>
        <th>Daten:<br />zahlt zurück</th>
        <td style="background-color: rgba(0, 128, 0, 0.35); color: black;">
          ${n_true_positive}
        </td>
        <td>
          ${n_false_negative}
        </td>
        <td><b>${n_true_positive + n_false_negative}</b></td>
      </tr>
      <tr>
        <th>Daten:<br />zahlt nicht zurück</th>
        <td>
          ${n_false_positive}
        </td>
        <td style="background-color: rgba(0, 128, 0, 0.35); color: black;">
          ${n_true_negative}
        </td>
        <td><b>${n_false_positive + n_true_negative}</b></td>
      </tr>
      <tr>
        <th>Summe</th>
        <td><b>${n_true_positive + n_false_positive}</b></td>
        <td><b>${n_false_negative + n_true_negative}</b></td>
        <td><b>${n_total}</b></td>
      </tr>
    </tbody>
  </table>
</div>
```


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
        <td contenteditable="false">${accuracy}%</td>
        <td contenteditable="false">${positive_rate}%</td>
        <td contenteditable="false">${true_positive_rate}%</td>
        <td contenteditable="false">${gewinn}€</td>
      </tr>
    </tbody>
  </table>
</div>
```
