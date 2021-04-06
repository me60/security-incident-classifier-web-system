<template>
    <div />
</template>

<script>
    import EventBus from '../../event-bus.js';
    export default {
        name: "BinaryIntermediate",
        props: ["name", "priority", "severity_system", "other_intermediate", "i_i_c"],
        data() {
            return {
                // Bin inits at false, behaviour starts on first change
                currentState : false,
                affectorsActive : [],
                statesAffected : []
            }
        },
        mounted() {
            EventBus.$on((this.name + ".activate"), (message) =>  {

                /* TODO: Priority amongst sending effects, not just
                intermediate priority*/

                let affector = this.$parent.decodeUniqueIdentifier(message);
                let stateAffect = this.$parent.decodeAffect(message);
                console.log("Payload: " + stateAffect);
                this.affectorsActive.push(affector);
                this.statesAffected.push(stateAffect);
                this.determineState();
            });
            EventBus.$on((this.name + ".deactivate"), (message) =>  {
                let affector = this.$parent.decodeUniqueIdentifier(message);
                let stateAffect = this.$parent.decodeAffect(message);
                console.log("Payload: " + stateAffect);
                this.affectorsActive.push(affector);
                this.statesAffected.push(stateAffect);
                this.determineState();
            });
        },
        methods: {
            determineState : function() {
                let a = true;
                for (let i = 0; i < this.affectorsActive.length; i++) {
                    // May be a source of a headache!
                    if (this.statesAffected[i]) {
                        this.currentState = true;
                        a = false;
                    }
                }
                if (a) {
                    this.currentState = false;
                }
                this.signalIntermediates();
            },
            signalIntermediates : function() {
                for (let i = 0; i < this.i_i_c.length; i++) {
                    // find interacting intermediates (if any) that match with
                    // the current state, signal that intermediate via
                    // effect-intermediate protocol EventBus event
                    if (this.i_i_c[i].bin_trigger == this.currentState) {
                        // get the last sent intermediate-intermediate event and
                        // deactivate it, if this is the first one sent (i.e. there
                        // is no previous intermediate-intermediate event stored)
                        // then simply do nothing and send the current event
                        if (this.$parent.lastSentPayload != null) {
                            this.$parent.deactivateOtherIntermediate();
                            this.$parent.activateOtherIntermediate(this.i_i_c[i]);
                        } else {
                            this.$parent.activateOtherIntermediate(this.i_i_c[i]);
                        }
                    }
                }
                this.signalSeverity();
            },
            signalSeverity : function() {
                console.log("Signalling: " + this.severity_system);
                EventBus.$emit(this.severity_system, this.state);
            }
        }
    }
</script>

<style />
