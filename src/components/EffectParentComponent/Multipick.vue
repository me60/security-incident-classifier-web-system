<template>
    <div>
        <button type="button"> {{this.name}} </button>
    </div>
</template>

<script>
    export default {
        name: "Multipick",
        props: ["name", "p_q_name"],
        methods: {
            changeHandler : function() {
                this.$parent.determineActivation();
            }
        }
    }
</script>

<style />
