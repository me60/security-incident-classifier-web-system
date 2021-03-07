<template>
    </b>
</template>

<script>
    // Imports
    import EventBus from '../event-bus.js';

    // Events
    EventBus.$on('validateIntermediates', all_intermediates =>  {
        intermediateValidation(all_intermediates);
    });

    // Functions
    function intermediateValidation(all_intermediates) {
        console.log("eyy");
    } // intermediateValidation

    // Vue
    export default {
        name: "Intermediate",
        methods: {
        // Validation method for intermediate checks
        // redirect effect invocation to appropriate intermediate subtype
        }
    }
</script>

<style>
</style>
