<template>
    <div />
</template>

<script>
    import EventBus from '../../event-bus.js';
    export default {
        name: "IncrementIntermediate",
        props: ["name", "classes", "priority", "severity_system", "other_intermediate", "i_i_c"],
        data() {
            return {
                // Class inits at 0, behaviour starts at 1
                currentClass : 0,
                /* We maintain a list of affectors and map each index to the
                index of the class they signal in classAffected*/
                affectorsActive: [],
                classesAffected: []
            }
        },
        // Mounted acts as the area where events are listened for
        mounted() {
            /* We've already checked that the intermediate signalled by the
            effect is valid, so here we just take the triggering effect's word
            for it that the signal is legitimate*/
            EventBus.$on((this.name + ".activate"), (message) =>  {
                let affector = this.$parent.decodeUniqueIdentifier(message);
                let classAffect = this.$parent.decodeAffect(message);
                this.affectorsActive.push(affector);
                this.classesAffected.push(classAffect);
                this.evaluateHighestClass();
            });
            EventBus.$on((this.name + ".deactivate"), (message) =>  {
                let affector = this.$parent.decodeUniqueIdentifier(message);
                let indexToRemove = this.affectorsActive.indexOf(affector);

                // generalise this
                if (indexToRemove != -1) {
                    this.affectorsActive.splice(indexToRemove, 1);
                    this.classesAffected.splice(indexToRemove, 1);
                } else {
                    console.log("Something went wrong: Affect " + affector + " was not logged as active within intermediate");
                }
                this.evaluateHighestClass();
            });
        },
        methods: {
            evaluateHighestClass : function() {
                if (this.classesAffected.length == 0) {
                    this.currentClass = 0;
                    if (typeof this.i_i_c !== 'undefined') {
                        this.signalIntermediates();
                    } else {
                        this.signalSeverity();
                    }
                    return;
                }
                let highestClass = this.classesAffected[0];
                let highestBefore = this.currentClass;
                for (let i = 0; i < this.classesAffected.length; i++) {
                    if (highestClass < this.classesAffected[i]) {
                        highestClass = this.classesAffected[i];
                    }
                }
                this.currentClass = highestClass;
                console.log("Highest class is " + this.currentClass);
                /* This intermediate has detected a different highest class
                in accordance with their effect group(s), we should signal
                the severity group*/
                if (highestBefore != this.currentClass) {
                    console.log("Change in this controller's highest detected class, signalling end class");
                    if (typeof this.i_i_c !== 'undefined') {
                        this.signalIntermediates();
                    } else {
                        this.signalSeverity();
                    }
                }
            },
            signalIntermediates : function() {
                for (let i = 0; i < this.i_i_c.length; i++) {
                    if (this.i_i_c[i].class_trigger == this.currentClass || this.i_i_c[i].class_trigger == 'any') {
                        if (this.$parent.lastSentPayload != null) {
                            this.$parent.deactivateOtherIntermediate();
                            this.$parent.activateOtherIntermediate(this.i_i_c[i], this.currentClass);
                        } else {
                            this.$parent.activateOtherIntermediate(this.i_i_c[i], this.currentClass);
                        }
                    }
                }
                this.signalSeverity();
            },
            signalSeverity : function() {
                EventBus.$emit(this.severity_system, this.currentClass);
            }
        }
    }
</script>

<style />
