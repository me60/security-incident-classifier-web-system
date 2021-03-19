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
                onscreen: "Severity goes here!"
            }
        },
        mounted() {
            EventBus.$on((this.name), (message) =>  {
                for (let i = 0; i < this.severity_intermediate_control.length; i++) {
                    console.log("!!! " + message + " " + this.severity_intermediate_control[i].bin_trigger);
                    let equivalent = (message === 'true');
                    if (equivalent === this.severity_intermediate_control[i].bin_trigger) {
                        this.onscreen = this.severity_intermediate_control[i].severity;
                    }
                }
            });
        }
    }
</script>

<style />
