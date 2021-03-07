<template>
      <div id="effects">
          <div v-if="c_class" != null>
              <IncrementEffect v-bind:c_class="c_class" v-bind:p_q_type="p_q_type" v-bind:a_i="a_i" v-bind:name="name" />
          </div>
          <div v-if="state" != null>
              <BinaryEffect v-bind:state="state" v-bind:p_q_type="p_q_type" v-bind:a_i="a_i" v-bind:name="name" />
          </div>
          <div v-if="weight" != null>
              <CounterEffect v-bind:weight="weight" v-bind:p_q_type="p_q_type" v-bind:a_i="a_i" v-bind:name="name" />
          </div>
      </div>
</template>

<script>
    import IncrementEffect from './IncrementEffect.vue';
    import BinaryEffect from './BinaryEffect.vue';
    import CounterEffect from './CounterEffect.vue'
    export default {
        name: "Effect",
        // a_i = affecting_intermediate, p_q_type = parent question type
        props: ["name", "a_i", "p_q_type", "c_class", "state", "weight"],
        components: {
            IncrementEffect,
            BinaryEffect,
            CounterEffect
        }
    }
</script>

<style>
    #multipick {
        background-color: blue;
    }
    #multipick:active {
        background-color: red;
    }
    #multipick:visited {
        background-color: red;
    }
</style>
