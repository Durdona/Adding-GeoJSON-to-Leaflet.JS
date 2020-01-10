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
  margin-top: 40px;
}
</style>
