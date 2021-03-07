<template>
    <Button :p_q_type="p_q_type" :p_q_name="p_q_name" :name="name"/>
</template>

<script>
    import Button from './Button.vue';
    export default {
        name: "BinaryEffect",
        props: ["state", "p_q_type", "p_q_name", "a_i", "name"],
        components: {
            Button
        },
        methods: {
            activate : function() {
                console.log("BinaryEffect detected as active!");
            },
            deactivate : function() {
                console.log("BinaryEffect has been deactivated!");
            }
        }
    }
</script>

<style>
</style>
