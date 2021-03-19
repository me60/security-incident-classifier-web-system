<template>
    <div />
</template>

<script>
    import EventBus from '../../event-bus.js';
    export default {
        name: "CounterIntermediate",
        props: ["name", "thresholds", "priority"],
        data() {
            return {
                // Count inits at 0, behaviour starts at 1
                currentCount : 0,
                affectorsActive: [],
                countContributed: []
            }
        },
        mounted() {
            EventBus.$on((this.name + ".activate"), (message) =>  {
                let affector = this.$parent.decodeUniqueIdentifier(message);
                console.log(affector);
                
                let countAffect = this.$parent.decodeAffect(message);
                console.log(countAffect);
            });
            EventBus.$on((this.name + ".deactivate"), (message) =>  {
                let affector = this.$parent.decodeUniqueIdentifier(message);
                console.log(affector);
                let countAffect = this.$parent.decodeAffect(message);
                console.log(countAffect);
            });
        }
    }
</script>

<style />
