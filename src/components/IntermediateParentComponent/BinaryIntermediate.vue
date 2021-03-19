<template>
    <div />
</template>

<script>
    import EventBus from '../../event-bus.js';
    export default {
        name: "BinaryIntermediate",
        props: ["name", "priority"],
        data() {
            return {
                // Bin inits at "null", behaviour starts at "true" or "false"
                // (Keep as string for now even though it's a binary type)
                state : "null",
                affectorsActive: [],
                stateAffected: []
            }
        },
        mounted() {
            EventBus.$on((this.name + ".activate"), (message) =>  {
                let affector = this.$parent.decodeUniqueIdentifier(message);
                console.log(affector);
                let stateAffect = this.$parent.decodeAffect(message);
                console.log(stateAffect);
            });
            EventBus.$on((this.name + ".deactivate"), (message) =>  {
                let affector = this.$parent.decodeUniqueIdentifier(message);
                console.log(affector);
                let stateAffect = this.$parent.decodeAffect(message);
                console.log(stateAffect);
            });
        }
    }
</script>

<style />
