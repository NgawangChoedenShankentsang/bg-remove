<template>
  <div id="app">
    <BackgroundRemoval />
  </div>
</template>

<script>
import BackgroundRemoval from './components/BackgroundRemoval.vue';

export default {
  name: 'App',
  components: {
    BackgroundRemoval
  }
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
