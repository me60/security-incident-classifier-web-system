<template>
    <div>
        <input type="checkbox" :name="`group${p_q_name}`" :id="`group${p_q_name}`" @change="changeHandler()" /> {{this.name}}
    </div>
</template>

<script>
    export default {
        name: "Multipick",
        props: ["name", "p_q_name"],
        methods: {
            changeHandler : function() {
                console.log("Clicked multipick");
                if (this.$parent.activated == false) {
                    this.$parent.activate();
                } else {
                    this.$parent.deactivate();
                }
            }
        }
    }
</script>

<style />
