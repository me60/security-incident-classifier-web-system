<template>
    <div id="increment_severity_group">
        <b> {{ this.onscreen }} </b>
    </div>
</template>

<script>
    import EventBus from '../../event-bus.js';
    export default {
        name: "IncrementSeverityGroup",
        props: ["name", "severities", "severity_intermediate_control"],
        data() {
            return {
                onscreen: "Severity will go here on activation!"
            }
        },
        mounted() {
            EventBus.$on((this.name), (message) => {
                for (let i = 0; i < this.severity_intermediate_control.length; i++) {
                    if (message == this.severity_intermediate_control[i].class_trigger) {
                        this.onscreen = this.severity_intermediate_control[i].severity;
                    }
                }
            });
        }
    }
</script>

<style />
