<template>
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
</template>

<style>
</style>

<script>
import axios from 'axios'

const openWeatherMapApiKey = '70978937430be9d1a65977a0ba05784d'
const openWeatherMapUrl = 'https://api.openweathermap.org/data/2.5/forecast'

export default {
  name: 'PageIndex',
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
}
</script>
