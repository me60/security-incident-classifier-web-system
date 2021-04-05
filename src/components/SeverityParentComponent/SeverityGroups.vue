<template>
    <div id="severity_groups" class="severity_groups">
        <div v-for="severity_group in severity_groups" :key="severity_group.name">
            <h3>{{severity_group.display_name}}</h3>
            <SeverityGroup :name="severity_group.name" :type="severity_group.type" :severities="severity_group.severities" :severity_intermediate_control="severity_group.severity_intermediate_control" />
        </div>
    </div>
</template>

<script>
    import SeverityGroup from './SeverityGroup.vue'
    export default {
        name: "SeverityGroups",
        components: {
          SeverityGroup
        },
        props: ["severity_groups"]
    }
</script>

<style>
    .severity_groups {
        background: #333;
        color: #fff;
        text-align: center;
        padding: 10px;
    }
</style>
