<template>
    <div id="increment_severity_group">
        <b> <div v-bind:style="{ color : activeColor }"> {{ this.onscreen }} </div> </b>
    </div>
</template>

<script>
    import EventBus from '../../event-bus.js';
    export default {
        name: "IncrementSeverityGroup",
        props: ["name", "severities", "severity_intermediate_control"],
        data() {
            return {
                onscreen: "-",
                activeColor: "white"
            }
        },
        mounted() {
            // This code finds the message to display based on received signal
            EventBus.$on((this.name), (message) => {
                for (let i = 0; i < this.severity_intermediate_control.length; i++) {
                    if (message == this.severity_intermediate_control[i].class_trigger) {
                        this.onscreen = this.severity_intermediate_control[i].severity;
                        this.activeColor = this.severity_intermediate_control[i].colour;
                    }
                }
            });
        }
    }
</script>

<style>
</style>
