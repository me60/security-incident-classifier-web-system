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
            classifier_name : "University of St. Andrews Incident Classifier",
            all_intermediates : [
                {
                    name : "university_confidential_data_controller",
                    type : "inc",
                    classes : 4,
                    priority : true,
                    severity_system: "university_confidential_data_severity",
                    other_intermediate: "risk_heat_map_controller",
                    intermediate_intermediate_control : [
                        // must include controls for each class
                        {affecting_intermediate : "risk_heat_map_controller", class_trigger: 0, weight : 0},
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
                        // must be empty if none
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
                        {affecting_intermediate : "risk_heat_map_controller", effect : "Yes", weight : 0, basic_trigger : "Inform Governance"},
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
                {
                    question_text : "Can the data identify a living individual?",
                    type : "onepick",
                    effects : ["Yes","No"],
                    intermediates : ["personal_data_controller"],
                    effect_intermediate_control : [
                        {affecting_intermediate : "personal_data_controller", effect : "Yes", state : true},
                        {affecting_intermediate : "personal_data_controller", effect : "No", state : false}
                    ]
                },
                {
                    question_text : "Does the data include any of the following:",
                    type : "multipick",
                    effects : ["Racial or Ethnic Origin","Political Opinion","Religious Beliefs","Trade Union Membership","Physical or Mental Health Conditions","Sex Life","Involvement in Criminal Proceedings","Outcomes of Criminal Convictions"],
                    intermediates : ["personal_data_controller"],
                    effect_intermediate_control : [
                        {affecting_intermediate : "personal_data_controller", effect : "Racial or Ethnic Origin", state : true},
                        {affecting_intermediate : "personal_data_controller", effect : "Political Opinion", state : true},
                        {affecting_intermediate : "personal_data_controller", effect : "Religious Beliefs", state : true},
                        {affecting_intermediate : "personal_data_controller", effect : "Trade Union Membership", state : true},
                        {affecting_intermediate : "personal_data_controller", effect : "Physical or Mental Health Conditions", state : true},
                        {affecting_intermediate : "personal_data_controller", effect : "Sex Life", state : true},
                        {affecting_intermediate : "personal_data_controller", effect : "Involvement in Criminal Proceedings", state : true},
                        {affecting_intermediate : "personal_data_controller", effect : "Outcomes of Criminal Convictions", state : true}
                    ]
                },
                {
                    question_text : "Is there a substantial risk to health, safety or well being to individuals or groups?",
                    type : "onepick",
                    effects : ["Yes","No"],
                    intermediates : ["personal_data_controller"],
                    effect_intermediate_control : [
                        {affecting_intermediate : "personal_data_controller", effect : "Yes", state : true},
                        {affecting_intermediate : "personal_data_controller", effect : "No", state : false}
                    ]
                },
                {
                    question_text : "Does it involve the prevention or detection of crime?",
                    type : "onepick",
                    effects : ["Yes","No"],
                    intermediates : ["personal_data_controller"],
                    effect_intermediate_control : [
                        {affecting_intermediate : "personal_data_controller", effect : "Yes", state : true},
                        {affecting_intermediate : "personal_data_controller", effect : "No", state : false}
                    ]
                },
                {
                    question_text : "Might it stop the apprehension or prosecution of an offender?",
                    type : "onepick",
                    effects : ["Yes","No"],
                    intermediates : ["personal_data_controller"],
                    effect_intermediate_control : [
                        {affecting_intermediate : "personal_data_controller", effect : "Yes", state : true},
                        {affecting_intermediate : "personal_data_controller", effect : "No", state : false}
                    ]
                },
                {
                    question_text : "Does the data include opinions about an individual?",
                    type : "onepick",
                    effects : ["Yes","No"],
                    intermediates : ["personal_data_controller"],
                    effect_intermediate_control : [
                        {affecting_intermediate : "personal_data_controller", effect : "Yes", state : true},
                        {affecting_intermediate : "personal_data_controller", effect : "No", state : false}
                    ]
                },
                {
                    question_text : "Is the loss of data likely to cause harm or inconvenience to an individual?",
                    type : "onepick",
                    effects : ["Yes","No"],
                    intermediates : ["personal_data_controller"],
                    effect_intermediate_control : [
                        {affecting_intermediate : "personal_data_controller", effect : "Yes", state : true},
                        {affecting_intermediate : "personal_data_controller", effect : "No", state : false}
                    ]
                },
                {
                    question_text : "Staff contact details (where these do not concern public facing roles)?",
                    type : "onepick",
                    effects : ["Yes","No"],
                    intermediates : ["personal_data_controller"],
                    effect_intermediate_control : [
                        {affecting_intermediate : "personal_data_controller", effect : "Yes", state : true},
                        {affecting_intermediate : "personal_data_controller", effect : "No", state : false}
                    ]
                },
                {
                    question_text : "Student/staff photograph?",
                    type : "onepick",
                    effects : ["Yes","No"],
                    intermediates : ["personal_data_controller"],
                    effect_intermediate_control : [
                        {affecting_intermediate : "personal_data_controller", effect : "Yes", state : true},
                        {affecting_intermediate : "personal_data_controller", effect : "No", state : false}
                    ]
                },
                {
                    question_text : "Contracts of employment?",
                    type : "onepick",
                    effects : ["Yes","No"],
                    intermediates : ["personal_data_controller"],
                    effect_intermediate_control : [
                        {affecting_intermediate : "personal_data_controller", effect : "Yes", state : true},
                        {affecting_intermediate : "personal_data_controller", effect : "No", state : false}
                    ]
                },
                {
                    question_text : "Student transcripts?",
                    type : "onepick",
                    effects : ["Yes","No"],
                    intermediates : ["personal_data_controller"],
                    effect_intermediate_control : [
                        {affecting_intermediate : "personal_data_controller", effect : "Yes", state : true},
                        {affecting_intermediate : "personal_data_controller", effect : "No", state : false}
                    ]
                },
                {
                    question_text : "Counselling records?",
                    type : "onepick",
                    effects : ["Yes","No"],
                    intermediates : ["personal_data_controller"],
                    effect_intermediate_control : [
                        {affecting_intermediate : "personal_data_controller", effect : "Yes", state : true},
                        {affecting_intermediate : "personal_data_controller", effect : "No", state : false}
                    ]
                },
                {
                    question_text : "Disciplinary proceedings?",
                    type : "onepick",
                    effects : ["Yes","No"],
                    intermediates : ["personal_data_controller"],
                    effect_intermediate_control : [
                        {affecting_intermediate : "personal_data_controller", effect : "Yes", state : true},
                        {affecting_intermediate : "personal_data_controller", effect : "No", state : false}
                    ]
                },
                {
                    question_text : "Grievance proceedings?",
                    type : "onepick",
                    effects : ["Yes","No"],
                    intermediates : ["personal_data_controller"],
                    effect_intermediate_control : [
                        {affecting_intermediate : "personal_data_controller", effect : "Yes", state : true},
                        {affecting_intermediate : "personal_data_controller", effect : "No", state : false}
                    ]
                },
                {
                    question_text : "Does the information lost contain any of the following? (1):",
                    type : "multipick",
                    effects : ["Course information","Student ID Card Information","Degree congregation programme (including list of graduates)","Map of University buildings","Opening hours","Press releases", "Published research papers", "University prospectus"],
                    intermediates : ["general_severity_controller"],
                    effect_intermediate_control : [
                        {affecting_intermediate : "university_confidential_data_controller", effect : "Course information", class : 1},
                        {affecting_intermediate : "university_confidential_data_controller", effect : "Student ID Card Information", class : 1},
                        {affecting_intermediate : "university_confidential_data_controller", effect : "Degree congregation programme (including list of graduates)", class : 1},
                        {affecting_intermediate : "university_confidential_data_controller", effect : "Map of University buildings", class : 1},
                        {affecting_intermediate : "university_confidential_data_controller", effect : "Opening hours", class : 1},
                        {affecting_intermediate : "university_confidential_data_controller", effect : "Press releases", class : 1},
                        {affecting_intermediate : "university_confidential_data_controller", effect : "Published research papers", class : 1},
                        {affecting_intermediate : "university_confidential_data_controller", effect : "University prospectus", class : 1}
                    ]
                },
                {
                    question_text : "Does the information lost contain any of the following? (2):",
                    type : "multipick",
                    effects : ["Budget information","Details of funding settlements","Draft documents (which do not contain personal or sensitive personal data)","Internal audit reports","Internal memos","Key performance indicators","Lecture materials","Minutes of University and School committees","Planning applications","Student statistics (anonymised)"],
                    intermediates : ["general_severity_controller"],
                    effect_intermediate_control : [
                        {affecting_intermediate : "university_confidential_data_controller", effect : "Budget information", class : 2},
                        {affecting_intermediate : "university_confidential_data_controller", effect : "Details of funding settlements", class : 2},
                        {affecting_intermediate : "university_confidential_data_controller", effect : "Draft documents (which do not contain personal or sensitive personal data)", class : 2},
                        {affecting_intermediate : "university_confidential_data_controller", effect : "Internal audit reports", class : 2},
                        {affecting_intermediate : "university_confidential_data_controller", effect : "Internal memos", class : 2},
                        {affecting_intermediate : "university_confidential_data_controller", effect : "Key performance indicators", class : 2},
                        {affecting_intermediate : "university_confidential_data_controller", effect : "Lecture materials", class : 2},
                        {affecting_intermediate : "university_confidential_data_controller", effect : "Minutes of University and School committees", class : 2},
                        {affecting_intermediate : "university_confidential_data_controller", effect : "Planning applications", class : 2},
                        {affecting_intermediate : "university_confidential_data_controller", effect : "Student statistics (anonymised)", class : 2}
                    ]
                },
                {
                    question_text : "Does the information lost contain any of the following? (3):",
                    type : "multipick",
                    effects : ["Commercial contracts","Disaster recovery / business continuity plans","Documentation that contains decisions surrounding academic performance","Examination results","Payroll / banking details","Planning / forecasting reports","Procurement / invitation to tender documentation","Research grant applications","Strategic planning","University Risk Register and controls"],
                    intermediates : ["general_severity_controller"],
                    effect_intermediate_control : [
                        {affecting_intermediate : "university_confidential_data_controller", effect : "Commercial contracts", class : 3},
                        {affecting_intermediate : "university_confidential_data_controller", effect : "Disaster recovery / business continuity plans", class : 3},
                        {affecting_intermediate : "university_confidential_data_controller", effect : "Documentation that contains decisions surrounding academic performance", class : 3},
                        {affecting_intermediate : "university_confidential_data_controller", effect : "Examination results", class : 3},
                        {affecting_intermediate : "university_confidential_data_controller", effect : "Payroll / banking details", class : 3},
                        {affecting_intermediate : "university_confidential_data_controller", effect : "Planning / forecasting reports", class : 3},
                        {affecting_intermediate : "university_confidential_data_controller", effect : "Procurement / invitation to tender documentation", class : 3},
                        {affecting_intermediate : "university_confidential_data_controller", effect : "Research grant applications", class : 3},
                        {affecting_intermediate : "university_confidential_data_controller", effect : "Strategic planning", class : 3},
                        {affecting_intermediate : "university_confidential_data_controller", effect : "University Risk Register and controls", class : 3}
                    ]
                },
                {
                    question_text : "Does the information lost contain any of the following? (4):",
                    type : "multipick",
                    effects : ["Accident reports","Bank/credit card details","Case files and correspondence surrounding investigations by a regulatory body","Communications with Government (Ministerial level)","Government (Ministerial level)","Communications with legal counsel","Communications with Police Scotland (operational matters)","Legal proceedings","Passwords and other forms of access control credentials"],
                    intermediates : ["general_severity_controller"],
                    effect_intermediate_control : [
                        {affecting_intermediate : "university_confidential_data_controller", effect : "Accident reports", class : 4},
                        {affecting_intermediate : "university_confidential_data_controller", effect : "Bank/credit card details", class : 4},
                        {affecting_intermediate : "university_confidential_data_controller", effect : "Case files and correspondence surrounding investigations by a regulatory body", class : 4},
                        {affecting_intermediate : "university_confidential_data_controller", effect : "Communications with Government (Ministerial level)", class : 4},
                        {affecting_intermediate : "university_confidential_data_controller", effect : "Government (Ministerial level)", class : 4},
                        {affecting_intermediate : "university_confidential_data_controller", effect : "Communications with legal counsel", class : 4},
                        {affecting_intermediate : "university_confidential_data_controller", effect : "Communications with Police Scotland (operational matters)", class : 4},
                        {affecting_intermediate : "university_confidential_data_controller", effect : "Legal proceedings", class : 4},
                        {affecting_intermediate : "university_confidential_data_controller", effect : "Passwords and other forms of access control credentials", class : 4}
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
                        {class_trigger: 0, severity : "No Data", colour : "grey"},
                        {class_trigger: 1, severity : "Public", colour : "green"},
                        {class_trigger: 2, severity : "Internal", colour : "orange"},
                        {class_trigger: 3, severity : "Confidential", colour : "red"},
                        {class_trigger: 4, severity : "Strictly Confidential", colour : "black"}
                    ]
                },
                {
                    name : "personal_data_severity",
                    display_name : "Personal Data",
                    type : "exclusive",
                    severities : ["Yes","No"],
                    intermediates : ["personal_data_controller"],
                    severity_intermediate_control : [
                        {bin_trigger : false, severity : "No", colour : "grey"},
                        {bin_trigger : true, severity : "Yes - Possible GDPR Notification Required", colour : "black"}
                    ]
                },
                {
                    name : "risk_heat_map_severity",
                    display_name : "Incident Classification",
                    type : "exclusive",
                    severities : ["Category A / NCSC 6","Category B / NCSC 5","Category C / NCSC 4","Category D / NCSC 3"],
                    intermediates : ["place_dropped_controller"],
                    severity_intermediate_control : [
                        {on_threshold : 0, severity : "N/A", colour : "grey"},
                        {on_threshold : 1, severity : "Category A / NCSC 6", colour : "green"},
                        {on_threshold : 5, severity : "Category B / NCSC 5", colour : "orange"},
                        {on_threshold : 9, severity : "Category C / NCSC 4", colour : "red"},
                        {on_threshold : 13, severity : "Category D / NCSC 3", colour : "black"}
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
