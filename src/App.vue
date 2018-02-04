<template>
  <div id="app">
    <link rel="stylesheet" href="css/foundation.min.css">
    <h1 class="text-center">{{ title }}</h1>
    <div>
      <app-form></app-form>
    </div>
  </div> 
</template>

<script>

import Form from './Form.vue';

export default {
  name: 'app',
  data () {
    return {
      title: '"Pico y Placa" Predictor'
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
