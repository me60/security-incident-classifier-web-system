<template>
    <Button :p_q_type="p_q_type" :name="name"/>
</template>

<script>
    import Button from './Button.vue';
    export default {
        name: "IncrementEffect",
        props: ["c_class", "p_q_type", "a_i", "name"],
        components: {
            Button
        },
        methods: {
            activate : function() {
                console.log("IncrementEffect detected as active!");
            },
            deactivate : function() {
                console.log("IncrementEffect has been deactivated!");
            }
        }
    }
</script>

<style>
</style>
