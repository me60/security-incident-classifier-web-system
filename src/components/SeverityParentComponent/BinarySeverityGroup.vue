<template>
    <div id="binary_severity_group">
        <b> <div v-bind:style="{ color : activeColor }"> {{ this.onscreen }} </div> </b>
    </div>
</template>

<script>
    import EventBus from '../../event-bus.js';
    export default {
        name: "BinarySeverityGroup",
        props: ["name", "severities", "severity_intermediate_control"],
        data() {
            return {
                onscreen: "-",
                activeColor: "white"
            }
        },
        mounted() {
            // This code finds the message to display based on received signal
            EventBus.$on((this.name), (message) =>  {
                console.log("!!! " + message);
                for (let i = 0; i < this.severity_intermediate_control.length; i++) {
                    if (message == this.severity_intermediate_control[i].bin_trigger) {
                        this.onscreen = this.severity_intermediate_control[i].severity;
                        this.activeColor = this.severity_intermediate_control[i].colour;
                        console.log(this.onscreen);
                    }
                }
            });
        }
    }
</script>

<style />
