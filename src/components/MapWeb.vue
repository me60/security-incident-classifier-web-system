<template>

</template>

<script>
  export default {
      name: "MapWeb",
      components: {
          EffectGroup,
          Intermediate,
          SeverityGroup
      },
      data() {
          return {
              QUESTION_TYPE : {
                [yes_no,one_pick,multi_pick]
              },
              INTERMEDIATE_TYPE : {
                [increment,binary,counter]
              },
              SEVERITY_TYPE : {
                [inclusive,exclusive]
              },
              all_intermediates : {
                  [{
                      name = "amount_severity_controller",
                      type = this.INTERMEDIATE_TYPE.increment,
                      classes = 3,
                      priority = true
                  },
                  {
                      name = "covering_severity_controller",
                      type = this.INTERMEDIATE_TYPE.increment,
                      classes = 3,
                      priority = true
                  },
                  {
                      name = "ant_colour_controller",
                      type = this.INTERMEDIATE_TYPE.binary,
                      priority = true
                  },
                  {
                      name = "place_dropped_controller",
                      type = this.INTERMEDIATE_TYPE.counter,
                      thresholds = [1,2,3],
                      priority = true
                  }]
              }
              effect_groups : [{
                      question_text = "How much bread was dropped?",
                      type = this.QUESTION_TYPE.one_pick,
                      effects = {"Crumbs","Bits","Slice","Loaf"},
                      intermediates = ["amount_severity_controller"],
                      effect_intermediate_control = [
                          {"amount_severity_controller","Crumbs",1},
                          {"amount_severity_controller","Bits",1},
                          {"amount_severity_controller","Slice",2},
                          {"amount_severity_controller","Loaf",2}
                      ]
                  },
                  {
                      question_text = "What was on the bread?",
                      type = this.QUESTION_TYPE.one_pick,
                      effects = {"Nothing","Butter","Jam"},
                      intermediates = ["covering_severity_controller"],
                      effect_intermediate_control = [
                          {"covering_severity_controller","Nothing",1},
                          {"covering_severity_controller","Butter",2},
                          {"covering_severity_controller","Jam",3}
                      ]
                  },
                  {
                      question_text = "Are the ants red?",
                      type = this.QUESTION_TYPE.yes_no,
                      intermediates = ["ant_colour_controller"],
                      effect_intermediate_control = [
                          {"ant_colour_controller","Yes",true},
                          {"ant_colour_controller","No",false}
                      ]
                  },
                  {
                      question_text = "Where was the bread dropped?",
                      type = this.QUESTION_TYPE.multi_pick,
                      effects = {"Nothing","Butter","Jam"},
                      intermediates = ["place_dropped_controller"],
                      effect_intermediate_control = [
                          {"place_dropped_controller","Kitchen",1},
                          {"place_dropped_controller","Bathroom",1},
                          {"place_dropped_controller","Bedroom",1}
                      ]
                  }
              ],
              severity_groups = [
                  {
                      name = "general_severity",
                      type = this.SEVERITY_TYPE.exclusive,
                      severities = ["Not Severe","Pretty Bad","Infestation Likely"],
                      intermediates = ["amount_severity_controller","covering_severity_controller"],
                      severity_intermediate_control = [
                          {1,"Not Severe"},
                          {2,"Pretty Bad"},
                          {3,"Infestation Likely"}
                      ]
                  },
                  {
                      name = "ant_type_severity",
                      type = this.SEVERITY_TYPE.exclusive,
                      severities = ["Normal Ants","Fire Ants!"],
                      intermediates = ["ant_colour_controller"],
                      severity_intermediate_control = [
                          {true,"Fire Ants!"},
                          {false,"Normal Ants"}
                      ]
                  },
                  {
                      name = "rooms_severity",
                      type = this.SEVERITY_TYPE.exclusive,
                      severities = ["1 Room Affected","2 Rooms Affected","3 Rooms Affected"],
                      intermediates = ["place_dropped_controller"],
                      severity_intermediate_control = [
                          {1,"1 Room Affected"},
                          {2,"2 Rooms Affected"},
                          {3,"3 Rooms Affected"}
                      ]
                  }
              ]
          }
      },
      methods: {

          /* Code here should formulate the MapWeb structure for the on-screen
          render - instantiate effects, intermediates, etc. as component
          instantiations that can be referenced by their uid and contain
          appropriate types, e.g. if a specific intermediate is a counter and
          what thresholds it contains*/
          construct: function () {
              // send, as separate events on the event bus
                // the effect groups
                    // perform validation
                // the intermediates
                    // perform validation
                // the severity systems
                    // perform validation
          } // construct
      }
  }
</script>

<style>
</style>
