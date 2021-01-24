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
              effect_groups : {},
              intermediates : {},
              severity_groups : {}
          }
      },
      methods: {

          /* Code here should formulate the MapWeb structure for the on-screen
          render*/
          construct: function () {
              this.effect_groups
          } // construct
      }
  }
</script>

<style>
</style>
