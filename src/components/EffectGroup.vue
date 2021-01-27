<template>
  <div id="tables">
    <div v-for="effect_group in effect_groups" v-bind:key="effect_group.question_text">
      <h3>{{effect_group.question_text}}</h3>
        <Effect v-bind:effects="effect_group.effects"/>
    </div>
  </div>
</template>

<script>
  import Effect from './Effect.vue'
  export default {
    name: "EffectGroup",
    props: ["effect_groups"],
    components: {
      Effect
    }
    /*,methods: {
      // validation method for effect group checks
    }*/
  }
</script>

<style>
</style>
