<template>
  <div id="app">
    <GLTFModel />
  </div>
</template>

<script>
import GLTFModel from './components/GLTFModel.vue';

export default {
  name: 'App',
  components: {
    GLTFModel
  }
};
</script>

<style>
#app {
  width: 100vw;
  height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
}
</style>
