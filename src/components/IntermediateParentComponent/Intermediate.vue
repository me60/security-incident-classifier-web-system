<template>
    <div id="intermediate">
        <div v-if="type === 'inc'">
            <IncrementIntermediate :classes="classes" :name="name" :priority="priority" :severity_system="severity_system" :other_intermediate="other_intermediate" :i_i_c="i_i_c" />
        </div>
        <div v-if="type === 'bin'">
            <BinaryIntermediate :name="name" :priority="priority" :severity_system="severity_system" :other_intermediate="other_intermediate" :i_i_c="i_i_c" />
        </div>
        <div v-if="type === 'count'">
            <CounterIntermediate :name="name" :thresholds="thresholds" :priority="priority" :severity_system="severity_system" :other_intermediate="other_intermediate" :i_i_c="i_i_c"/>
        </div>
    </div>
</template>

<script>
    import EventBus from '../../event-bus.js';
    import IncrementIntermediate from './IncrementIntermediate.vue';
    import CounterIntermediate from './CounterIntermediate.vue';
    import BinaryIntermediate from './BinaryIntermediate.vue';
    export default {
        name: "Intermediate",
        // i_i_c = intermediate_intermediate_control
        props: ["name", "type", "classes", "thresholds", "priority", "severity_system", "other_intermediate", "i_i_c"],
        components: {
            IncrementIntermediate,
            BinaryIntermediate,
            CounterIntermediate
        },
        data() {
            return {
                lastSentPayload : null
            }
        },
        methods: {
            activateOtherIntermediate : function(i_i_c_entry) {
                // At this stage, other intermediate signal is required
                let affector = null;
                if (i_i_c_entry.weight != null) {
                    affector = i_i_c_entry.weight;
                }
                if (i_i_c_entry.state != null) {
                    affector = i_i_c_entry.state;
                }
                if (i_i_c_entry.class != null) {
                    affector = i_i_c_entry.class;
                }
                console.log("Signalling: " + this.other_intermediate);
                let payload = ("I." + this.name + "." + affector);
                // !!! Intermediates may ONLY have one OTHER intermediate they
                // can signal (for now)
                EventBus.$emit((this.other_intermediate + ".activate"), payload);
                this.lastSentPayload = payload;
            },
            deactivateOtherIntermediate : function() {
                // If this is called, an activation has been sent before that
                // has been updated in that another signal is required, we
                // must deactivate the previous activation
                // At this stage, other intermediate signal is required
                console.log("Signalling: " + this.other_intermediate);
                EventBus.$emit((this.other_intermediate + ".deactivate"), this.lastSentPayload);
            },
            decodeUniqueIdentifier : function(message) {
                let parts = message.split(".");
                return parts[0] + "." + parts[1];
            },
            decodeAffect : function(message) {
                let parts = message.split(".");
                return parts[parts.length-1];
            }
        }
    }
</script>

<style />
