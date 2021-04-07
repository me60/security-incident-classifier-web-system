<template>
    <div id="binary_severity_group">
        <b> {{ this.onscreen }} </b>
    </div>
</template>

<script>
    import EventBus from '../../event-bus.js';
    export default {
        name: "BinarySeverityGroup",
        props: ["name", "severities", "severity_intermediate_control"],
        data() {
            return {
                onscreen: "Severity will go here on activation!"
            }
        },
        mounted() {
            EventBus.$on((this.name), (message) =>  {
                console.log("!!! " + message);
                for (let i = 0; i < this.severity_intermediate_control.length; i++) {
                    //console.log("compare " + equivalent + " to " + this.severity_intermediate_control[i].bin_trigger + " " + this.severity_intermediate_control[i].severity);
                    if (message == this.severity_intermediate_control[i].bin_trigger) {
                        this.onscreen = this.severity_intermediate_control[i].severity;
                        console.log(this.onscreen);
                    }
                }
            });
        }
    }
</script>

<style />
