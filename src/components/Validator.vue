<template>
    <div id="validator">
</template>

<script>
    import EventBus from "../event-bus.js";
    export default {
        name : "Validator",
        mounted() {
            EventBus.$on('validateSeverities', (severities) => {
                console.log("Validating Severities");
            });
            EventBus.$on('validateIntermediates', (intermediates) => {
                console.log("Validating Intermediates");
            });
            EventBus.$on('validateEffectGroups', (effect_groups) => {
                console.log("Validating Effect Groups");
            });
        }
    }
</script>

<style />
