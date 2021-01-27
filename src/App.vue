<!-- Abstracted HTML Representation -->
<template>
  <div id="app">
    <Header />
    <EffectGroup v-bind:effect_groups="effect_groups"/>
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
  import EffectGroup from './components/EffectGroup.vue'
  import Intermediate from './components/Intermediate.vue'
  import SeverityGroup from './components/SeverityGroup.vue'
  var Enum_Question_Type = {
      YES_NO : "yes_no",
      ONE_PICK : "one_pick",
      MULTI_PICK : "multi_pick"
  };
  var Enum_Intermediate_Type = {
      INCREMENT : "increment",
      BINARY : "binary",
      COUNTER : "counter"
  };
  var Enum_Severity_Type = {
      INCLUSIVE : "inclusive",
      EXCLUSIVE : "exclusive"
  };
  export default {
    name: 'App',
    components: {
      //Tables,
      Header,
      Results,
      EffectGroup,
      Intermediate,
      SeverityGroup
    },
    data() {
        return {
            // Define the enums used for the MapWeb
            inc : Enum_Intermediate_Type.INCREMENT,
            bin : Enum_Intermediate_Type.BINARY,
            count : Enum_Intermediate_Type.COUNTER,
            yesno : Enum_Question_Type.YES_NO,
            onepick : Enum_Question_Type.ONE_PICK,
            multipick : Enum_Question_Type.MULTI_PICK,
            exclusive : Enum_Severity_Type.EXCLUSIVE,
            Enum_Intermediate_Type,
            Enum_Question_Type,
            Enum_Severity_Type,
            all_intermediates : [
                {
                    name : "amount_severity_controller",
                    type : this.inc,
                    classes : 3,
                    priority : true
                },
                {
                    name : "covering_severity_controller",
                    type : this.inc,
                    classes : 3,
                    priority : true
                },
                {
                    name : "ant_colour_controller",
                    type : this.bin,
                    priority : true
                },
                {
                    name : "place_dropped_controller",
                    type : this.count,
                    thresholds : [1,2,3],
                    priority : true
                }
            ],
            effect_groups : [{
                    question_text : "How much bread was dropped?",
                    type : this.onepick,
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
                    type : this.onepick,
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
                    type : this.yesno,
                    intermediates : ["ant_colour_controller"],
                    effect_intermediate_control : [
                        {affecting_intermediate : "ant_colour_controller", effect : "Yes", state : true},
                        {affecting_intermediate : "ant_colour_controller", effect : "No", state : false}
                    ]
                },
                {
                    question_text : "Where was the bread dropped?",
                    type : this.multipick,
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
                    type : this.exclusive,
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
                    type : this.exclusive,
                    severities : ["Normal Ants","Fire Ants!"],
                    intermediates : ["ant_colour_controller"],
                    severity_intermediate_control : [
                        {bin_trigger : true, severity : "Fire Ants!"},
                        {bin_trigger : false, severity : "Normal Ants"}
                    ]
                },
                {
                    name : "rooms_severity",
                    type : this.exclusive,
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
        this.triggerOtherComponentValidation()
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
        } // triggerValidation
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
</style>
