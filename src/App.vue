<template>
  <div id="app">
    <blog></blog>
    <router-view/>
  </div>
</template>
<script>
export default {
  data: () => ({
    name: 'App',
    zoom: 2,
    center: [0, 0],
    absolute: true,
    overlay: false,
    drawer: null,
    rotation: 0,
    geolocPosition: undefined
  }),
  props: {
    source: String
  },
  methods: {
    setPositionCurrent () {
      this.center = this.geolocPosition
      this.zoom = 17
    }
  }

}
</script>
