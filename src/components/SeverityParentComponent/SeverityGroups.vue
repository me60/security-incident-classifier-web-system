<template>
    <div id="severity_groups" class="severity_groups">
        <div v-for="severity_group in severity_groups" :key="severity_group.name">
            <div class="severity_name">{{severity_group.display_name}}</div>
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
        position: sticky;
        bottom: 0;
        background: #333;
        color: #fff;
        text-align: center;
        padding: 10px;
    }
    .severity_name {
        font-size: 16px;
    }
</style>
