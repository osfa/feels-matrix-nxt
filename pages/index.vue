<template>
  <div id="app"><FeelsViewer /></div>
</template>

<script>
import AudioModule from '../components/AudioModule.vue'
import FeelsViewer from '../components/FeelsViewer.vue'
// import OpenSeaDragon from '~/components/OpenSeaDragon.vue'

// import Tinder from '../components/vue-tinder/Tinder.vue'
// import Fog from '../components/Fog'
// import SmallClock from '../components/SmallClock'
// import { the330, the156, the52, the84 } from './data/panelLibrary.js'

export const random = (min, max) => {
  min = Math.ceil(min)
  max = Math.floor(max)
  return Math.floor(Math.random() * (max - min) + min)
}

/* eslint-disable */
Array.prototype.sample = function () {
  return this[Math.floor(Math.random() * this.length)]
}
/* eslint-disable */

export default {
  name: 'IndexPage',
  components: { FeelsViewer, 'audio-module': AudioModule },
  data: () => ({}),
  created() {
    // this.$gtag.pageview(this.$route)
  },
  mounted() {},
  methods: {},
}
</script>

<style>
html,
body {
  height: 100%;
  margin: 0;
  padding: 0;
}
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  height: 100vh;
  width: 100vw;
  background: blue;
}
</style>
