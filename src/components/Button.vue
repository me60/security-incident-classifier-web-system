<template>
    <div>
        <div v-if="p_q_type === 'multipick'">
            <Multipick :name="name" />
        </div>
        <div v-else>
            <Onepick :name="name" />
        </div>
    </div>
</template>

<script>
    import Multipick from './Multipick.vue';
    import Onepick from './Onepick.vue';
    export default {
        name: "Button",
        props: ["p_q_type", "name"],
        components: {
            Multipick,
            Onepick
        },
        data() {
            return {
                activated : false
            }
        },
        methods: {
            determineActivation : function () {
                //this.activated = !(this.activated);
                this.activated == true ? this.activate() : this.deactivate();
            },
            activate : function() {
                this.$parent.activate();
            },
            deactivate : function() {
                this.$parent.deactivate();
            }
        }
    }
</script>

<style />
