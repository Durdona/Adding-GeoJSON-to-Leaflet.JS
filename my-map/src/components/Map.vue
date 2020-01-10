<template>
  <div>

    <div class="columns">
      <h2 class="column has-background-success">Hellloooo</h2>
      <h2 class="column has-background-warning">Heeeeee</h2>
      <h2 class="column has-background-danger">Heeeeee</h2>
    </div>
    
     <div id="mapid"></div>

  </div>
</template>

<script>

export default {
  name: "Map",
  data (){
      return {
           
      }
  },
  methods: {
       
  },
  props: {
    msg: String
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped></style>
