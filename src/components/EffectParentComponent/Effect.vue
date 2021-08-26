<template>
      <div id="effects">
          <div v-if="c_class != null">
              <IncrementEffect :c_class="c_class" :p_q_type="p_q_type" :p_q_name="p_q_name" :a_i="a_i" :name="name" :basic_trigger="basic_trigger" />
          </div>
          <div v-if="state != null">
              <BinaryEffect :state="state" :p_q_type="p_q_type" :p_q_name="p_q_name" :a_i="a_i" :name="name" :basic_trigger="basic_trigger" />
          </div>
          <div v-if="weight != null">
             <CounterEffect :weight="weight" :p_q_type="p_q_type" :p_q_name="p_q_name" :a_i="a_i" :name="name" :basic_trigger="basic_trigger" />
          </div>
          <div v-if="contribution != null">
             <CodeBlockEffect :contribution="contribution" :p_q_type="p_q_type" :p_q_name="p_q_name" :a_i="a_i" :name="name" :basic_trigger="basic_trigger" />
          </div>
      </div>
</template>

<script>
    import IncrementEffect from './IncrementEffect.vue';
    import BinaryEffect from './BinaryEffect.vue';
    import CounterEffect from './CounterEffect.vue';
    import CodeBlockEffect from './CodeBlockEffect.vue';
    import EventBus from '../../event-bus.js';
    export default {
        name: "Effect",
        // a_i = affecting_intermediate, p_q_type = parent question type
        // p_q_name = parent question name

        // TODO: have a_i be a_i*s*

        props: ["name", "a_i", "p_q_type", "p_q_name", "c_class", "state", "weight", "contribution", "basic_trigger"],
        components: {
            IncrementEffect,
            BinaryEffect,
            CounterEffect,
            CodeBlockEffect
        },
        methods: {

            // TODO: implement getEmitEventName*s* instead

            // TODO: implement a method to add an a_i to the a_is, this will be
            // called upon by EffectGroup.vue when it has encountered a
            // duplicate effect, but a different affecting_intermediate
            // ..
            // this will be an EventBus.$on method

            debug : function () {
                console.log("name: " + this.name + " a_i: " + this.a_i + " p_q_type: " + this.p_q_type + " c_class: " + this.c_class + " state: " + this.state + " weight: " + this.weight + " contribution: " + this.contribution + " basic trigger: " + this.basic_trigger);
            },
            getEmitEventName : function(activation) {
                let actString = activation ? "activate" : "deactivate";
                return this.a_i + "." + actString;
            },
            getPayload : function() {
                if (this.c_class != null) {
                    return this.p_q_name + "." + this.name + "." + this.c_class;
                }
                if (this.weight != null) {
                    return this.p_q_name + "." + this.name + "." + this.weight;
                }
                if (this.state != null) {
                    return this.p_q_name + "." + this.name + "." + this.state;
                }
                if (this.contribution != null) {
                    return this.p_q_name + "." + this.name + "." + this.contribution;
                }
            }
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
