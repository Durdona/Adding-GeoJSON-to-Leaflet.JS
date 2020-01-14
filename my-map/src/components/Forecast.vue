<template>
  <div class="column">
    <!-- {{ forecast }} -->
    <ul class="list has-text-justified">
      <li class="list-item" :key="index" v-for="(forecasts, index) in forecast">
        {{ index }}. {{ forecasts.city }}, {{ forecasts.country }}
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  name: "Forecast",
  props: {
    forecast: Array
  }
};
</script>

<style>
ul {
  overflow-y: scroll;
  height: 80vh;
}
ul li:hover {
  background-color: rgb(165, 165, 165);
}
</style>
