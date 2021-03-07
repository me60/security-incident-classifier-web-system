<template>
    <div>
        <input type="radio" :name="`group${p_q_name}`" :id="`group${p_q_name}`" :change="changeHandler()">
        <label for="Onepick"> {{this.name}} </label>
    </div>
</template>

<script>
    export default {
        name: "Onepick",
        props: ["name", "p_q_name"],
        methods: {
            changeHandler : function() {
                this.$parent.determineActivation();
            }
        }
    }
</script>

<style />
