<template>
  <div id="app">
    <BgRemoveNavBar />
    <BackgroundRemoval class="spaced"/>
  </div>
</template>

<script>
import BgRemoveNavBar from './components/BgRemoveNavBar.vue';
import BackgroundRemoval from './components/BackgroundRemoval.vue';

export default {
  name: 'App',
  components: {
    BgRemoveNavBar,
    BackgroundRemoval
  }
};
</script>

<style>
#app {
  font-family: "Inter", sans-serif;
  text-align: center;
  color: #2c3e50;
}

.spaced {
  margin-top: 80px; /* Adds space below the first component */
}
</style>
