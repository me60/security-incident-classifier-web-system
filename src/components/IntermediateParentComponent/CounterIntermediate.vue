<template>
    <div />
</template>

<script>
    import EventBus from '../../event-bus.js';
    export default {
        name: "CounterIntermediate",
        props: ["name", "thresholds", "priority", "severity_system", "other_intermediate", "i_i_c"],
        data() {
            return {
                // Count inits at 0, behaviour starts at 1
                currentThreshold: -3000,
                currentCount : 0,
                affectorsActive: [],
                countsContributed: []
            }
        },

        /* TODO: Counter functionality in effects and this intermediate +
        CodeBlock intermediates!*/

        mounted() {
            EventBus.$on((this.name + ".activate"), (message) =>  {
                let affector = this.$parent.decodeUniqueIdentifier(message);
                let countAffect = this.$parent.decodeAffect(message);
                countAffect = parseInt(countAffect, 10);
                console.log("Adding " + countAffect + " to counter");
                this.affectorsActive.push(affector);
                this.countsContributed.push(countAffect);

                // Add the count affect to the counter
                this.currentCount = this.currentCount + countAffect;
                this.determineThreshold();
            });
            EventBus.$on((this.name + ".deactivate"), (message) =>  {
                let affector = this.$parent.decodeUniqueIdentifier(message);
                let indexToRemove = this.affectorsActive.indexOf(affector);
                let countAffect = this.$parent.decodeAffect(message);
                countAffect = parseInt(countAffect, 10);
                console.log("Subtracting " + countAffect + " from counter");

                // Remove affector unchecked and its count
                if (indexToRemove != -1) {
                    this.affectorsActive.splice(indexToRemove, 1);
                    this.countsContributed.splice(indexToRemove, 1);
                } else {
                    console.log("Something went wrong: Affect " + affector + " was not logged as active within intermediate");
                }

                // Subtract the weight from the counter
                this.currentCount = this.currentCount - countAffect;
                this.determineThreshold();
            });
        },
        methods: {
            determineThreshold : function() {
                //let lower = this.thresholds[0];
                //console.log("Highest: " + highest);
                console.log("Current Count: " + this.currentCount);
                for (let i = 0; i < this.thresholds.length; i++) {
                    /* !!! Thresholds act as INCLUSIVE boundaries !!!
                    I will include this in the docs as it could be a
                    source of a headache to a maintainer, i.e. if a threshold
                    sits at 10, a count of 10 will trigger that threshold, NOT
                    a count of 11*/
                    if (this.currentCount >= this.thresholds[i]) {
                        this.currentThreshold = this.thresholds[i];
                    }
                }
                console.log("Current Threshold: " + this.currentThreshold);
                this.signalIntermediates();
            },
            signalIntermediates : function() {
                if (this.i_i_c.lenth != 0) {
                    for (let i = 0; i < this.i_i_c.length; i++) {
                        if (this.i_i_c[i].on_threshold == this.currentThreshold) {
                            if (this.$parent.lastSentPayload != null) {
                                this.$parent.deactivateOtherIntermediate();
                                this.$parent.activateOtherIntermediate(this.i_i_c[i]);
                            } else {
                                this.$parent.activateOtherIntermediate(this.i_i_c[i]);
                            }
                        }
                    }
                }
                this.signalSeverity();
            },
            signalSeverity : function() {
                console.log("Signalling: " + this.severity_system);
                EventBus.$emit(this.severity_system, this.currentThreshold);
            }
        }
    }
</script>

<style />
