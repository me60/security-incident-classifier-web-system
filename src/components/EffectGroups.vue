<template>
    <div id="tables">

        <!-- EffectGroups make up the body of the form, loop through each-->
        <div v-for="effect_group in effect_groups" :key="effect_group.question_text">
            <h3>{{effect_group.question_text}}</h3>
            <EffectGroup :effect_group="effect_group" :all_intermediates="all_intermediates" />
        </div>
    </div>
</template>

<script>
    // Imports
    //import EventBus from '../event-bus.js';
    import EffectGroup from './EffectGroup.vue'

    /* Events
    EventBus.$on('buildForm', effect_groups =>  {
        buildForm(effect_groups);
    });

    // Functions
    function buildForm(effect_groups) {
        //this.effect_groups = effect_groups;
    } // buildForm*/

    export default {
        name: "EffectGroups",
        props: ["effect_groups", "all_intermediates"],
        components: {
            EffectGroup
        },
        data() {
            return {
                i : 0
            }
        },
        methods: {

            // validation method for effect group checks

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
