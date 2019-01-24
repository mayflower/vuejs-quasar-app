# 02. Holen der Wetter-Daten

## Vorbereitung

Um das Wetter für unseren Standort zu ermitteln, schicken wir die Koordinaten zu einem kostenlosen Wetterdienst, der unsere Anfrage prompt mit einem JSON beantworetet. Darin enhalten sind die Wetterdaten der nächsten 5 Tage in 3-Stunden-Schritten, die wir aufbereitet ausgeben wollen.

**Schritt 1**: Hole Dir einen kostenlosen API-Key bei [OpenWeatherMap](https://openweathermap.org/appid) (damit fragen wir die Wetterdaten ab)

Erfahrungsgemäß dauert die Freischaltung eines API-Keys gerne mal eine halbe Stunde, wenn nicht länger(!). Also hol' Dir den Key sofort, bevor Du weiter machst, damit Du später nicht unnötig warten musst.

**Schritt 2**: Um HTTP-Requests zu machen, bedienen wir uns einer kleinen Library namens [Axios](https://github.com/axios/axios). Diese vereinheitlicht und vereinfacht unsere Arbeit. Genauso könntest Du aber auch den nativen `fetch`-Befehl von Javascript verwenden, wenn Du sicherstellen kannst, dass dieser in den [Browsern Deiner Anwender funktioniert](https://caniuse.com/#search=fetch).

## Umsetzung

Neben den Wetter-Daten liefert uns OpenWeatherMap netterweise auch noch die Stadt mit, in der wir uns befinden. Diese werden wir dem User auch direkt anzeigen.
Du ahnst es sicherlich schon: Wir brauchen dazu neue Platzhalter und ein wenig Logik.

Zunächst registriern wir ein Platzhalter namens `locality` im `data`-Bereich. Du kannst ihn mit `undefined`, `null`, oder einem leeren String vorbefüllen. Das Template sollte den Platzhalter auch ausgeben. Sie sieht das bei mir dann aus:

``` html
<q-page padding>
  <pre>
    Your location: {{ locality }}
    Latitude {{ latitude }}
    Longitude {{ longitude }}
  </pre>
</q-page>
```

... und im Script-Bereich:

``` javascript
export default {
  name: 'PageIndex',
  data: () => ({
    latitude: 0,
    longitude: 0,
    locality: undefined
  }),
  created () {
    navigator.geolocation.getCurrentPosition(({coords}) => {
      this.locality = 'No clue, yet'
      this.latitude = coords.latitude
      this.longitude = coords.longitude
    })
  }
}
```
## Lass uns das Wetter holen!

Jetzt ist der Zeitpunkt, zu dem Axios zum Einsatz kommt und die Wetterdaten von OpenWeatherMap besorgt. Dazu habe ich im `<script>`-Part zunächst die axios-Library importiert und zwei Konstanten definiert. Zum einen den API-Key für OpenWeatherMap, zum anderen den API-Endpunkt für unsere Wetterdaten. Axios liefert eine [Promise](https://developer.mozilla.org/de/docs/Web/JavaScript/Reference/Global_Objects/Promise) zurück, in der sich die Daten befinden. Den API-Key solltest Du natürlich mit Deinem ersetzen:

``` javascript
import axios from 'axios'

const openWeatherMapApiKey = 'xxxxxxxxxxxxxxxxxxxxxxxxx'
const openWeatherMapUrl = 'https://api.openweathermap.org/data/2.5/forecast'

export default {
  name: 'PageIndex',
  data: () => ({
    latitude: 0,
    longitude: 0,
    locality: undefined
  }),
  created () {
    navigator.geolocation.getCurrentPosition(({coords}) => {
      const weatherUrl = `${openWeatherMapUrl}?units=metric&lat=${coords.latitude}&lon=${coords.longitude}&appid=${openWeatherMapApiKey}`
      axios.get(weatherUrl).then(({data}) => {
        this.locality = data.city.name
        this.latitude = coords.latitude
        this.longitude = coords.longitude
      }).catch(e => {
        this.$q.notify('Leider konnte das aktuelle Wetter nicht ermittelt werden.')
        console.error(e)
      })
    })
  }
}
```

Im Fehlerfall geben wir eine unspezifische Fehlermeldung mittels [Notify](https://quasar-framework.org/components/notify.html) aus, im Erfolgsfall befüllen wir die Platzhalter.

## Temperaturen ausgeben

So sind wir dem Ziel schon zum Greifen nahe. Die zurückgeliferten Daten sind sehr umfangreich, aber auch ebenso [gut dokumentiert](https://openweathermap.org/forecast5). Im ersten Schritt geben wir einfach die Temperaturwerte aus. Dazu registrieren wir einen weitern Platzhalter, den ich in meinem Fall schlicht `weather` nenne. In diesem wird die Liste an Wetterdaten dann abgelegt, um diese in der View dann auszugeben.

Also:

``` javascript
...
  data: () => ({
    latitude: 0,
    longitude: 0,
    locality: undefined,
    weather: []
  }),
  created () {
    navigator.geolocation.getCurrentPosition(({coords}) => {
      const weatherUrl = `${openWeatherMapUrl}?units=metric&lat=${coords.latitude}&lon=${coords.longitude}&appid=${openWeatherMapApiKey}`
      axios.get(weatherUrl).then(({data}) => {
        this.locality = data.city.name
        this.latitude = coords.latitude
        this.longitude = coords.longitude
        this.weather = data.list
      }).catch(e => {
        this.$q.notify('Leider konnte das aktuelle Wetter nicht ermittelt werden.')
        console.error(e)
      })
    })
  }
  ...
}
```
und

``` html
<q-page padding>
<pre>
  Your location: {{ locality }}
  Latitude {{ latitude }}
  Longitude {{ longitude }}
</pre>
<h6>Temperatures</h6>
<span v-for="item in weather" :key="item.dt">
 {{ item.main.temp }} °C
</span>
</q-page>
```

## Ziel erreicht

Das soll es für diesen Schritt gewesen sein. Unsere App zeigt nach dem Starten den aktuellen Standort und die Temperaturen der nächsten 5 Tage an. Schön ist die Ausgabe noch nicht, aber das gehen wir im nächsten Branch an und verwenden ein paar der Quasar-Komponenten.

Herzlichen Glückwunsch! Wir sehen uns in **04-ui** wieder: `git checkout 04-ui`