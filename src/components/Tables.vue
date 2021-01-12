<template>
  <div id="tables">
    <!-- Table names shall be unique -->
    <div v-for="table in tables" v-bind:key="table.name">
      <h3>{{table.name}}</h3>
        <Questions v-bind:questions="table.questions"/>
    </div>
  </div>
</template>

<script>
  import Questions from './Questions.vue'
  export default {
    name: "Tables",
    props: ["tables"],
    components: {
      Questions
    }
  }
</script>

<style>
</style>
