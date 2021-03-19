<template>
    <div />
</template>

<script>
    import EventBus from '../../event-bus.js';
    export default {
        name: "IncrementIntermediate",
        props: ["name", "classes", "priority"],
        data() {
            return {
                // Class inits at 0, behaviour starts at 1
                currentClass : 0,
                /* We maintain a list of affectors and map each index to the
                index of the class they signal in classAffected*/
                affectorsActive: [],
                classAffected: []
            }
        },
        // Mounted acts as the area where events are listened for
        mounted() {
            /* We've already checked that the intermediate signalled by the
            effect is valid, so here we just take the triggering effect's word
            for it that the signal is legitimate*/
            EventBus.$on((this.name + ".activate"), (message) =>  {
                let affector = this.$parent.decodeUniqueIdentifier(message);
                let classAffect = this.$parent.decodeNumber(message);
            });
            EventBus.$on((this.name + ".deactivate"), (message) =>  {
                let affector = this.$parent.decodeUniqueIdentifier(message);
                let classAffect = this.$parent.decodeNumber(message);
            });
        }
    }
</script>

<style />
