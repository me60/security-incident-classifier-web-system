<template>
    <div id="intermediates">
        <div v-for="intermediate in all_intermediates" :key="intermediate.name">
            <div v-if="intermediate.type === 'inc'">
                <Intermediate :name="intermediate.name" :type="intermediate.type" :classes="intermediate.classes" :priority="intermediate.priority" :severity_system="intermediate.severity_system" :other_intermediate="intermediate.other_intermediate" :i_i_c="intermediate.intermediate_intermediate_control"/>
            </div>
            <div v-if="intermediate.type === 'bin'">
                <Intermediate :name="intermediate.name" :type="intermediate.type" :priority="intermediate.priority" :severity_system="intermediate.severity_system" :other_intermediate="intermediate.other_intermediate" :i_i_c="intermediate.intermediate_intermediate_control"/>
            </div>
            <div v-if="intermediate.type === 'count'">
                <Intermediate :name="intermediate.name" :type="intermediate.type" :thresholds="intermediate.thresholds" :priority="intermediate.priority" :severity_system="intermediate.severity_system" :other_intermediate="intermediate.other_intermediate" :i_i_c="intermediate.intermediate_intermediate_control"/>
            </div>
            <div v-if="intermediate.type === 'codeblock'">
                <Intermediate :name="intermediate.name" :type="intermediate.type" :pertaining_block="intermediate.pertaining_block" :priority="intermediate.priority" :severity_system="intermediate.severity_system" :other_intermediate="intermediate.other_intermediate" :i_i_c="intermediate.intermediate_intermediate_control"/>
            </div>
        </div>
    </div>
</template>

<script>
    import Intermediate from './Intermediate.vue'
    export default {
        name: "Intermediates",
        props: ["all_intermediates"],
        components: {
            Intermediate
        }
    }
</script>

<style>

</style>
