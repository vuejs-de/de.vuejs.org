---
title: Bedingtes Rendering
type: guide
order: 7
---

## `v-if`

Die `v-if` Direktive wird benutzt um einen Block bedingt zu rendern. Der Block wird nur gerendert wenn der Ausdruck der Direktive einen truthy Wert zurückgibt. 

``` html
<h1 v-if="awesome">Vue ist toll!</h1>
```

Es ist auch möglich einen "else Block" mit `v-else` hinzuzufügen:

``` html
<h1 v-if="awesome">Vue ist toll!</h1>
<h1 v-else>Oh nein 😢</h1>
```

### Bedingte Gruppen mit `v-if` auf `<template>`

Da `v-if` eine Direktive gilt sie nur für jeweils ein Element. Aber was wenn wir mehrere Elemente umschalten wollen? In diesem Fall können wir `v-if` an ein `<template>` Element binden welches als Hülle dient. Das `<template>` Elemente ist im gerenderte Ergebnis nicht enthalten.  

``` html
<template v-if="ok">
  <h1>Title</h1>
  <p>Paragraph 1</p>
  <p>Paragraph 2</p>
</template>
```

### `v-else`

Du kannst die `v-else` Direktive benutzen um einen "else block" für `v-if` anzugeben:

``` html
<div v-if="Math.random() > 0.5">
  Jetzt siehst du mich
</div>
<div v-else>
  Jetzt siehst du mich nicht
</div>
```

Ein `v-else` Element muss direkt auf ein `v-if` oder `v-else-if` Element folgen - anderfalls wird es nicht erkannt.

### `v-else-if`

> Neu in 2.1.0+

Wie der Name bereits andeutet dient das `v-else-if` als ein "else if Block" für `v-if`. Eine Mehrfach-Verkettung ist möglich: 

```html
<div v-if="type === 'A'">
  A
</div>
<div v-else-if="type === 'B'">
  B
</div>
<div v-else-if="type === 'C'">
  C
</div>
<div v-else>
  Nicht A/B/C
</div>
```

Ähnlich wie `v-else` muss ein `v-else-if` Element direkt hinter einem `v-if` oder `v-else-if` Element stehen.

### Wiederverwendbare Elemente mit `key` steuern

Vue versucht Elemente so effizient wie möglich zu rendern, häufig durch wiederverwenden statt von Grund auf neu zu rendern. Dies macht Vue nicht nur sehr schnell, sondern kann noch weitere Vorteile mit sich bringen. Zu Beispiel wenn es dem Benutzer erlaubt ist zwischen verschiedenen Login-Typen zu wählen:

``` html
<template v-if="loginType === 'benutzername'">
  <label>Benutzername</label>
  <input placeholder="Benutzername eingeben">
</template>
<template v-else>
  <label>E-Mail</label>
  <input placeholder="E-Mail-Adresse eingeben">
</template>
```

Then switching the `loginType` in the code above will not erase what the user has already entered. Since both templates use the same elements, the `<input>` is not replaced - just its `placeholder`.

Check it out for yourself by entering some text in the input, then pressing the toggle button:

{% raw %}
<div id="no-key-example" class="demo">
  <div>
    <template v-if="loginType === 'benutzername'">
      <label>Benutzername</label>
      <input placeholder="Benutzername eingeben">
    </template>
    <template v-else>
      <label>E-Mail</label>
      <input placeholder="E-Mail-Adresse eingeben">
    </template>
  </div>
  <button @click="toggleLoginType">Login-Typ umschalten</button>
</div>
<script>
new Vue({
  el: '#no-key-example',
  data: {
    loginType: 'benutzername'
  },
  methods: {
    toggleLoginType: function () {
      return this.loginType = this.loginType === 'benutzername' ? 'email' : 'benutzername'
    }
  }
})
</script>
{% endraw %}

Da dieses Verhalten nicht immer gewünscht ist bietet Vue eine Möglichkeit zu sagen "Diese beiden Elemente sind komplett voneinander getrennt - verwende sie nicht wieder". Dazu reicht es ein `key` Attribute mit jeweils einzigartigen Werte zu setzen: 

``` html
<template v-if="loginType === 'benutzername'">
  <label>Benutzername</label>
  <input placeholder="Benutzername eingeben" key="benutzername-input">
</template>
<template v-else>
  <label>E-Mail</label>
  <input placeholder="E-Mail-Adresse eingeben" key="email-input">
</template>
```

Jetzt werden die Input-Felder von Grund auf neu gerendert wenn umgeschalten wird. Siehe selbst:

{% raw %}
<div id="key-example" class="demo">
  <div>
    <template v-if="loginType === 'benutzername'">
      <label>Benutzername</label>
      <input placeholder="Benutzername eingeben" key="benutzername-input">
    </template>
    <template v-else>
      <label>E-Mail</label>
      <input placeholder="E-Mail-Adresse eingeben" key="email-input">
    </template>
  </div>
  <button @click="toggleLoginType">Login-Typ umschalten</button>
</div>
<script>
new Vue({
  el: '#key-example',
  data: {
    loginType: 'benutzername'
  },
  methods: {
    toggleLoginType: function () {
      return this.loginType = this.loginType === 'benutzername' ? 'email' : 'benutzername'
    }
  }
})
</script>
{% endraw %}

Die `<label>` Elemente werden weiterhin effizient wiederverwendet da sie kein `key` Attribut gesetzt haben.

## `v-show`

Eine weitere Option um ein Element bedingt anzuzeigen ist die `v-show` Direktive. Die Benutzung gestaltet sich ziemlich gleich:

``` html
<h1 v-show="ok">Hallo!</h1>
```

Der Unterschied liegt darin das ein Element mit `v-show` immer gerendert wird und im DOM verbleibt; `v-show` steuert lediglich die `display` CSS Eigeschaft des Elements. 

<p class="tip">Vergiss nicht das `v-show` das `<template>` Element nicht unterstützt und auch nicht mit `v-else` funktioniert.</p>

## `v-if` vs `v-show`

`v-if` macht "richtiges" bedingtes rendering da es sicherstellt das Event-Listener und Kind-Komponenten im bedingten Block ordentlich zerstört und neu erzeugt werden beim umschalten.

Weiterhin ist `v-if` **lazy**: Wenn die Bedingung beim ersten rendering false ist, passiert rein gar nichts - der bedingte Block wird nicht gerendert bis die Bedingung zum ersten Mal true wird.

Im Vergleich dazu ist `v-show` viel einfacher - das Element wird immer gerendert, unabhängig von der Erst-Bedingung und das umschalten basiert auf CSS. 

Allgemein ausgedrückt hat `v-if` viel höhere Umschalt-Kosten während `v-show` beim ersten rendern mehr kostet. Also verwende `v-show` wenn Du etwas sehr oft umschalten musst und `v-if` wenn es sehr unwahrscheinlich ist das sich die Bedingung zur Laufzeit ändert. 

## `v-if` mit `v-for`

<p class="tip">`v-if` zusammen mit `v-for` zu benutzen wird **nicht empfohlen**. Siehe [style guide](/v2/style-guide/#Avoid-v-if-with-v-for-essential) für mehr Informationen.</p>

Wenn `v-if` mit `v-for` benutzt wird hat `v-for` eine höhere Priorität als `v-if`. Siehe <a href="../guide/list.html#v-for-with-v-if">Listen rendering guide</a> für details.
