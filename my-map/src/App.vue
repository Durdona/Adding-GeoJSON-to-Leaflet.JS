<template>
  <div id="app">
    <Map/> 
  </div>
</template>

<script>
import Map from './components/Map.vue' 


export default {
  name: "app",
  components: {
    Map
  }
}
</script>

<style>
 
#app {
 font-family: 'Jura', sans-serif;  
 letter-spacing: 0.02em;
}
</style>
