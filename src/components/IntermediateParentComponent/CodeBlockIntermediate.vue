<template>
    <div />
</template>

<script>
    import EventBus from '../../event-bus.js';
    export default {
        name: "CodeBlockIntermediate",
        props: ["name", "priority", "pertaining_block", "severity_system", "other_intermediate", "i_i_c"],
        data() {
            return {
                commVal : 0.0,
                affectorsActive : [],
                contributionsAffected : []
            }
        },
        mounted() {
            EventBus.$on((this.name + ".activate"), (message) =>  {
                let affector = this.$parent.decodeUniqueIdentifier(message);
                let contributeAffect = this.$parent.decodeDecimalAffectMaybe(message);
                this.affectorsActive.push(affector);
                this.contributionsAffected.push(contributeAffect);
                // Determine method to execute from pertaining_block
                this[this.pertaining_block]();
                // make sure to signal other intermediates as well as severity
            });
            EventBus.$on((this.name + ".deactivate"), (message) =>  {
                let affector = this.$parent.decodeUniqueIdentifier(message);
                let indexToRemove = this.affectorsActive.indexOf(affector);
                if (indexToRemove != -1) {
                    this.affectorsActive.splice(indexToRemove, 1);
                    this.contributionsAffected.splice(indexToRemove, 1);
                } else {
                    console.log("Something went wrong: Affect " + affector + " was not logged as active within intermediate");
                }
                // Determine method to execute from pertaining_block
                // make sure to signal other intermediates as well as severity
            });
        },
        methods: {

            /* I have included an 'any' option in the trigger section of a given
            i_i_c as well as a 'self' option - the 'any' option means that
            regardless of the stored value for the intermediate (currentState,
            currentThreshold, etc.), intermediate-intermediate communication
            will take place. The 'self' option means that, when interpreted by
            the parent, the child intermediate is called on to gather the
            stored currentState, currentThreshold, etc., and is used as the
            communicated value to the other intermediate - these two control
            strings in intermediate-intermediate communication mean that we can
            have a codeblock intermediate that informs another codeblock of its
            stored communication value whenever it is changed. This behaviour
            is attractive to regressive severity schemes or interacting
            codeblock intermediates where one's output is a consistent input
            to another*/

            baseCalculator : function() {
                let lookup = [
                    {name : "Confidentiality", value : null},
                    {name : "Integrity", value : null},
                    {name : "Availability", value : null},
                    {name : "Scope", value : null},
                    {name : "Attack Vector", value : null},
                    {name : "Attack Complexity", value : null},
                    {name : "Privileges Required", value : null},
                    {name : "User Interaction", value : null}
                ];

                // Find the variables
                let variables = this.findVariables(lookup);
                console.log(variables);

                // Store each variable from the lookup
                let confidentiality = parseFloat(lookup.find(x => x.name == "Confidentiality").value);
                let integrity = parseFloat(lookup.find(x => x.name == "Integrity").value);
                let availability = parseFloat(lookup.find(x => x.name == "Availability").value);
                let scope = (lookup.find(x => x.name == "Scope").value);
                let attack_vector = parseFloat(lookup.find(x => x.name == "Attack Vector").value);
                let privileges_required = parseFloat(lookup.find(x => x.name == "Privileges Required").value);
                let attack_complexity = parseFloat(lookup.find(x => x.name == "Attack Complexity").value);
                let user_interaction = parseFloat(lookup.find(x => x.name == "User Interaction").value);

                // Formulate the calculation, remember, each value may only
                // keep track of commVal, not the rest of the values - this is
                // where we transmit via intermediate-intermediate signalling

                /* WARNING: Source of magic numbers and the formula itself
                in this example comes from:
                https://www.first.org/cvss/specification-document */

                /* WARNING check for floating point inaccuracy if things start
                to go wrong!*/

                //console.log(parseFloat(lookup.find(x => x.name == "Confidentiality").value));

                let iss = (1 - (1 - confidentiality) * (1 - integrity) * (1 - availability));
                let impact = 0;
                if (scope == 'Unchanged') {
                    console.log("Scope Unchanged!");
                    impact = (6.42 * iss);
                } else {
                    console.log("Scope Changed!");
                    impact = (7.52 * ((iss - 0.029) - 3.25) * (Math.pow((iss * 0.02), 15)));
                }
                console.log("Impact = " + impact)
                let exploitability = (8.22 * attack_vector * attack_complexity * privileges_required * user_interaction);
                let baseScore = 0;
                if (impact <= 0) {
                    baseScore = 0;
                } else if (scope == 'Unchanged') {
                    baseScore = Math.ceil(Math.min((impact + exploitability), 10));
                } else if (scope == 'Changed') {
                    baseScore = Math.ceil(Math.min((1.08 * (impact + exploitability)), 10));
                }
                this.commVal = baseScore;
                console.log("commVal: " + this.commVal);
                /*Communicate the commVal to other intermediates if
                intermediate-intermediate control exists*/
                if (typeof this.i_i_c !== 'undefined') {
                    this.signalIntermediates();
                } else {
                    this.signalSeverity();
                }
            },

            temporalCalculator : function() {
            },

            environmentalCalculator : function() {
            },

            findVariables : function(lookup) {
                let a = 0;
                for (let i = 0; i < this.affectorsActive.length; i++) {
                    for (let j = 0; j < lookup.length; j++) {
                        // Get the value before the '.'
                        let parts = this.affectorsActive[i].split(".");
                        let questionValue = parts[0];
                        if (lookup[j].name == questionValue) {
                            //console.log("before: " + );
                            lookup[j].value = this.contributionsAffected[i];
                            a++;
                        }
                    }
                }
                if (a != lookup.length) {
                    console.log("Not all values filled - filling nulls with 0");
                    return this.fillLookupNullsWithZero(lookup);
                } else {
                    return lookup;
                }
            },

            fillLookupNullsWithZero : function(lookup) {
                for (let i = 0; i < lookup.length; i++) {
                    if (lookup[i].value == null) {
                        lookup[i].value = "0";
                    }
                }
                return lookup;
            },

            signalIntermediates : function() {
                for (let i = 0; i < this.i_i_c.length; i++) {
                    if (this.i_i_c[i].contribution_trigger == this.commVal || this.i_i_c[i].contribution_trigger == 'any') {
                        if (this.$parent.lastSentPayload != null) {
                            this.$parent.deactivateOtherIntermediate();
                            this.$parent.activateOtherIntermediate(this.i_i_c[i], this.commValue);
                        } else {
                            this.$parent.activateOtherIntermediate(this.i_i_c[i], this.commValue);
                        }
                    }
                }
                this.signalSeverity();
            },
            signalSeverity : function() {
                console.log("Sending to " + this.severity_system + " " + this.commVal);
                EventBus.$emit(this.severity_system, this.commVal);
            }
        }
    }
</script>

<style />
