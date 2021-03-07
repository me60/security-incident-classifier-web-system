<template>
    <div>
        <input type="radio" name="radio" id="Onepick" change="changeHandler()">
        <label for="Onepick"> {{this.name}} </label>
    </div>
</template>

<script>
    export default {
        name: "Onepick",
        props: ["name"],
        methods: {
            changeHandler : function() {
                this.$parent.determineActivation();
            }
        }
    }
</script>

<style />
