<!-- Abstracted HTML Representation -->
<template>
  <div id="app">
    <Header />
    <EffectGroups :effect_groups="effect_groups" :all_intermediates="all_intermediates" />
    <Intermediate />
    <SeverityGroup />
    <Results />
  </div>
</template>

<!-- Body of single page -->
<script>
  import EventBus from './event-bus.js';
  //import Tables from './components/Tables.vue'
  import Header from './components/Header.vue'
  import Results from './components/Results.vue'
  import EffectGroups from './components/EffectGroups.vue'
  import Intermediate from './components/Intermediate.vue'
  import SeverityGroup from './components/SeverityGroup.vue'
  export default {
    name: 'App',
    components: {
      //Tables,
      Header,
      Results,
      EffectGroups,
      Intermediate,
      SeverityGroup
    },
    data() {
        return {
            all_intermediates : [
                {
                    name : "amount_severity_controller",
                    type : "inc",
                    classes : 3,
                    priority : true
                },
                {
                    name : "covering_severity_controller",
                    type : "inc",
                    classes : 3,
                    priority : true
                },
                {
                    name : "ant_colour_controller",
                    type : "bin",
                    priority : true
                },
                {
                    name : "place_dropped_controller",
                    type : "count",
                    thresholds : [1,2,3],
                    priority : true
                }
            ],
            effect_groups : [{
                    question_text : "How much bread was dropped?",
                    type : "onepick",
                    effects : ["Crumbs","Bits","Slice","Loaf"],
                    intermediates : ["amount_severity_controller"],
                    effect_intermediate_control : [
                        {affecting_intermediate : "amount_severity_controller", effect : "Crumbs", class : 1},
                        {affecting_intermediate : "amount_severity_controller", effect : "Bits", class : 1},
                        {affecting_intermediate : "amount_severity_controller", effect : "Slice", class : 2},
                        {affecting_intermediate : "amount_severity_controller", effect : "Loaf", class : 2}
                    ]
                },
                {
                    question_text : "What was on the bread?",
                    type : "onepick",
                    effects : ["Nothing","Butter","Jam"],
                    intermediates : ["covering_severity_controller"],
                    effect_intermediate_control : [
                        {affecting_intermediate : "covering_severity_controller", effect : "Nothing", class : 1},
                        {affecting_intermediate : "covering_severity_controller", effect : "Butter", class : 2},
                        {affecting_intermediate : "covering_severity_controller", effect : "Jam", class : 3}
                    ]
                },
                {
                    question_text : "Are the ants red?",
                    type : "onepick",
                    effects : ["Yes","No"],
                    intermediates : ["ant_colour_controller"],
                    effect_intermediate_control : [
                        {affecting_intermediate : "ant_colour_controller", effect : "Yes", state : true},
                        {affecting_intermediate : "ant_colour_controller", effect : "No", state : false}
                    ]
                },
                {
                    question_text : "Where was the bread dropped?",
                    type : "multipick",
                    effects : ["Kitchen","Bathroom","Bedroom"],
                    intermediates : ["place_dropped_controller"],
                    effect_intermediate_control : [
                        {affecting_intermediate : "place_dropped_controller", effect : "Kitchen", weight : 1},
                        {affecting_intermediate : "place_dropped_controller", effect : "Bathroom", weight : 1},
                        {affecting_intermediate : "place_dropped_controller", effect : "Bedroom", weight : 1}
                    ]
                }
            ],
            severity_groups : [
                {
                    name : "general_severity",
                    type : "exclusive",
                    severities : ["Not Severe","Pretty Bad","Infestation Likely"],
                    intermediates : ["amount_severity_controller","covering_severity_controller"],
                    severity_intermediate_control : [
                        {class_trigger: 1, severity : "Not Severe"},
                        {class_trigger: 2, severity : "Pretty Bad"},
                        {class_trigger: 3, severity : "Infestation Likely"}
                    ]
                },
                {
                    name : "ant_type_severity",
                    type : "exclusive",
                    severities : ["Normal Ants","Fire Ants!"],
                    intermediates : ["ant_colour_controller"],
                    severity_intermediate_control : [
                        {bin_trigger : true, severity : "Fire Ants!"},
                        {bin_trigger : false, severity : "Normal Ants"}
                    ]
                },
                {
                    name : "rooms_severity",
                    type : "exclusive",
                    severities : ["1 Room Affected","2 Rooms Affected","3 Rooms Affected"],
                    intermediates : ["place_dropped_controller"],
                    severity_intermediate_control : [
                        {on_threshold : 1, severity : "1 Room Affected"},
                        {on_threshold : 2, severity : "2 Rooms Affected"},
                        {on_threshold : 3, severity : "3 Rooms Affected"}
                    ]
                }
            ]
        }
    },
    created: function () {
        this.triggerOtherComponentValidation();
        this.triggerFormBuild();
    },
    methods : {
        /**
        * Pass EffectGroup, SeverityGroup and IntermediateGroup data on the
        * EventBus so methods within their components can detect if there's an
        * issue with the proposed MapWeb
        */
        triggerOtherComponentValidation : function () {
            console.log("hi");
            EventBus.$emit('validateIntermediates', this.all_intermediates);
            console.log("hi");
            EventBus.$emit('validateEffectGroups', this.effect_groups);
            EventBus.$emit('validateSeverities', this.severity_groups);

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

        }, // triggerValidation

        /**
         * Sends an event along the event bus that specifies for the form to be
         * built - this includes only the questions and answers to MWOM
         * configured questions
         */
        triggerFormBuild : function () {
            console.log("Autoconfiguring questions and answer buttons etc.");
            console.log("checking changes.................");
            EventBus.$emit('buildForm', this.effect_groups);
        } // triggerFormBuild
    }
  }
</script>

<!-- Global Styling -->
<style>
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
