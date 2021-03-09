<template>
    <div>
        <input type="radio" :name="`group${p_q_name}`" :id="`group${p_q_name}`" @change="changeHandler()">
        <label for="Onepick"> {{this.name}} </label>
    </div>
</template>

<script>
    import EventBus from "../event-bus.js";
    export default {
        name: "Onepick",
        props: ["name", "p_q_name"],
        methods: {
            changeHandler : function() {
                // find the previous active checked radio button and deactivate
                // it so as to signal the intermediate
                EventBus.$emit('deactivateOnepick', this.p_q_name);
                // this one gets called to set this onepick as true so to fire
                this.$parent.activate();
            }
        }
    }
</script>

<style />
