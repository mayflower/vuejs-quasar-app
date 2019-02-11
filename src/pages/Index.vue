<template>
  <q-page padding>
    <pre>
      Your location: {{ locality }}
      Latitude {{ latitude }}
      Longitude {{ longitude }}
      Temperature {{ temperature }} °C
    </pre>
  </q-page>
</template>

<script>
const openWeatherMapApiKey = 'xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx'
const openWeatherMapUrl = 'https://api.openweathermap.org/data/2.5/weather'

export default {
  name: 'PageIndex',
  data: () => ({
    latitude: 0,
    longitude: 0,
    locality: undefined,
    temperature: undefined
  }),
  created () {
    navigator.geolocation.getCurrentPosition(({coords}) => {
      const weatherUrl = `${openWeatherMapUrl}?units=metric&lat=${coords.latitude}&lon=${coords.longitude}&appid=${openWeatherMapApiKey}`
      this.$axios.get(weatherUrl).then(({data}) => {
        this.locality = data.name
        this.latitude = coords.latitude
        this.longitude = coords.longitude
        this.temperature = data.main.temp
      }).catch(e => {
        this.$q.notify('Leider konnte das aktuelle Wetter nicht ermittelt werden.')
        console.error(e)
      })
    })
  }
}
</script>
