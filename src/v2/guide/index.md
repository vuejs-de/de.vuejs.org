---
title: Einführung
type: guide
order: 2
---

## Was ist Vue.js?

Vue (Englische Aussprache /vjuː/, wie **view**) ist ein **progressives framework** zur Entwicklung von Bedienoberflächen.
Im Unterschied zu anderen monolithischen frameworks ist Vue von Grund auf so konzipiert, dass es schrittweise "adaptiert" werden kann. Die Kern-Bibliothek ist nur für die Darstellungs-Schicht zuständig und kann leicht in andere Bibliotheken oder bestehende Projekte integriert werden. Auf der anderen Seite ist es mit Vue natürlich auch möglich mit den entsprechenden [Werkzeugen](single-file-components.html) und [Bibliotheken zur Unterstützung](https://github.com/vuejs/awesome-vue#components--libraries) anspruchsvolle Single-Page-Webanwendungen umzusetzen.

Falls Du bevor du richtig eintauchst erst einmal mehr über Vue lernen willst, haben <a id="modal-player"  href="#">wir ein Video erstellt</a> welches die Kern-Prinzipien anhand eines Beispielprojects erklärt.

Wenn Du ein erfahrener Frontend-Entwickler bist und wissen willst wie sich Vue von anderen Bibliotheken und Frameworks unterscheidet, schaue Dir den [Vergleich mit anderen Frameworks](comparison.html) an.

<div class="vue-mastery"><a href="https://www.vuemastery.com/courses/intro-to-vue-js/vue-instance/" target="_blank" rel="noopener" title="Free Vue.js Course">Schauen einen kostenlosen Video-Kurs von Vue Mastery</a></div>

## Loslegen

<a class="button" href="installation.html">Installation</a>

<p class="tip">Dieser offizielle Guide setzt fortgeschrittene Kenntnisse in HTML, CSS und JavaScript voraus. Wenn Du in der Frontend-Enwicklung noch komplett neu bist, ist es vielleicht nicht die beste Idee direkt mit diesem Framework anzufangen. Befasse Dich mit den Grundlagen und kommen dann zurück! Vorausgehende Erfahrung mit anderen Frameworks ist hilfreich, aber nicht zwingend notwendig.</p>

Die einfachste Möglichkeit Vue.js auszuprobieren ist das [JSFiddle Hello World Beispiel](https://jsfiddle.net/chrisvfritz/50wL7mdz/). Öffne es ruhig in einem in weiteren Tab während wir ein paar grundlegende Beispiele durchgehen. Oder Du <a href="https://gist.githubusercontent.com/chrisvfritz/7f8d7d63000b48493c336e48b3db3e52/raw/ed60c4e5d5c6fec48b0921edaed0cb60be30e87c/index.html" target="_blank" download="index.html" rel="noopener noreferrer">erstellst eine <code>index.html</code> Datei</a> und inkludierst Vue folgendermaßen:

``` html
<!-- Entwicklungsversion, enthält hilfreiche Konsolen-Warnungen -->
<script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
```

Oder:

``` html
<!-- Produktiv-Version, für Geschwindigkeit und Dateigröße optimiert -->
<script src="https://cdn.jsdelivr.net/npm/vue"></script>
```
Die [Installations-Seite](installation.html) enthält weitere Informationen zur Installation von Vue. Hinweis: Wir empfehlen das Anfänger **nicht** mit der `vue-cli` anfangen, besonders wenn Du noch keine Erfahrung mit Node.js-basierten Programmen hast. 

Wenn Du es lieber etwas interaktiver haben möchtest, dann schaue Dir [diese Tutorial-Reihe auf Scrimba](https://scrimba.com/playlist/pXKqta) an welche einen Screencast und eine Code-Spielwiese enthält so das du immer wenn du möchtest pausieren und ausprobieren kannst.

## Deklaratives Rendering

<div class="scrimba"><a href="https://scrimba.com/p/pXKqta/cQ3QVcr" target="_blank" rel="noopener noreferrer">Mache diese Übung auf Scrimba</a></div>

Im Kern von Vue.js gibt es ein System welches es uns ermöglicht Daten deklarativ mit einfacher Template-Syntax in das DOM zu rendern:

``` html
<div id="app">
  {{ message }}
</div>
```
``` js
var app = new Vue({
  el: '#app',
  data: {
    message: 'Hello Vue!'
  }
})
```
{% raw %}
<div id="app" class="demo">
  {{ message }}
</div>
<script>
var app = new Vue({
  el: '#app',
  data: {
    message: 'Hello Vue!'
  }
})
</script>
{% endraw %}

Wir haben soeben unsere erste Vue-Anwendung erstellt!
Dies sieht dem rendern eines String-Templates sehr ähnlich, aber Vue hat unter der Haube sehr viel mehr für uns gemacht. Die Daten und das DOM sind jetzt verbunden und alles ist **reaktiv**. Woher wissen wir das? Öffne Deine Java-Script-Konsole (jetzt, auf dieser Seite) und setze `app.message` auf einen anderen Wert. Das weiter oben gerenderte Beispiel sollte sich entsprechend aktualisieren. 

Neben Text-Interpolation können wir Daten auch an Attribute "binden":

``` html
<div id="app-2">
  <span v-bind:title="message">
    Lasse Deine Maus für ein paar Sekunden über mir verweilen 
    um meinen dynamisch gebundenen Title zu sehen!
  </span>
</div>
```
``` js
var app2 = new Vue({
  el: '#app-2',
  data: {
    message: 'Du hast diese Seite am ' + new Date().toLocaleString() + ' geladen'
  }
})
```
{% raw %}
<div id="app-2" class="demo">
  <span v-bind:title="message">
    Lasse Deine Maus für ein paar Sekunden über mir verweilen um meinen dynamisch gebundenen Title zu sehen!
  </span>
</div>
<script>
var app2 = new Vue({
  el: '#app-2',
  data: {
    message: 'Du hast diese Seite am ' + new Date().toLocaleString() + ' geladen'
  }
})
</script>
{% endraw %}

Hier lernen wir etwas neues kennen. Das `v-bind` Attribut aus dem Beispiel oben nennt man eine **Direktive**. Direktiven sind spezielle Attribute von Vue welche man an ihrem `v-`-Prefix erkennt und wie Du Dir sicher schon denken konntest erzeugen Direktiven reaktives Verhalt am DOM. In diesem Beisiel sagt die Direktive "halte das  `title` Attribut auf dem neusten Stand mit der `message` Eigenschaft der Vue-Instanz"

Wenn Du Deine JavaScript-Konsole wieder öffnest und `app2.message = 'some new message'` eingibst, siehst Du ein weiteres Mal wie das gebundene HTML - in diesem Fall das `title`  Attribute - aktualisiert wird.

## Bedingungen und Schleifen

<div class="scrimba"><a href="https://scrimba.com/p/pXKqta/cEQe4SJ" target="_blank" rel="noopener noreferrer">Mache diese Übung auf Scrimba</a></div>

Das umschalten der Sichtbarkeit eines Elements gestaltet sich ebenfalls einfach:

``` html
<div id="app-3">
  <span v-if="seen">Jetzt kannst Du mich sehen</span>
</div>
```

``` js
var app3 = new Vue({
  el: '#app-3',
  data: {
    seen: true
  }
})
```

{% raw %}
<div id="app-3" class="demo">
  <span v-if="seen">Jetzt kannst Du mich sehen</span>
</div>
<script>
var app3 = new Vue({
  el: '#app-3',
  data: {
    seen: true
  }
})
</script>
{% endraw %}

Geben nun `app3.seen = false` in der Konsole ein und die Nachricht sollte verschwinden.

Dieses Beispiel zeigt das wir Daten nicht nur an Text und Attributen binden können, sondern auch an die **Struktur** des DOM. Desweiteren stellt Vue auch ein Übergangs-Effekt-System bereit welche automatisch [Übergangs-Effekte](transitions.html) anwenden kann wenn Elemente von Vue eingefügt/aktualisiert/entfernt werden.

Es gibt noch eine ganze Menge weitere Direktiven, jede mit ihrer eigenen Funktionalität. Die `v-for` Direktive zum Beispiel kann benutzt werden um eine Liste von Items aus den Daten eines Arrays anzuzeigen.

``` html
<div id="app-4">
  <ol>
    <li v-for="todo in todos">
      {{ todo.text }}
    </li>
  </ol>
</div>
```
``` js
var app4 = new Vue({
  el: '#app-4',
  data: {
    todos: [
      { text: 'JavaScript lernen' },
      { text: 'Vue lernen' },
      { text: 'Etwas tolles bauen' }
    ]
  }
})
```
{% raw %}
<div id="app-4" class="demo">
  <ol>
    <li v-for="todo in todos">
      {{ todo.text }}
    </li>
  </ol>
</div>
<script>
var app4 = new Vue({
  el: '#app-4',
  data: {
    todos: [
      { text: 'JavaScript lernen' },
      { text: 'Vue lernen' },
      { text: 'Etwas tolles bauen' }
    ]
  }
})
</script>
{% endraw %}

Gebe in der Konsole `app4.todos.push({ text: 'New item' })` ein. Du solltest sehen das ein weiteres Item der Liste hinzugefügt wurde.

## Benutzereingaben verarbeiten

<div class="scrimba"><a href="https://scrimba.com/p/pXKqta/czPNaUr" target="_blank" rel="noopener noreferrer">Mache diese Übung auf Scrimba</a></div>

Damit Benutzer mit deiner Anwendung interagieren können, können wir die `v-on` Direktive benutzen um Event-Listener anzuhängen die Methoden von userer Vue-Instanz aufrufen. 

``` html
<div id="app-5">
  <p>{{ message }}</p>
  <button v-on:click="reverseMessage">Nachricht umkehren</button>
</div>
```
``` js
var app5 = new Vue({
  el: '#app-5',
  data: {
    message: 'Hallo Vue.js!'
  },
  methods: {
    reverseMessage: function () {
      this.message = this.message.split('').reverse().join('')
    }
  }
})
```
{% raw %}
<div id="app-5" class="demo">
  <p>{{ message }}</p>
  <button v-on:click="reverseMessage">Nachricht umkehren</button>
</div>
<script>
var app5 = new Vue({
  el: '#app-5',
  data: {
    message: 'Hallo Vue.js!'
  },
  methods: {
    reverseMessage: function () {
      this.message = this.message.split('').reverse().join('')
    }
  }
})
</script>
{% endraw %}

Interessant ist das wir mit dieser Methode nur den Zustand unserer Anwendung aktualisieren ohne dabei das DOM anzufassen - alle DOM-Manipulationen werden von Vue durchgeführt und unser Code konzentriert sich auf die darunter liegende Logik.  

Weiterhin stellt Vue die `v-model` Direktive bereit welche eine bi-direktionale Verbindung zwischen Benutzereingabe und Anwendungszustand zu einem Kinderspiel macht:

``` html
<div id="app-6">
  <p>{{ message }}</p>
  <input v-model="message">
</div>
```
``` js
var app6 = new Vue({
  el: '#app-6',
  data: {
    message: 'Hallo Vue!'
  }
})
```
{% raw %}
<div id="app-6" class="demo">
  <p>{{ message }}</p>
  <input v-model="message">
</div>
<script>
var app6 = new Vue({
  el: '#app-6',
  data: {
    message: 'Hallo Vue!'
  }
})
</script>
{% endraw %}

## Mit Komponenten arbeiten

<div class="scrimba"><a href="https://scrimba.com/p/pXKqta/cEQVkA3" target="_blank" rel="noopener noreferrer">Mache diese Übung auf Scrimba</a></div>

Das Komponenten-System ist ein weiteres wichtiges Konzept von Vue da es eine Abstraktion bereit stellt welche es uns erlaubt, große, umfangreiche Anwendungen aus kleineren, eigenständigen und oft wiederverwendbaren Komponenten zu erstellen. Wenn man darüber nachdenkt, dann kann jede Art von Bedienoberfläche als ein Baum aus Komponenten abstrahiert werden:

![Component Tree](/images/components.png)

In Vue, eine Komponenten ist im Wesentlichen eine Vue-Instanz mit im Voraus festgelegten Optionen. Eine Komponente in Vue zu registrieren ist umkompliziert:

``` js
// Lege eine Komponente mit dem Namen todo fest
Vue.component('todo-item', {
  template: '<li>Dies ist ein todo</li>'
})
```
Jetzt kann die Komponente im Template einer anderen Komponente verwendet werden:

``` html
<ol>
  <!-- Erzeuge eine Instanz der todo Komponente -->
  <todo-item></todo-item>
</ol>
```
Das Ergebnis wäre allerdings das jedes todo den gleichen Text hat, was nicht besonders interessant ist. Viel nützlicher wäre es, wenn wir Daten aus unserer Eltern-Komponente an die Kind-Komponente übergeben würden. Dazu passen wir die Komponenten-Definition so an, dass die Komponente ein [prop](components.html#Props) akzeptiert:

``` js
Vue.component('todo-item', {
  // Die todo-item Komponente akzeptiert ein
  // "prop", ein benutzerdefiniertes Attribut.
  // Das prop hat den Bezeichner todo.
  props: ['todo'],
  template: '<li>{{ todo.text }}</li>'
})
```

Jetzt können wir mit `v-bind` das todo an jede wiederholte Komponente weiterreichen:

``` html
<div id="app-7">
  <ol>
    <!--
      Jedes todo-item bekommt jetzt das todo Objekt 
      welches es repräsentiert so das der Inhalt dynamisch wird.
      Weiterhin müssen wir jeder Komponenten einen "key"
      mitgeben, aber darauf gehen wir später noch ein.
    -->
    <todo-item
      v-for="item in groceryList"
      v-bind:todo="item"
      v-bind:key="item.id"
    ></todo-item>
  </ol>
</div>
```
``` js
Vue.component('todo-item', {
  props: ['todo'],
  template: '<li>{{ todo.text }}</li>'
})

var app7 = new Vue({
  el: '#app-7',
  data: {
    groceryList: [
      { id: 0, text: 'Gemüse' },
      { id: 1, text: 'Käse' },
      { id: 2, text: 'Was Menschen sonst so essen' }
    ]
  }
})
```
{% raw %}
<div id="app-7" class="demo">
  <ol>
    <todo-item v-for="item in groceryList" v-bind:todo="item" :key="item.id"></todo-item>
  </ol>
</div>
<script>
Vue.component('todo-item', {
  props: ['todo'],
  template: '<li>{{ todo.text }}</li>'
})
var app7 = new Vue({
  el: '#app-7',
  data: {
    groceryList: [
      { id: 0, text: 'Gemüse' },
      { id: 1, text: 'Käse' },
      { id: 2, text: 'Was Menschen sonst so essen' }
    ]
  }
})
</script>
{% endraw %}

Dies mag ein erfundenes Beispiel sein, aber uns ist es gelungen unsere Anwendung in zwei kleinere Einheiten zu zerlegen welche durch die props-Schnittstelle entkoppelt sind. Unsere `<todo-item>` Komponenten können wir um ein komplexeres Template und Logik erweitern ohne das die Eltern-Anwendung beeinflusst wird.

In großen Anwendungen ist es notwendig alles in kleinere Komponenten aufzuteilen um die Entwicklung überblicken zu können. Wir werden [später in diesem Guide](components.html) noch viel über Komponenten reden, aber hier ist schon mal ein Beispiel wie das Template einer Anwendungen mit Komponenten aussehen kann:

``` html
<div id="app">
  <app-nav></app-nav>
  <app-view>
    <app-sidebar></app-sidebar>
    <app-content></app-content>
  </app-view>
</div>
```

### Ähnlichkeiten zu benutzerdefinierten Elementen

Dir ist vielleicht aufgefallen das Vue Komponenten starke Ähnlichkeiten mit **Benutzerdefinierten Elementen** haben welche Teil der [Webkomponenten Spezifikation](https://www.w3.org/wiki/WebComponents/) sind. Das liegt daran das Komponenten in Vue lose nach dieser Spezifikation modeliert sind. Zum Beispiel implementieren Komponenten in Vue die [Slot API](https://github.com/w3c/webcomponents/blob/gh-pages/proposals/Slots-Proposal.md) und das spezielle `is` Attribut. Es gibt allerdings einige zentrale Unterschiede: 

1. Die Webkomponenten Spezifikation ist zwar final aber noch nicht in jedem Browser nativ implementiert. Safari 10.1+, Chrome 54+ und Firefox 63+ haben native Unterstützung für Webkomponenten. Als Vergleich, Vue-Komponenten benötigen keine Polyfills und funktionieren konsisten in allen unterstützten Browsern (IE9 und darüber). Wenn nötig können Vue-Komponenten auch in native benutzerdefinierte Elemente eingebettet werden. 

2. Vue-Komponenten stellen wichtige Features bereit welche in einfachen benutzerdefinierten Elementen nicht verfügbar sind, vor allem Kreuz-Komponenten Datenfluß, benutzerdefinierte Ereignis-Kommunikation und Buildtool-Integration.

Obwohl Vue intern keine benutzerdefinierten Elemente verwendet hat es [großartige Interoperabilität](https://custom-elements-everywhere.com/#vue) wenn es darum geht benutzerdefinierte Elemente zu verwenden oder auszuliefern. Die Vue CLI unterstützt ebenfalls das bauen von Vue-Komponenten welche sich als benutzerdefinierte Elemente registrieren. 

## Bereit für mehr?

Wir haben uns kurz die grundlegenden Merkmale des Vue.js-Kerns angeschaut - im Verlauf dieses Guides werden wir diese und weitere, forgeschrittene Features im Detail betrachten, also lies am besten alles durch!

<div id="video-modal" class="modal"><div class="video-space" style="padding: 56.25% 0 0 0; position: relative;"><iframe src="https://player.vimeo.com/video/247494684" style="height: 100%; left: 0; position: absolute; top: 0; width: 100%; margin: 0" frameborder="0" webkitallowfullscreen mozallowfullscreen allowfullscreen></iframe></div><script src="https://player.vimeo.com/api/player.js"></script><p class="modal-text">Video by <a href="https://www.vuemastery.com" target="_blank" rel="noopener" title="Vue.js Courses on Vue Mastery">Vue Mastery</a>. Watch Vue Mastery’s free <a href="https://www.vuemastery.com/courses/intro-to-vue-js/vue-instance/" target="_blank" rel="noopener" title="Vue.js Courses on Vue Mastery">Intro to Vue course</a>.</div>
