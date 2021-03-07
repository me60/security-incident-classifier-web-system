<template>
    <div>
        <button type="button" id="Multipick"> {{this.name}} </button>
    </div>
</template>

<script>
    export default {
        name: "Multipick",
        props: ["name"],
        methods: {
            changeHandler : function() {
                this.$parent.determineActivation();
            }
        }
    }
</script>

<style />
