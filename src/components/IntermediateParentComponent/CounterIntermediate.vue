<template>
    <div />
</template>

<script>
    import EventBus from '../../event-bus.js';
    export default {
        name: "CounterIntermediate",
        props: ["name", "thresholds", "priority", "severity_system"],
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
                       !!! I will include this in the docs as it could be a
                       source of a headache to a maintainer*/
                    if (this.currentCount >= this.thresholds[i]) {
                        this.currentThreshold = this.thresholds[i];
                    }
                    /*if (this.currentCount < this.thresholds[i] && this.thresholds[i] > lower) {
                        /* Maintain the highest as the thresholds may not be
                        sorted
                        console.log(this.currentCount + " " + this.thresholds[i]);
                        lower = this.thresholds[i];
                        this.currentThreshold = this.thresholds[i];
                    }*/
                }
                console.log("Current Threshold: " + this.currentThreshold);
                this.signalSeverity();
            },
            signalSeverity : function() {
                /*This intermediate will signal the CEILING threshold
                it evaluates at any one time*/
                console.log("Signalling: " + this.severity_system);
                EventBus.$emit(this.severity_system, this.currentThreshold);
            }
        }
    }
</script>

<style />
