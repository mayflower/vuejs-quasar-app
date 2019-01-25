# 05. Refresh - die Wetterdaten aktualisieren

Eine Kleinigkeit sollten wir noch einbauen: Eine Refresh-Möglichkeit für die App. In der Desktop-Variante zeigen wir dazu einen Refresh-Button an, in der mobilen Variante kommt ein "Pull-to-Refresh" zum Einsatz. Wenn der User also den am Screen seines Smartphones nach unten wischt, aktualisieren sich die Daten.

Hättest Du gedacht, dass Quasar auch hier mit der passenden Komponente zur Stelle ist? Ja, hättest Du und Du hast Recht!

Die Komponente nennt sich ... Trommelwirbel ... [Pull to Refresh](https://quasar-framework.org/components/pull-to-refresh.html). Das kam jetzt überraschend, nicht wahr?

Um diese Komponente verfügbar zu haben, müssen wir sie in der `quasar.config.js` eintragen. Der Bereich `components` sollte bei Dir jetzt also so aussehen:

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
  'QPage',
  'QPullToRefresh',
  'QIcon',
  'QBtn'
]
```

Schon ist die Komponente einsatzbereit. QIcon und QBtn brauchen wir auch gleich!

Jetzt passen wir erst mal die View an:

``` html
<q-page class="flex flex-center">
  <q-pull-to-refresh :handler="updateWeather">
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
  </q-pull-to-refresh>
</q-page>
```

Wischt der User also den Screen nach unten, wird die Methode `updateWeather()` aufgerufen. Wie aber registriert man in VueJS Methoden?

### Das Objekt `methods`

Im Objekt `methods` kann man VueJS-Methoden registrieren, die sowohl von der View, als auch im Logik-Part erreichbar sind.

Also:


``` javascript
data: () => ({
    weather: undefined
  }),
  created () {
    this.updateWeather()
  },
  methods: {
    updateWeather (done) {
      navigator.geolocation.getCurrentPosition(async ({coords}) => {
        try {
          const weatherUrl = `${openWeatherMapUrl}?units=metric&lat=${coords.latitude}&lon=${coords.longitude}&appid=${openWeatherMapApiKey}&lang=de`
          const { data } = await axios.get(weatherUrl)
          this.weather = data
          if (typeof done === 'function') {
            done()
          }
        } catch (e) {
          this.$q.notify('Leider konnte das aktuelle Wetter nicht ermittelt werden.')
          console.error(e)
        }
      })
    }
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
```

Wenn man jetzt das Bild nach unten zieht, erscheint eine Meldung (die Du natürlich jederzeit leicht verändern kannst) und nach dem Loslassen der Maus werden die aktuellen Wetterdaten geholt und angezeigt. Die Verwendung von [`async` und `await`](https://javascript.info/async-await) konnte dazu beitragen die anonymen Funktionen aufzulösen und den Code etwas entschlacken. Die Methode `done()` ist eine Callback-Methode, die dann aufgerufen werden soll, wenn der Aktualisierungsprozess abgeschlossen ist. Da initial kein Callback mitgegeben wird (in der `created()`-Methode), muss dies geprüft werden.


## Button für Browser einblenden

Da die das "Nach unten ziehen" zum Aktualisieren in der Desktop-Umgebung zwar möglich, aber nicht üblich ist, blenden wir im Desktop-Fall einen Button ein. Das geht wie gewohnt sehr einfach und schnell:

``` html
...
<q-card-title slot="overlay">
  {{ weather.name }}
  <span slot="subtitle">
    {{ description }}.
  </span>
  <q-btn class="desktop-only" icon="refresh" color="white" outline round slot="right" @click="updateWeather" />
</q-card-title>
...
```
Tadaaa ... ein Button erscheint. Klickst Du ihn, werden die Daten aktualisiert. Wechselst Du in der Developer-Toolbar Deines Browsers auf "Mobile", verschwindet der Button und man kann die Daten durch "Nach unten ziehen" aktualisieren.


## Ziel erreicht

Das soll es für diesen Schritt gewesen sein. Unsere App zeigt nach dem Starten den aktuellen Standort und die Temperaturen auf einem schönen Hintergrund an. Auch die Höchst- und Tiefsttemperatur, sowie die aktuelle Windgeschwindigkeit wird ausgegeben. Die Daten können aktualisiert werden.

Im nächsten Schritt wollen wir eine richtige mobile App aus unserer kleinen Wetter-Anzeige bauen. Dazu verwendet Quasar [Apache Cordova](https://cordova.apache.org/) als Wrapper, was früher als PhoneGap bekannt war (jetzt aufgespalten in Apache Cordova und Adobe PhoneGap).

Herzlichen Glückwunsch! Wir sehen uns in **06-mobile-app** wieder: `git checkout 06-mobile-app`
