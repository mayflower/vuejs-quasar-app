<template>
  <q-page class="flex flex-center">
    <pre>
      Your current position is
      Latitude {{ latitude }}
      Longitude {{ longitude }}
    </pre>
  </q-page>
</template>

<style>
</style>

<script>
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
</script>
