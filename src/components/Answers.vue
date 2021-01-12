<template>
  <div id="answers">
    <div v-for="answer in answers" v-bind:key="answer.answer_text">
      <button type="button" v-on:click="buttonHandler">{{answer.answer_text}}</button>
    </div>
  </div>
</template>

<script>
  export default {
    name: "Answers",
    props: ["answers"]/*,

    methods: {
      // method that triggers on all button clicks...
      buttonHandler: function (event) {
        // ...for button clicks where the parent question is in the personal data table
        // ...signal event bus with p_data and "yes" if it was yes or "no" if it was no
      }
    }*/
  }
</script>

<style scoped>
</style>
