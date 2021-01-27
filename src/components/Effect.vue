<template>
  <div id="effects">
    <div v-for="effect in effects" v-bind:key="effect">
        <button type="button">{{effect}}</button>
    </div>
  </div>
</template>

<script>
  //<button type="button" v-on:click="buttonHandler">{{effect}}</button>
  export default {
    name: "Effect",
    props: ["effects"]/*,

    methods: {
      // method that triggers on all button clicks...
      buttonHandler: function (event) {
        // ...for button clicks where the parent question is in the personal data table
        // ...signal event bus with p_data and "yes" if it was yes or "no" if it was no
      }
    }*/
  }
</script>

<style>
</style>
