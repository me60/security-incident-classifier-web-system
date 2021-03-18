<template>
    <div id="intermediates">
        <div v-for="intermediate in all_intermediates" :key="intermediate.name">
            <Intermediate />
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
