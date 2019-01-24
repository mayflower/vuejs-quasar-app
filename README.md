# 02. Ausgeben der Geo-Location

## Ersetzen des Quasar-Logos mit einer Ausgabe der Geo-Koordinaten

Die kopmlette App-Logik verbirgt sich im `src`-Verzeichnis. Dort finden wir unter `pages` einmal die Error404.vue page, die ausgeliefert wird, wenn unsere App versucht eine Ansicht anzuzeigen, die nicht gefunden werden kann. Danaben ist die **Index.vue** und die modifizieren wir jetzt.

Wie jede VueJS-Komponente ist auch diese aufgeteilt in `<template>` (der HTML-Teil), `<script>` (der Logik-Teil) und `<style>` (dem CSS).
Im `<template>`-Part ersetzen wir einfach das Bild durch folgenden Code:

``` html
<q-page class="flex flex-center">
    <pre>
        Your current position is
        Latitude {{ latitude }}
        Longitude {{ longitude }}
    </pre>
</q-page>
```

Die beiden Platzhalter "latitude" und "longitude" müssen jetzt noch im `<script>`-Part registriert werden. Dank Databinding wird die View sofort aktualisiert, sobald sich der Wert der Platzhalter ändert:

``` javascript
export default {
  name: 'PageIndex',
  data: () => ({
    latitude: 0,
    longitude: 0
  })
}
```

## Die Koordinaten dynamisch ermitteln

Zugegeben, es ist noch nicht wirklich spannend, wenn unsere App 0 und 0 ausgibt. Um die Koordinaten dynamisch zu ermittlen verwenden wir einfach die native Javascript-Methode `navigator.geolocation.getCurrentPosition()`, die uns die aktuelle Position in einem Callbar zurückliefert. 
Dem muss der Benutzer aber erst zustimmen.

## Wann ermitteln wir die Position?

VueJS bietet sogenannte Lifecycle Hooks. Hooks sind schlicht Funktionen, die zu einem bestimmten Zeitpunkt ausgeführt werden und "created" klingt so, als wäre es die richtige Wahl.

Darum ermitteln wir die Position und die View wird automatisch aktualisiert.

``` javascript
export default {
  name: 'PageIndex',
  data: () => ({
    latitude: 0,
    longitude: 0
  }),
  created () {
    navigator.geolocation.getCurrentPosition(({coords}) => {
      this.latitude = coords.latitude
      this.longitude = coords.longitude
    })
  }
}
```

## Ziel erreicht

Das soll es für diesen Schritt gewesen sein. Unsere App startet, ermittelt die Geo-Koordinaten des Browsers und gibt diese dann in der View aus.

Herzlichen Glückwunsch! Wir sehen uns in **03-get-weather** wieder: `git checkout 03-get-weather`