<template>
    <div id="intermediates">
        <div v-for="intermediate in all_intermediates" :key="intermediate.name">
            <div v-if="intermediate.type === 'inc'">
                <Intermediate :name="intermediate.name" :type="intermediate.type" :classes="intermediate.classes" :priority="intermediate.priority" :severity_system="intermediate.severity_system"/>
            </div>
            <div v-if="intermediate.type === 'bin'">
                <Intermediate :name="intermediate.name" :type="intermediate.type" :priority="intermediate.priority" :severity_system="intermediate.severity_system"/>
            </div>
            <div v-if="intermediate.type === 'count'">
                <Intermediate :name="intermediate.name" :type="intermediate.type" :thresholds="intermediate.thresholds" :priority="intermediate.priority" :severity_system="intermediate.severity_system"/>
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
