<template>
    <div>
        <div v-if="p_q_type === 'multipick'">
            <Multipick :name="name" :p_q_name="p_q_name" />
        </div>
        <div v-else>
            <Onepick :name="name" :p_q_name="p_q_name" />
        </div>
    </div>
</template>

<script>
    import Multipick from './Multipick.vue';
    import Onepick from './Onepick.vue';
    import EventBus from '../../event-bus.js';

    export default {
        name: "Button",
        props: ["p_q_type", "p_q_name", "name", "basic_trigger"],
        components: {
            Multipick,
            Onepick
        },
        data() {
            return {
                activated : false
            }
        },
        mounted() {
            /* Only the activated onepick button in this EffectGroup will
            trigger on this emitted event, however, it is sent to all*/
            EventBus.$on('deactivateOnepick', (p_q_name) => {
                if (this.p_q_type === "onepick" && this.p_q_name === p_q_name && this.activated) {
                    this.deactivate();
                }
            });
        },
        methods: {
            activate : function() {
                this.activated = true;
                this.activateTrigger();
                this.$parent.activate();
            },
            deactivate : function() {
                this.activated = false;
                this.$parent.deactivate();
            },
            activateTrigger : function() {
                if (this.basic_trigger != undefined) {
                    window.alert(this.basic_trigger);
                }
            }
        }
    }
</script>

<style />
