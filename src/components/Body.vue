<template>
    
</template>

<script>
import Tables from './Tables.vue';
export default {
  name: 'Body',
  components: {
    Tables
  },
  data() {
    return {
      tables: []
    }
  }
}
</script>

<style scoped>
  * {
    box-sizing: border-box;
    margin: 0;
    padding: 0;
  }
  body {
    font-family: Arial, Helvetica, sans-serif;
    line-height: 1.4;
    background-color: gray;
  }
  .btn {
    display: inline-block;
    border: none;
    background: #555;
    color: #fff;
    padding: 7px 20px;
    cursor: pointer;
  }
  .btn:hover {
    background: #666;
  }
</style>
