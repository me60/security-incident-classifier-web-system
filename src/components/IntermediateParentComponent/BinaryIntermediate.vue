<template>
    <div />
</template>

<script>
    import EventBus from '../../event-bus.js';
    export default {
        name: "BinaryIntermediate",
        props: ["name", "priority", "severity_system"],
        data() {
            return {
                // Bin inits at false, behaviour starts on first change
                state : false
            }
        },
        mounted() {
            EventBus.$on((this.name + ".activate"), (message) =>  {

                /* TODO: Priority amongst sending effects, not just
                intermediate priority*/

                //let affector = this.$parent.decodeUniqueIdentifier(message);
                let stateAffect = this.$parent.decodeAffect(message);
                this.state = stateAffect;
                this.signalSeverity();
            });
            EventBus.$on((this.name + ".deactivate"), (message) =>  {
                //let affector = this.$parent.decodeUniqueIdentifier(message);
                let stateAffect = this.$parent.decodeAffect(message);
                this.state = stateAffect;
                this.signalSeverity();
            });
        },
        methods: {
            signalSeverity : function() {
                console.log("Signalling: " + this.severity_system);
                EventBus.$emit(this.severity_system, this.state);
            }
        }
    }
</script>

<style />
