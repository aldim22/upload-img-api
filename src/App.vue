<template>
  <div class="container">
    <img alt="Vue logo" src="./assets/logo.png">
    <HelloWorld msg="IMG API"/>
    <div>
      <image-upload></image-upload>
    </div>
  </div>
  
</template>

<script>
import HelloWorld from './components/HelloWorld.vue'
import ImageUpload from './components/ImageUpload.vue';
export default {
  name: 'App',
  components: {
    HelloWorld,
    ImageUpload
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
