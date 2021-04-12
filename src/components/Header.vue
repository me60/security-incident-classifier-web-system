<template>
  <header class="header">
    <h3>{{ this.classifier_name }}</h3>
    <h2>Security Incident Classifier SH Project</h2>
  </header>
</template>

<script>
  export default {
    name: "Header",
    props : ["classifier_name"]
  }
</script>

<style scoped>
    .header {
        background: #333;
        color: #fff;
        text-align: center;
        padding: 10px;
    }
    .header a {
        color: #fff;
        padding-right: 5px;
        text-decoration: none;
    }
</style>
