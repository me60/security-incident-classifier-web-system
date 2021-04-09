<template>
    <div id="effect_group">

        <!-- Go through each answer/effect...-->
        <div v-for="effect in effect_group.effects" :key="effect">

            <!-- Start the button group for each question -->
            <div class="btn-group" data-toggle="buttons" :id="`group${effect_group.question_text}`">

            <!-- Find intermediate data associated with the effect-->
                <div v-for="(effect_intermediate, index) in effect_group.effect_intermediate_control" :key="`effect_intermediate-${index}`">

                    <!-- Once the effect is found, its associates
                    class/state/weight effect can be gathered -->
                    <div v-if="effect_intermediate.effect === effect">

                        <!-- Find what kind of intermediate the effect
                        signals-->
                        <div v-for="intermediate in all_intermediates" :key="intermediate.name">
                            <div v-if="intermediate.name === effect_intermediate.affecting_intermediate">
                                <div v-if="intermediate.type === 'inc'">
                                    <Effect :name="effect" :a_i="intermediate.name" :p_q_type="effect_group.type" :p_q_name="effect_group.question_text" :c_class="effect_intermediate.class" :basic_trigger="effect_intermediate.basic_trigger" />
                                </div>
                                <div v-if="intermediate.type === 'count'">
                                    <Effect :name="effect" :a_i="intermediate.name" :p_q_type="effect_group.type" :p_q_name="effect_group.question_text" :weight="effect_intermediate.weight" :basic_trigger="effect_intermediate.basic_trigger" />
                                </div>
                                <div v-if="intermediate.type === 'bin'">
                                    <Effect :name="effect" :a_i="intermediate.name" :p_q_type="effect_group.type" :p_q_name="effect_group.question_text" :state="effect_intermediate.state" :basic_trigger="effect_intermediate.basic_trigger" />
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
    import Effect from './Effect.vue'
    export default {
        name: "EffectGroup",
        props: ["effect_group", "all_intermediates"],
        components: {
            Effect
        }
    }
</script>
