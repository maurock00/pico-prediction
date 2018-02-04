<template>
  <div id="app">
    <app-form></app-form>
  </div>
</template>

<script>

import Form from './Form.vue';

export default {
  name: 'app',
  data () {
    return {

    }
  }, 
  components: { 
    appForm: Form
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

h1 {
  font-weight: normal;
}
