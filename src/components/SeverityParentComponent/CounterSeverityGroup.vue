<template>
    <div id="counter_severity_group">
        <b> <div v-bind:style="{ color : activeColor }"> {{ this.onscreen }} </div> </b>
    </div>
</template>

<script>
    import EventBus from '../../event-bus.js';
    export default {
        name: "CounterSeverityGroup",
        props: ["name", "severities", "severity_intermediate_control"],
        data() {
            return {
                onscreen: "Severity will go here on activation!",
                activeColor: "white"
            }
        },
        mounted() {
            EventBus.$on((this.name), (message) => {
                for (let i = 0; i < this.severity_intermediate_control.length; i++) {
                    if (message == this.severity_intermediate_control[i].on_threshold) {
                        console.log(this.severity_intermediate_control[i].severity);
                        this.onscreen = this.severity_intermediate_control[i].severity;
                        this.activeColor = this.severity_intermediate_control[i].colour;
                    }
                }
            });
        }
    }
</script>

<style />
