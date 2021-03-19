<template>
    <div id="intermediate">
        <div v-if="type === 'inc'">
            <IncrementIntermediate :classes="classes" :name="name" :priority="priority"/>
        </div>
        <div v-if="type === 'bin'">
            <BinaryIntermediate :name="name" :priority="priority"/>
        </div>
        <div v-if="type === 'count'">
            <CounterIntermediate :name="name" :thresholds="thresholds" :priority="priority"/>
        </div>
    </div>
</template>

<script>
    import IncrementIntermediate from './IncrementIntermediate.vue';
    import CounterIntermediate from './CounterIntermediate.vue';
    import BinaryIntermediate from './BinaryIntermediate.vue';
    export default {
        name: "Intermediate",
        props: ["name", "type", "classes", "thresholds", "priority", "severity_system"],
        components: {
            IncrementIntermediate,
            BinaryIntermediate,
            CounterIntermediate
        }
    }
</script>

<style />
