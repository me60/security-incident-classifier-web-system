<template>
    <Button :p_q_type="p_q_type" :p_q_name="p_q_name" :name="name"/>
</template>

<script>
    import Button from './Button.vue';
    export default {
        name: "IncrementEffect",
        props: ["c_class", "p_q_type", "p_q_name", "a_i", "name"],
        components: {
            Button
        },
        methods: {
            activate : function() {
                console.log(this.p_q_name + "." + this.name + " detected as active!");
            },
            deactivate : function() {
                console.log(this.p_q_name + "." + this.name + " detected as inactive!");
            }
        }
    }
</script>

<style>
</style>
