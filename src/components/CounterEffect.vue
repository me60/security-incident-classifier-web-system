<template>
    <Button :p_q_type="p_q_type" :p_q_name="p_q_name" :name="name"/>
</template>

<script>
    import Button from './Button.vue';
    export default {
        name: "CounterEffect",
        props: ["weight", "p_q_type", "p_q_name", "a_i", "name"],
        components: {
            Button
        },
        methods: {
            activate : function() {
                console.log("CounterEffect detected as active!");
            },
            deactivate : function() {
                console.log("CounterEffect has been deactivated!");
            }
        }
    }
</script>

<style>
</style>
