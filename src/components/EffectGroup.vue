<template>
    <div id="tables">

        <!-- EffectGroups make up the body of the form, loop through each-->
        <div v-for="effect_group in effect_groups" :key="effect_group.question_text">
            <h3>{{effect_group.question_text}}</h3>

            <!-- Go through each answer/effect...-->
            <div v-for="effect in effect_group.effects" :key="effect">
                <div class="btn-group" data-toggle="buttons">

                    <!-- Find intermediate data associated with the effect-->
                    <div v-for="(effect_intermediate, index) in effect_group.effect_intermediate_control" v-bind:key="`effect_intermediate-${index}`">
                        <div v-if="effect_intermediate.effect === effect">

                            <!-- Find what kind of intermediate the effect
                            signals-->
                            <div v-for="intermediate in all_intermediates" v-bind:key="intermediate.name">
                                <div v-if="intermediate.name === effect_intermediate.affecting_intermediate">
                                    <div v-if="intermediate.type === 'inc'">
                                        <Effect v-bind:name="effect" v-bind:a_i="intermediate.name" v-bind:p_q_type="effect_group.type" v-bind:class="effect_intermediate.class" />
                                    </div>
                                    <div v-if="intermediate.type === 'count'">
                                        <Effect v-bind:name="effect" v-bind:a_i="intermediate.name" v-bind:p_q_type="effect_group.type" v-bind:weight="effect_intermediate.weight" />
                                    </div>
                                    <div v-if="intermediate.type === 'bin'">
                                        <Effect v-bind:name="effect" v-bind:a_i="intermediate.name" v-bind:p_q_type="effect_group.type" v-bind:state="effect_intermediate.state" />
                                    </div>
                                </div>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
    // Imports
    //import EventBus from '../event-bus.js';
    import Effect from './Effect.vue'

    /* Events
    EventBus.$on('buildForm', effect_groups =>  {
        buildForm(effect_groups);
    });

    // Functions
    function buildForm(effect_groups) {
        //this.effect_groups = effect_groups;
    } // buildForm*/

    export default {
        name: "EffectGroup",
        props: ["effect_groups", "all_intermediates"],
        components: {
            Effect
        },
        data() {
            return {
                i : 0
            }
        },
        methods: {

            // validation method for effect group checks

            /**
             * Required for gathering the current stage of iteration within the
             * loop so parent attributes of an effect can be gathered
             */
            loopIndicesFetch : function(index) {
                return index+1;
            }, // loopIndicesFetch

            /**
             * We can't use console commands in-html so we have to call on a
             * function for js to do it for us
             */
            debug : function (i) {
                i += 1;
                console.log("!!! " + (i) + " !!!");
            } // debug
        } // methods
    } // exportDefault
</script>

<style>
</style>
