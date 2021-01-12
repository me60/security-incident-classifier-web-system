<template>
  <div id="results">
    <h2>Personal Data: {{ p_data_text }}</h2>
    <h2>University Data: </h2>
    <h2>Incident Classification: </h2>
    <h2>Incident Manager Change: </h2>
  </div>
</template>

<script>
  import { EventBus } from '../event-bus.js';
  export default {
    name: "Results",
    data() {
      return {
        p_data_c: 0,
        p_data_text: "No",
        uni_data_1: 0,
        uni_data_2: 0,
        uni_data_3: 0,
        uni_data_4: 0,
        uni_data_text: "-",
        incident_class: "-",
        incident_manager_change_text: "-"
      }
    },
    methods: {
      checkPersonalData(answer) {
        if (answer === "Yes") {
          this.p_data_c++;
        } else {
          this.p_data_c--;
        }
        if (this.p_data_c > 0) {
          this.p_data_text = "Yes";
        } else {
          this.p_data_text = "No";
        }
      }
    },
    created() {
      EventBus.$on('p-data', this.checkPersonalData)
    }
  }
</script>

<style scoped>
</style>
