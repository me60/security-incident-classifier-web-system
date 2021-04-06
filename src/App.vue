<!-- Abstracted HTML Representation -->
<template>
  <div id="app" class="app">
    <title>Security Incident Classifier</title>
    <Header />
    <EffectGroups :effect_groups="effect_groups" :all_intermediates="all_intermediates" />
    <Intermediates :all_intermediates="all_intermediates"/>
    <SeverityGroups :severity_groups="severity_groups"/>
  </div>
</template>

<!-- Body of single page -->
<script>
  import EventBus from './event-bus.js';
  //import Tables from './components/Tables.vue'
  import Header from './components/Header.vue'
  //import Results from './components/Results.vue'
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
            all_intermediates : [
                {
                    name : "university_confidential_data_controller",
                    type : "inc",
                    classes : 4,
                    priority : true,
                    severity_system: "university_confidential_data_severity",
                    other_intermediate: "risk_heat_map_controller",
                    intermediate_intermediate_control : [
                        {affecting_intermediate : "risk_heat_map_controller", class_trigger: 1, weight : 0},
                        {affecting_intermediate : "risk_heat_map_controller", class_trigger: 2, weight : 1},
                        {affecting_intermediate : "risk_heat_map_controller", class_trigger: 3, weight : 2},
                        {affecting_intermediate : "risk_heat_map_controller", class_trigger: 4, weight : 3}
                    ]
                },
                {
                    name : "personal_data_controller",
                    type : "bin",
                    priority : true,
                    severity_system: "personal_data_severity",
                    other_intermediate: "risk_heat_map_controller",
                    intermediate_intermediate_control : [
                        {affecting_intermediate : "risk_heat_map_controller", bin_trigger: true, weight : 5},
                        {affecting_intermediate : "risk_heat_map_controller", bin_trigger: false, weight : 0}
                    ]
                },
                {
                    name : "risk_heat_map_controller",
                    type : "count",
                    // Thresholds should always have a 0
                    thresholds : [0,1,5,9,13],
                    priority : true,
                    severity_system: "risk_heat_map_severity",
                    intermediate_intermediate_control : [
                        {}
                    ]
                }
            ],
            effect_groups : [{
                    question_text : "Incident Classification",
                    type : "multipick",
                    effects : ["External Investigation","Malicious Code","Internal Investigation","Copyright Infrigement","Denial of Service","Unauthorised Access","APT","Social","Threat/Extortion/Blackmail"],
                    intermediates : ["risk_heat_map_controller"],
                    effect_intermediate_control : [
                        {affecting_intermediate : "risk_heat_map_controller", effect : "External Investigation", weight : 0},
                        {affecting_intermediate : "risk_heat_map_controller", effect : "Malicious Code", weight : 0},
                        {affecting_intermediate : "risk_heat_map_controller", effect : "Internal Investigation", weight : 0},
                        {affecting_intermediate : "risk_heat_map_controller", effect : "Copyright Infrigement", weight : 0},
                        {affecting_intermediate : "risk_heat_map_controller", effect : "Denial of Service", weight : 0},
                        {affecting_intermediate : "risk_heat_map_controller", effect : "Unauthorised Access", weight : 0},
                        {affecting_intermediate : "risk_heat_map_controller", effect : "APT", weight : 0},
                        {affecting_intermediate : "risk_heat_map_controller", effect : "Social", weight : 0},
                        {affecting_intermediate : "risk_heat_map_controller", effect : "Threat/Extortion/Blackmail", weight : 0}
                    ]
                },
                {
                    question_text : "Are we being asked to provide investigation data to an external party?",
                    type : "onepick",
                    effects : ["n/a or don't know","Yes","No"],
                    intermediates : ["risk_heat_map_controller"],
                    effect_intermediate_control : [
                        {affecting_intermediate : "risk_heat_map_controller", effect : "n/a or don't know", weight : 0},
                        {affecting_intermediate : "risk_heat_map_controller", effect : "Yes", weight : 1},
                        {affecting_intermediate : "risk_heat_map_controller", effect : "No", weight : 0}
                    ]
                },
                {
                    question_text : "Is the incident ongoing or over?",
                    type : "onepick",
                    effects : ["Over","Ongoing","Unknown"],
                    intermediates : ["risk_heat_map_controller"],
                    effect_intermediate_control : [
                        {affecting_intermediate : "risk_heat_map_controller", effect : "Over", weight : 0},
                        {affecting_intermediate : "risk_heat_map_controller", effect : "Unknown", weight : 2},
                        {affecting_intermediate : "risk_heat_map_controller", effect : "Ongoing", weight : 1}
                    ]
                },
                {
                    question_text : "Have we lost access to data?",
                    type : "onepick",
                    effects : ["n/a or don't know","Yes","No"],
                    intermediates : ["risk_heat_map_controller"],
                    effect_intermediate_control : [
                        {affecting_intermediate : "risk_heat_map_controller", effect : "n/a or don't know", weight : 0},
                        {affecting_intermediate : "risk_heat_map_controller", effect : "Yes", weight : 1},
                        {affecting_intermediate : "risk_heat_map_controller", effect : "No", weight : 0}
                    ]
                },
                {
                    question_text : "Are back-ups available?",
                    type : "onepick",
                    effects : ["n/a or don't know","Yes","No"],
                    intermediates : ["risk_heat_map_controller"],
                    effect_intermediate_control : [
                        {affecting_intermediate : "risk_heat_map_controller", effect : "n/a or don't know", weight : 0},
                        {affecting_intermediate : "risk_heat_map_controller", effect : "Yes", weight : 0},
                        {affecting_intermediate : "risk_heat_map_controller", effect : "No", weight : 1}
                    ]
                },
                {
                    question_text : "Is this a critical time in the calendar for those involved?",
                    type : "onepick",
                    effects : ["n/a or don't know","Yes","No"],
                    intermediates : ["risk_heat_map_controller"],
                    effect_intermediate_control : [
                        {affecting_intermediate : "risk_heat_map_controller", effect : "n/a or don't know", weight : 0},
                        {affecting_intermediate : "risk_heat_map_controller", effect : "Yes", weight : 1},
                        {affecting_intermediate : "risk_heat_map_controller", effect : "No", weight : 0}
                    ]
                },
                {
                    question_text : "If a system is affected, can business continue without it?",
                    type : "onepick",
                    effects : ["n/a or don't know","Yes","No"],
                    intermediates : ["risk_heat_map_controller"],
                    effect_intermediate_control : [
                        {affecting_intermediate : "risk_heat_map_controller", effect : "n/a or don't know", weight : 0},
                        {affecting_intermediate : "risk_heat_map_controller", effect : "Yes", weight : 0},
                        {affecting_intermediate : "risk_heat_map_controller", effect : "No", weight : 1}
                    ]
                },
                {
                    question_text : "If a system is affected, is the system critical to the functioning of the Uni?",
                    type : "onepick",
                    effects : ["n/a or don't know","Yes","No"],
                    intermediates : ["risk_heat_map_controller"],
                    effect_intermediate_control : [
                        {affecting_intermediate : "risk_heat_map_controller", effect : "n/a or don't know", weight : 0},
                        {affecting_intermediate : "risk_heat_map_controller", effect : "Yes", weight : 1},
                        {affecting_intermediate : "risk_heat_map_controller", effect : "No", weight : 0}
                    ]
                },
                {
                    question_text : "Does this have potential to spread automatically?",
                    type : "onepick",
                    effects : ["n/a or don't know","Yes","No"],
                    intermediates : ["risk_heat_map_controller"],
                    effect_intermediate_control : [
                        {affecting_intermediate : "risk_heat_map_controller", effect : "n/a or don't know", weight : 0},
                        {affecting_intermediate : "risk_heat_map_controller", effect : "Yes", weight : 3},
                        {affecting_intermediate : "risk_heat_map_controller", effect : "No", weight : 0}
                    ]
                },
                {
                    question_text : "Does this feel like a major incident?",
                    type : "onepick",
                    effects : ["Yes","No"],
                    intermediates : ["risk_heat_map_controller"],
                    effect_intermediate_control : [
                        {affecting_intermediate : "risk_heat_map_controller", effect : "Yes", weight : 3},
                        {affecting_intermediate : "risk_heat_map_controller", effect : "No", weight : 0}
                    ]
                },
                {
                    question_text : "Is there a potential cost to the incident?",
                    type : "onepick",
                    effects : ["None","Up to 10k","Between 10k and 100k","Over 100k"],
                    intermediates : ["risk_heat_map_controller"],
                    effect_intermediate_control : [
                        {affecting_intermediate : "risk_heat_map_controller", effect : "None", weight : 0},
                        {affecting_intermediate : "risk_heat_map_controller", effect : "Up to 10k", weight : 1},
                        {affecting_intermediate : "risk_heat_map_controller", effect : "Between 10k and 100k", weight : 2},
                        {affecting_intermediate : "risk_heat_map_controller", effect : "Over 100k", weight : 3}
                    ]
                },
                {
                    question_text : "How many people are involved? (does not include data breach)",
                    type : "onepick",
                    effects : ["0","1 to 5","6 to 10","More than 10"],
                    intermediates : ["risk_heat_map_controller"],
                    effect_intermediate_control : [
                        {affecting_intermediate : "risk_heat_map_controller", effect : "0", weight : 0},
                        {affecting_intermediate : "risk_heat_map_controller", effect : "1 to 5", weight : 1},
                        {affecting_intermediate : "risk_heat_map_controller", effect : "6 to 10", weight : 2},
                        {affecting_intermediate : "risk_heat_map_controller", effect : "More than 10", weight : 3}
                    ]
                },
                {
                    question_text : "Are VIPs affected",
                    type : "onepick",
                    effects : ["n/a or don't know","Yes","No"],
                    intermediates : ["risk_heat_map_controller"],
                    effect_intermediate_control : [
                        {affecting_intermediate : "risk_heat_map_controller", effect : "n/a or don't know", weight : 0},
                        {affecting_intermediate : "risk_heat_map_controller", effect : "Yes", weight : 1},
                        {affecting_intermediate : "risk_heat_map_controller", effect : "No", weight : 0}
                    ]
                },
            ],
            severity_groups : [
                {
                    name : "university_confidential_data_severity",
                    display_name : "University Confidential Data",
                    type : "exclusive",
                    severities : ["No data stored","Public","Internal","Confidential","Strictly Confidential"],
                    intermediates : ["university_confidential_data_controller"],
                    severity_intermediate_control : [
                        {class_trigger: 1, severity : "Public"},
                        {class_trigger: 2, severity : "Internal"},
                        {class_trigger: 3, severity : "Confidential"},
                        {class_trigger: 4, severity : "Strictly Confidential"}
                    ]
                },
                {
                    name : "personal_data_severity",
                    display_name : "Personal Data",
                    type : "exclusive",
                    severities : ["Yes","No"],
                    intermediates : ["personal_data_controller"],
                    severity_intermediate_control : [
                        {bin_trigger : true, severity : "Yes"},
                        {bin_trigger : false, severity : "No"}
                    ]
                },
                {
                    name : "risk_heat_map_severity",
                    display_name : "Incident Classification",
                    type : "exclusive",
                    severities : ["Category A / NCSC 6","Category B / NCSC 5","Category C / NCSC 4","Category D / NCSC 3"],
                    intermediates : ["place_dropped_controller"],
                    severity_intermediate_control : [
                        {on_threshold : 0, severity : "N/A"},
                        {on_threshold : 1, severity : "Category A / NCSC 6"},
                        {on_threshold : 5, severity : "Category B / NCSC 5"},
                        {on_threshold : 9, severity : "Category C / NCSC 4"},
                        {on_threshold : 13, severity : "Category D / NCSC 3"}
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
            EventBus.$emit('validateIntermediates', this.all_intermediates);
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
