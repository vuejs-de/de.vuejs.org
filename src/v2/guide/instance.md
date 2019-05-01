---
title: Die Vue-Instanz
type: guide
order: 3
---

## Eine Vue-Instanz erzeugen

Am Anfang einer Vue-Anwendung steht das erzeugen einer neuen **Vue-Instanz** mithilfe der `Vue` Funktion:

```js
var vm = new Vue({
  // options
})
```

Da das Design von Vue teilweise durch das [MVVM Entwurfsmuster](https://de.wikipedia.org/wiki/Model_View_ViewModel) inspiriert ist verwenden wir häufig per Konvention den Bezeichner `vm` (kurz für ViewModel) für die Vue-Instanz.

Beim Erzeugen einer Vue-Instanz wird ein **Optionen-Objekt** übergeben. Der Großteil dieses Guides beschäftige sich damit wie diese Optionen verwendet werden können um das erwünschte Verhalten zu erzeugen. Für eine Liste aller Optionen siehe die [API Referenz](../api/#Options-Data)

Eine Vue-Anwendung besteht aus einer **Wurzel-Vue-Instanz** welche mit `new Vue` erzeugen und optional als ein Baum von verschachtelten, wiederverwendbaren Komponenten organisiert ist. Der Komponenten-Baum einer Todo-Anwendung könnte beispielsweise so aussehen:

```
Wurzel-Instanz
└─ TodoListe
   ├─ TodoItem
   │  ├─ DeleteTodoButton
   │  └─ EditTodoButton
   └─ TodoListFooter
      ├─ ClearTodosButton
      └─ TodoListStatistics
```

[Das Komponenten-System](components.html) werden wir später noch im Detail behandeln. Für jetzt reicht es zu wissen das alle Vue-Komponenten auch Vue-Instanzen sind und somit das gleiche Optionen-Objekt akzeptieren (Ein paar wurzel-spezifische Optionen ausgenommen).

## Daten und Methoden

Wenn eine Vue-Instanz erzeugt wird, werden alle Eigenschaften des `data` Objekts zu Vue's **Reaktivitäts-System** hinzugefügt. Wenn sich die Werte dieser Eigenschaften ändern wird die View "reagieren" und sich mit den neuen Werten aktualisieren.  

```js
// Unser Daten-Objekt
var data = { a: 1 }

// Das Objekt wird einer Vue-Instanz hinzugefügt
var vm = new Vue({
  data: data
})

// Die Eigschaft der Instanz entspricht
// der Eigschaft des ursprünglichen Objekts
vm.a == data.a // => true

// Das Verändern der Eigeschaft über die Instanz 
// verändert auch die Eigenschaft am Daten ursprünglichen Objekt
vm.a = 2
data.a // => 2

// ... und vice versa
data.a = 3
vm.a // => 3
```

Wenn sich die Daten ändern, wird die View neu gerendert. Es sollte zur Kenntniss genommen werden das nur jede Eigschaften von `data` **reaktiv** sind welche beim Erzeugen der Instanz existierten. Das bedeutet, dass wenn Du eine neue Eigeschaft hinzufügst:

```js
vm.b = 'hi'
```
Die Änderung von `b` keine Aktualisierung der View auslösen. Wenn du weißt das Du eine Eigeschaft später benötigen wirst sie aber zu Beginn leer sein soll, dann musst Du ihr einen initialen Wert zuweisen. Zum Beispiel: 

```js
data: {
  newTodoText: '',
  visitCount: 0,
  hideCompletedTodos: false,
  todos: [],
  error: null
}
```

Die einzige Ausnahme ist wenn `Object.freeze()` verwendet wird was verhindert das existierende Eigenschaften verändert werden können, was auch bedeutet dass das Reaktivitäts-System Änderungen nicht mehr _nachverfolgen_ kann.

```js
var obj = {
  foo: 'bar'
}

Object.freeze(obj)

new Vue({
  el: '#app',
  data: obj
})
```

```html
<div id="app">
  <p>{{ foo }}</p>
  <!-- `foo` wird nicht mehr aktualisiert! -->
  <button v-on:click="foo = 'baz'">foo verändern</button>
</div>
```

Neben den Daten-Eigenschaften bietet Vue noch eine Menge weiterer, nützlicher Instanz-Methoden und -Eigenschaften. Diese sind mit dem `$` Prefix versehen um sie von benutzerdefinierten Eigenschaften zu unterscheiden. Zum Beispiel:

```js
var data = { a: 1 }
var vm = new Vue({
  el: '#example',
  data: data
})

vm.$data === data // => true
vm.$el === document.getElementById('example') // => true

// $watch ist eine Instanz-Methode
vm.$watch('a', function (newValue, oldValue) {
  // Dieser Callback wird aufgerüfen wenn sich `vm.a` ändert
})
```
Für eine vollständige Liste der Instanz-Eigenschaften und -Methoden siehe die [API Referenz](../api/#Instance-Properties).

## Instanz-Lebenszyklus-Hooks

Wenn eine Vue-Instanz erzeugt wurde durchläuft sie bei der Initialisierung eine Reihe von Schritten - zum Beispiel muss das Beobachten von Daten eingerichtet, das template kompiliert, die Instanz in das DOM eingehängt und das DOM bei Änderungen aktualisiert werden. Während all dies passiert werden Funktionen ausgeführt welche man **Lebenszyklus-Hooks** nennt und dem Benutzer die Gelegenheit geben, an bestimmten Abschnitten eigenen Code hinzuzufügen.

Der [`created`](../api/#created) Hook kann zum Beispiel verwendet werden um Code auszuführen nachdem eine Instanz erzeugt wurde:

```js
new Vue({
  data: {
    a: 1
  },
  created: function () {
    // `this` zeigt auf die vm Instanz
    console.log('a ist: ' + this.a)
  }
})
// => "a ist: 1"
```

Es gibt noch weitere Hooks welche an verschiedenen Abschnitten im Lebenszyklus einer Instanz aufgerufen werden, wie [`mounted`](../api/#mounted), [`updated`](../api/#updated), und [`destroyed`](../api/#destroyed). Alle Lebenszyklus-Hooks werden so aufgerufen das ihr `this` Kontext auf die Vue-Instanz zeigt zu der sie gehören. 

<p class="tip">Benutze keine [arrow functions](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Functions/Arrow_functions) auf einer Optionen-Eigenschaft oder als Callback, wie `created: () => console.log(this.a)` oder `vm.$watch('a', newValue => this.myMethod())`. Da es in arrow functions kein `this` gibt wird `this` als normaler Variablenbezeichner angesehen und lexikalisch in einschließenden Geltungsbereichen gesucht was häufig zu Fehlern wie `Uncaught TypeError: Cannot read property of undefined` oder `Uncaught TypeError: this.myMethod is not a function` führt.</p>

## Lebenszyklus-Diagramm

Nachfolgend ist ein Diagramm des Instanz-Lebenszyklus. Du musst an diesem Punkt nicht alles davon verstehen, aber wenn Du weiter fortgeschritten bist wird dieses Diagramm eine nützliche Referenz sein. 

![The Vue Instance Lifecycle](/images/lifecycle.png)
