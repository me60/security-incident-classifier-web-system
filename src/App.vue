<!-- Abstracted HTML Representation -->
<template>
  <div id="app" class="app">
    <title>Security Incident Classifier</title>
    <Header :classifier_name="classifier_name" />
    <EffectGroups :effect_groups="effect_groups" :all_intermediates="all_intermediates" />
    <Intermediates :all_intermediates="all_intermediates"/>
    <SeverityGroups :severity_groups="severity_groups"/>
  </div>
</template>

<!-- Body of single page -->
<script>
  import EventBus from './event-bus.js';
  import Header from './components/Header.vue'
  import EffectGroups from './components/EffectParentComponent/EffectGroups.vue'
  import Intermediates from './components/IntermediateParentComponent/Intermediates.vue'
  import SeverityGroups from './components/SeverityParentComponent/SeverityGroups.vue'
  export default {
    name: 'App',
    components: {
      Header,
      //Results,
      EffectGroups,
      Intermediates,
      SeverityGroups
    },
    data() {
        return {
            classifier_name : "Nutrition of Calvin Calculator",
            all_intermediates : [
                {
                    name : "calories_controller",
                    type : "count",
                    thresholds : [0,1500,2500],
                    priority : true,
                    severity_system : "calories_reflector_severity"
                },
                {
                    name : "protein_controller",
                    type : "count",
                    thresholds : [0,52,60],
                    priority : true,
                    severity_system : "protein_reflector_severity"
                },
                {
                    name : "carbs_controller",
                    type : "count",
                    thresholds : [0,225,325],
                    priority : true,
                    severity_system : "carbs_reflector_severity"
                },
                {
                    // accidental thomas the tank engine reference...
                    name : "fat_controller",
                    type : "count",
                    thresholds : [0,44,77],
                    priority : true,
                    severity_system : "fat_reflector_severity"
                },
            ],
            effect_groups : [
                {
                    question_text : "Breakfast",
                    type : "onepick",
                    effects : ["Breakfast Wrap", "Coconut-almond Oatmeal", "Eggs and Toast"],
                    intermediates : ["calories_controller", "protein_controller", "carbs_controller", "fat__controller"],
                    // at the moment, the relationship between effect groups
                    // and intermediates is one-to-many, however, here is a
                    // prime example of individual effects hosting multiple,
                    // separate intermediates, i.e. a one-to-many relationship
                    // between *effects* and intermediates, not just effect
                    // groups (which is a more conceptual relationship as it
                    // stands, and is a benefit of the architecture instead of
                    // an implementation detail)
                    // ..
                    // I need to investigate how effects are rendered, and
                    // change that to accommodate this
                    effect_intermediate_control : [
                        {affecting_intermediate : "calories_reflector_severity", effect : "Breakfast Wrap", weight : 601},
                        {affecting_intermediate : "protein_reflector_severity", effect : "Breakfast Wrap", weight : 33},
                        {affecting_intermediate : "carbs_reflector_severity", effect : "Breakfast Wrap", weight : 70},
                        {affecting_intermediate : "fat_reflector_severity", effect : "Breakfast Wrap", weight : 22},
                        {affecting_intermediate : "calories_reflector_severity", effect : "Coconut-almond Oatmeal", weight : 651},
                        {affecting_intermediate : "protein_reflector_severity", effect : "Coconut-almond Oatmeal", weight : 32},
                        {affecting_intermediate : "carbs_reflector_severity", effect : "Coconut-almond Oatmeal", weight : 77},
                        {affecting_intermediate : "fat_reflector_severity", effect : "Coconut-almond Oatmeal", weight : 23},
                        {affecting_intermediate : "calories_reflector_severity", effect : "Eggs and Toast", weight : 567},
                        {affecting_intermediate : "protein_reflector_severity", effect : "Eggs and Toast", weight : 38},
                        {affecting_intermediate : "carbs_reflector_severity", effect : "Eggs and Toast", weight : 60},
                        {affecting_intermediate : "fat_reflector_severity", effect : "Eggs and Toast", weight : 20},
                    ]
                },
                {
                    question_text : "Lunch",
                    type : "onepick",
                    effects : ["Turkey Avocado Wrap", "Salmon Bowl", "Chicken Quinoa Bowl"],
                    intermediates : ["calories_controller", "protein_controller", "carbs_controller", "fat__controller"],
                    effect_intermediate_control : [
                        {affecting_intermediate : "calories_controller", effect : "Turkey Avocado Wrap", weight : 648},
                        {affecting_intermediate : "protein_controller", effect : "Turkey Avocado Wrap", weight : 43},
                        {affecting_intermediate : "carbs_controller", effect : "Turkey Avocado Wrap", weight : 67},
                        {affecting_intermediate : "fat_controller", effect : "Turkey Avocado Wrap", weight : 25},
                        {affecting_intermediate : "calories_controller", effect : "Salmon Bowl", weight : 679},
                        {affecting_intermediate : "protein_controller", effect : "Salmon Bowl", weight : 44},
                        {affecting_intermediate : "carbs_controller", effect : "Salmon Bowl", weight : 70},
                        {affecting_intermediate : "fat_controller", effect : "Salmon Bowl", weight : 27},
                        {affecting_intermediate : "calories_controller", effect : "Chicken Quinoa Bowl", weight : 738},
                        {affecting_intermediate : "protein_controller", effect : "Chicken Quinoa Bowl", weight : 50},
                        {affecting_intermediate : "carbs_controller", effect : "Chicken Quinoa Bowl", weight : 77},
                        {affecting_intermediate : "fat_controller", effect : "Chicken Quinoa Bowl", weight : 28},
                    ]
                },
                {
                    question_text : "Dinner",
                    type : "onepick",
                    effects : ["Shrimp Stir Fry", "Steak and Potato wit Side Salad", "Chicken and Bean Bowl"],
                    intermediates : ["calories_controller", "protein_controller", "carbs_controller", "fat__controller"],
                    effect_intermediate_control : [
                        {affecting_intermediate : "calories_controller", effect : "Shrimp Stir Fry", weight : 678},
                        {affecting_intermediate : "protein_controller", effect : "Shrimp Stir Fry", weight : 43},
                        {affecting_intermediate : "carbs_controller", effect : "Shrimp Stir Fry", weight : 79},
                        {affecting_intermediate : "fat__controller", effect : "Shrimp Stir Fry", weight : 24},
                        {affecting_intermediate : "calories_controller", effect : "Steak and Potato wit Side Salad", weight : 677},
                        {affecting_intermediate : "protein_controller", effect : "Steak and Potato wit Side Salad", weight : 49},
                        {affecting_intermediate : "carbs_controller", effect : "Steak and Potato wit Side Salad", weight : 76},
                        {affecting_intermediate : "fat_controller", effect : "Steak and Potato wit Side Salad", weight : 24},
                        {affecting_intermediate : "calories_controller", effect : "Chicken and Bean Bowl", weight : 784},
                        {affecting_intermediate : "protein_controller", effect : "Chicken and Bean Bowl", weight : 50},
                        {affecting_intermediate : "carbs_controller", effect : "Chicken and Bean Bowl", weight : 81},
                        {affecting_intermediate : "fat_controller", effect : "Chicken and Bean Bowl", weight : 30},
                    ]
                }
            ],
            severity_groups : [
                {
                    name : "calories_reflector_severity",
                    display_name : "Calories (KCal)",
                    type : "reflective",
                    severities : ["Below Target", "Within Target", "Above Target"],
                    intermediates : ["calories_controller"],
                    severity_intermediate_control : [
                        {on_threshold : 0, severity : "Below Target"},
                        {on_threshold : 1500, severity : "Within Target"},
                        {on_threshold : 2500, severity : "Above Target"},
                    ]
                },
                {
                    name : "protein_reflector_severity",
                    display_name : "Protein (g)",
                    type : "reflective",
                    severities : ["Below Target", "Within Target", "Above Target"],
                    intermediates : ["protein_controller"],
                    severity_intermediate_control : [
                        {on_threshold : 0, severity : "Below Target"},
                        {on_threshold : 52, severity : "Within Target"},
                        {on_threshold : 60, severity : "Above Target"},
                    ]
                },
                {
                    name : "carbs_reflector_severity",
                    display_name : "Carbs (g)",
                    type : "reflective",
                    severities : ["Below Target", "Within Target", "Above Target"],
                    intermediates : ["carbs_controller"],
                    severity_intermediate_control : [
                        {on_threshold : 0, severity : "Below Target"},
                        {on_threshold : 225, severity : "Within Target"},
                        {on_threshold : 325, severity : "Above Target"},
                    ]
                },
                {
                    name : "fat_reflector_severity",
                    display_name : "Fat (g)",
                    type : "reflective",
                    severities : ["Below Target", "Within Target", "Above Target"],
                    intermediates : ["fat_controller"],
                    severity_intermediate_control : [
                        {on_threshold : 0, severity : "Below Target"},
                        {on_threshold : 44, severity : "Within Target"},
                        {on_threshold : 77, severity : "Above Target"},
                    ]
                }
            ]
        }
    },
    created: function () {
        // Validate MWOM logically
        this.triggerOtherComponentValidation();
        // Template component registration builds each component
    },
    methods : {
        /**
        * Pass EffectGroup, SeverityGroup and IntermediateGroup data on the
        * EventBus so methods within their components can detect if there's an
        * issue with the proposed MapWeb
        */
        triggerOtherComponentValidation : function () {

            // Validation events emitted on creation
            EventBus.$emit('validateSeverities', this.severity_groups);
            EventBus.$emit('validateIntermediates', this.all_intermediates);
            EventBus.$emit('validateEffectGroups', this.effect_groups);

            /*If the number of severity classes an intermediate has does not
            match the number of severity classes that the connecting effect
            groups are capable of signalling OR If a declared intermediate
            contains classes/binary states/thresholds that are never used*/

            /* Ordering of effect_intermediate_control needs to match the
            ordering of the effects, i.e. if the effects are yes and no, the
            control objects need to be in the order of yes then no*/

            /*"To calculate thresholds for counter intermediates, I'll have to
            iterate over every effect and calculate the sum of the weightings,
            if this is not exactly equal to the largest threshold, throw an
            error"*/

            /*If each effect does not have an associated mapping to an
            intermediate, even if the mapping does not affect an end severity*/

            /*If an intermediate is referenced in an effect-intermediate mapping
            that is not declared as an intermediate used by the effect group*/

            /*If the effect-intermediate mappings for a binary are anything but
            true and false*/

        },
    }
}
</script>

<!-- Global Styling -->
<style>
    .app {
        background-color: #333;
    }
    #app {
        font-family: Avenir, Helvetica, Arial, sans-serif;
        -webkit-font-smoothing: antialiased;
        -moz-osx-font-smoothing: grayscale;
        text-align: center;
        color: #2c3e50;
        margin-top: 60px;
    }
    h3 {
        font-size: 48px;
    }
    h4 {
        font-weight: bold;
        font-style: italic;
    }
    button,radio {
        font-size: 20px;
        background-color: gray;
        border: 2px solid black;
    }
</style>
