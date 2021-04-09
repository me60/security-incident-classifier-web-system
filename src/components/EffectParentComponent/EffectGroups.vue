<template>
    <div id="effect_groups" class="effect_groups">

        <!-- EffectGroups make up the body of the form, loop through each-->
        <div v-for="effect_group in effect_groups" :key="effect_group.question_text">
            <h3>{{effect_group.question_text}}</h3>
            <EffectGroup :effect_group="effect_group" :all_intermediates="all_intermediates" />
        </div>
    </div>
</template>

<script>
    import EffectGroup from './EffectGroup.vue'
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
    .effect_groups {
        background: #666699;
        color: #fff;
        text-align: center;
        padding: 10px;
    }
</style>
