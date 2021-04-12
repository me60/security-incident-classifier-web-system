<template>
    <div id="severity_group">
        <div v-if="severity_intermediate_control[0].class_trigger != null">
            <IncrementSeverityGroup :name="this.name" :severities="this.severities" :severity_intermediate_control="this.severity_intermediate_control"/>
        </div>
        <div v-if="severity_intermediate_control[0].bin_trigger != null">
            <BinarySeverityGroup :name="this.name" :severities="this.severities" :severity_intermediate_control="this.severity_intermediate_control"/>
        </div>
        <div v-if="severity_intermediate_control[0].on_threshold != null">
            <CounterSeverityGroup :name="this.name" :severities="this.severities" :severity_intermediate_control="this.severity_intermediate_control"/>
        </div>
        <div v-if="severity_intermediate_control[0].on_value != null">
            <RegressiveSeverityGroup :name="this.name" :severities="this.severities" :severity_intermediate_control="this.severity_intermediate_control"/>
        </div>
    </div>
</template>

<script>
    import IncrementSeverityGroup from './IncrementSeverityGroup.vue';
    import CounterSeverityGroup from './CounterSeverityGroup.vue';
    import BinarySeverityGroup from './BinarySeverityGroup.vue';
    import RegressiveSeverityGroup from './RegressiveSeverityGroup.vue';
    export default {
        name: "SeverityGroup",
        props: ["name", "type", "severities", "severity_intermediate_control"],
        components: {
            IncrementSeverityGroup,
            BinarySeverityGroup,
            CounterSeverityGroup,
            RegressiveSeverityGroup
        },
        methods: {
            random : function() {
                console.log("name: " + this.name);
            }
        }
    }
</script>

<style>
</style>
