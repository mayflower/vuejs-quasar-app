<template>
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
</template>

<script>
import axios from 'axios'
import { date } from 'quasar'

const openWeatherMapApiKey = '70978937430be9d1a65977a0ba05784d'
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
</script>
