<template>
  <div class="column">
    <!-- {{ forecast }} -->
    <ul class="list has-text-justified">
      <li class="list-item" :key="index" v-for="(dataLists, index) in dataList">
        {{ index }}. {{ dataLists.city }}, {{ dataLists.state }},
        {{ dataLists.country }}
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  name: "DataList",
  props: {
    dataList: Array
  }
};
</script>

<style>
ul {
  overflow-y: scroll;
  height: 79vh;
  font-size: 1.2em;
}
ul li:hover {
  background-color: rgb(102, 102, 102);
  color: beige;
}
</style>
