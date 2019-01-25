<template>
  <q-page class="flex flex-center">
    <q-pull-to-refresh :handler="updateWeather">
      <q-spinner v-if="!weather" :size="50" />
      <q-card inline style="max-width: 500px" v-else>
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
            <q-btn class="desktop-only" icon="refresh" color="white" outline round slot="right" @click="updateWeather" />
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
</template>

<script>
import axios from 'axios'

const openWeatherMapApiKey = '70978937430be9d1a65977a0ba05784d'
const openWeatherMapUrl = 'https://api.openweathermap.org/data/2.5/weather'

export default {
  name: 'PageIndex',
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
    kmh: metersPerSecond => Math.round(metersPerSecond / 1000 / (1 / 3600) * 100) / 100
  }
}
</script>
