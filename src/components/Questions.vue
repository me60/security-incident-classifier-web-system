<template>
  <div id="question">
    <!-- Question queries shall be unique -->
    <div v-for="question in questions" v-bind:key="question.query">
      <h4>{{question.query}}</h4>
        <Answers v-bind:answers="question.answers"/>
    </div>
  </div>
</template>

<script>
import Answers from './Answers.vue'
  export default {
    name: "Questions",
    props: ["questions"],
    components: {
      Answers
    }
  }
</script>

<style scoped>
</style>
