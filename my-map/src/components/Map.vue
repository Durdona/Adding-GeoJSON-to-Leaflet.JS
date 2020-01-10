<template>
  <div class="section">
    <div class="columns">
      <div class="column has-background-black-ter"></div>
      <div class="column has-background-grey-darker"></div>
      <div class="column has-background-grey-dark"></div>
      <div class="column has-background-grey-darker"></div>
      <div class="column has-background-black-ter"></div>
    </div>
    <div>
      <h1
        class="title is-1 has-text-centered is-size-desktop has-text-black-bis"
      >
        Interactive Map
      </h1>
    </div>
    <div id="mapid"></div>
  </div>
</template>

<script>
export default {
  name: "Map",
  data() {
    return {};
  },
  methods: {},
  props: {
    msg: String
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.section {
  padding: 0.5rem 0.5rem;
}
</style>
