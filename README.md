# 04. UI, wir hübschen unsere App auf

Quasar bietet eine Menge fertiger [Komponenten](https://quasar-framework.org/components/), die unseren Wetterdaten sofort ein aufgeräumteres Erscheinungsbild spendieren. Außerdem räumen wir alles weg, was unsere App nicht benötigt.

## Lass uns aufräumen!

Ja, mich nervt die Sidebar und Top-Leiste auch. Lass uns beides weg machen, weil wir diese Komponenten mit unserer App nicht benötigen.

Dazu reduziere ich radikal die Layout-Datei `layouts/MyLayout.vue` auf folgenden Inhalt:

``` html
<template>
  <q-layout>
    <q-page-container>
      <router-view />
    </q-page-container>
  </q-layout>
</template>
```

Nachdem die [Quasar-Komponenten](https://quasar-framework.org/components/) nun nicht mehr gebraucht werden, können wir sie aus der `quasar.config.js` werfen. Diese Datei beherbergt sämtliche App übergreifenden Konfigurationen. Nur die hier registrierten Quasar-Komponenten werden letztendlich in die App compiliert. So hast Du es in der Hand, wie groß die Dateigröße Deiner App am Ende ist.

Den Bereich `components` kannst Du mit diesem hier ersetzen:

``` javascript
[
  'QCard',
  'QCardTitle',
  'QCardMain',
  'QCardMedia',
  'QCardSeparator',
  'QLayout',
  'QSpinner',
  'QPageContainer',
  'QPage'
]
```
*Perfekt!*

Für den Hintergrund habe ich ein [Photo](http://dream-wallpaper.com/photography-wallpaper/hd-photography-7-wallpaper/1440x900/free-wallpaper-1.html) in den `assets`-Ordner gelegt. Der Rest geht mit [Quasar-Board-Komponenten](https://quasar-framework.org/components/). Da wären:
1. Eine [Spinner-Komponente](https://quasar-framework.org/components/spinner.html), die so lange angezeigt wird, bis die Wetterdaten geladen wurden. Das ist besonders relevant, wenn wir später dieses Projekt als Smartphone-App kompilieren.
2. Eine [Card-Componente](https://quasar-framework.org/components/card.html), die als Container für Bild und Text dient. Optional sind auch noch Action-Buttons konfigurierbar, aber die benötigt unsere einfache App derzeit nicht.

Da wir die Geo-Koordinaten des Users nicht mehr anzeigen werden, können wir kurzerhand die Platzhalter auf einen einzigen reduzieren: **"weather"**. Unser data-Block sieht daher sehr übersichtlich aus:

``` javascript
data: () => ({
  weather: undefined
}),
```

## Wie blenden wir den Spinner ein, solange die App die Wetterdaten lädt?

VueJS ermöglicht sogenannte conditional renderings. Man kann einem HTML-Element also ganz einfach sagen, "zeige dich nur, wenn Bedingung solange Bedingung x noch nicht eingetroffen ist". In unserem Fall ist der Platzhalter "weather" auf `undefined` gesetzt. Dadurch kann man also schreiben `<q-spinner v-if="!weather" :size="50" />`. Dabei ist das Attribut ":size" die Größe des Spinners in Pixel, wobei durch den vorangestellten Doppelpunkt der Ausdruck in den Anführungszeichen als Javascript interpretiert wird. So wird also die Zahl 50 und nicht der String "50" an den Spinner übergeben.
Jetzt wäre es schön, wenn der Rest der Applikation erst angezeigt wird, wenn die Wetterdaten geladen sind. Quasi
1. Spinner anzeigen
2. Daten laden
3. Daten wurden geladen
4. Spinner ausblenden
5. Card-Komponente mit den ausgefüllten Daten anzeigen

Das kann ganz einfach erreicht werden, indem das nächste DOM-Element nach dem Spinner mit dem Attribut `v-else` ausgestattet wird.

Im Ganzen sieht das dann in Etwa so aus:

``` html
<q-spinner v-if="!weather" :size="50" />
<q-card v-else>
  <p>... Wetterdaten ...</p>
<q-card>
```

Das war ja leicht :)

Eine weiter Hilfestellung bietet uns VueJS mit dem `computed`-Objekt. Diese enthält schlicht eine Liste von Funktionen. Diese Funtionen kann man in der View wiederum wie einen Platzhalter ansprechen.

### Ein Beispiel

``` javascript
computed: {
  currentDate () {
    const ts = new Date();
    return `${ts.getDate()}.${ts.getMonth() + 1}.${ts.getFullYear()}`;
  }
}
```

In der View würde man dann das Datum folgendermaßen ausgeben:

``` html
<p>Heute ist {{ currentDate }}</p>
```
Dies wird oft dazu genutzt, Berechnungen zurück zu geben, sodass in der View lediglich das Ergebnis ausgegeben wird und keine Rechenoperationen nötig sein. So kann eine saubere Kapselung von Logik und Anzeige erreicht werden. Eine Besonderheit ist, dass die Werte, die `computed` zurückliefert aktualisiert werden, sobald sich einer der zur Berechnung notwendigen Operatoren ändern.

### Beispiel, wie sich die View automatisch aktualisiert:

``` html
<p>Der Gesamtpreis beträgt {{ totalPrice }}€</p>
```

``` javascript
export default {
  data: () => ({
    items: [
      { price: 120 }
    ]
  }),
  computed: {
    totalPrice () {
     return this.items.reduce((prev,elem) => prev + elem.price, 0)
    }
  },
  created() {
    setTimeout(() => {
      this.items.push({price: 200})
    }, 4000)
  }
};
```
Hier siehst Du zunächst den Preis von 120 Euro ausgegeben. Nach 4 Sekunden aktualisiert sich dann der Preis, weil die Einkaufsliste (`items`) um einen neuen Artikel für 200 Euro erweitert wurde.

Jetzt haben wir alle Techniken und Komponenten zusammen für das Finale: Das Design der App ansprechender zu gestalten.

## Das finale Design

### Im `<template>`-Part findest Du folgenden Code

``` html
  <q-page class="flex flex-center">
    <q-spinner v-if="!weather" :size="50" />
    <q-card v-else>
      <q-card-media>
        <img src="~assets/background.jpg">
        <h1 slot="overlay" class="q-pl-sm q-mb-none q-mt-none">
          {{ weather.main.temp | round }}°C
        </h1>
        <q-card-title slot="overlay">
          {{ weather.name }}
          <span slot="subtitle">
            {{ description }}.
          </span>
        </q-card-title>
      </q-card-media>
      <q-card-main>
        <div class="row justify-between">
          <div class="col">
            <span class="q-body-1">Temperatur min:</span>
            <span class="q-body-2">{{ weather.main.temp_min | round }}°C</span>
          </div>
          <div class="col">
            <span class="q-body-1">Temperatur max:</span>
            <span class="q-body-2">{{ weather.main.temp_max | round }}°C</span>
          </div>
          <div class="col">
            <span class="q-body-1">Wind:</span>
            <span class="q-body-2">{{ weather.wind.speed | kmh }} km/h</span>
          </div>
        </div>
      </q-card-main>
    </q-card>
  </q-page>
```

### Im `<script>`-Part

``` javascript
import axios from 'axios'
import { date } from 'quasar'

const openWeatherMapApiKey = 'xxxxxxxxxxxxxxxxxxxxxxxxxxxx'
const openWeatherMapUrl = 'https://api.openweathermap.org/data/2.5/weather'
const { formatDate } = date

export default {
  name: 'PageIndex',
  data: () => ({
    weather: undefined
  }),
  created () {
    navigator.geolocation.getCurrentPosition(({coords}) => {
      const weatherUrl = `${openWeatherMapUrl}?units=metric&lat=${coords.latitude}&lon=${coords.longitude}&appid=${openWeatherMapApiKey}&lang=de`
      axios.get(weatherUrl).then(({data}) => {
        this.weather = data
      }).catch(e => {
        this.$q.notify('Leider konnte das aktuelle Wetter nicht ermittelt werden.')
        console.error(e)
      })
    })
  },
  computed: {
    description () {
      return this.weather.weather.map(weather => weather.description).join(', ')
    }
  },
  filters: {
    round: float => Math.round(float),
    localDate: timeString => formatDate(new Date(timeString), 'DD.MMM'),
    localTime: timeString => formatDate(new Date(timeString), 'HH:mm'),
    kmh: metersPerSecond => Math.round(metersPerSecond / 1000 / (1 / 3600) * 100) / 100
  }
}
```
## Ach, fast vergessen .... die [VueJS-Filter](https://vuejs.org/v2/guide/filters.html)

Sicherlich hast Du Dir bereits am Code schon die Funktion der Filter erschließen können. Ähnlich wie beim `computed`-Objekt, handelt es sich hierbei um eine Liste von Methode, die dazu dient, die View-Ausgabe zu formatieren. Sei dies das Umwandeln des Timestamps in das lokale Datumsformat oder die Umwandlung von Meter pro Sekunde in km/h.

## Vorläufiges Fazit

Der Code ist nicht *Clean* und entspricht nicht dem [SOLID-Prinzip](https://en.wikipedia.org/wiki/SOLID). Das bedeutet, dass die Filter zum Beispiel ausgelagert werden können, um an jeder beliebigen Stelle wiederverwendet werden zu können. Auch der Inhalt der `created()`-Methode sollte an einer anderen Stelle platziert werden. So könnte man den das Wetter-Update auch manuell antriggern, ohne die Seite neu laden zu müssen. 

Die Konfiguaration sollte auch in eine gesonderte `config`-Datei ausgelagert werden, weil Zugangsdaten zentral verwaltet werden sollten, bzw. aus Sicherheitsgründen oft von Außerhalb in die Komponente gereicht werden, sodass der Quellcode der Komponente völlig ohne solcher Daten auskommt.


## Ziel erreicht

Das soll es für diesen Schritt gewesen sein. Unsere App zeigt nach dem Starten den aktuellen Standort und die Temperaturen auf einem schönen Hintergrund an. Auch die Höchst- und Tiefsttemperatur, sowie die aktuelle Windgeschwindigkeit wird ausgegeben.

Herzlichen Glückwunsch! Wir sehen uns in **05-refresh** wieder: `git checkout 05-refresh`