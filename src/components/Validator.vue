<template>
    <div id="validator">
</template>

<script>
    import EventBus from "../event-bus.js";
    export default {
        name : "Validator",
        mounted() {
            EventBus.$on('validateSeverities', (severities) => {
                // Code goes here
            });
            EventBus.$on('validateIntermediates', (intermediates) => {
                // Code goes here
            });
            EventBus.$on('validateEffectGroups', (effect_groups) => {
                // Code goes here
            });
        }
    }
</script>

<style />
