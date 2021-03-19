<template>
    <div id="counter_severity_group">
        <b><div :id="this.name"> Severity goes here! </div></b>
    </div>
</template>

<script>
    //import EventBus from '../../event-bus.js';
    export default {
        name: "CounterSeverityGroup",
        props: ["name", "severities", "severity_intermediate_control"]
        /*mounted: {
            EventBus.$on((this.name), (message) =>  {
                console.log("eyy");
            });
        }*/
    }
</script>

<style />
