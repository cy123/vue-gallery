<template>
  <div id="app">
    <Gallery></Gallery>
  </div>
</template>

<script>

import Gallery from './views/Gallery'
export default {
  name: 'App',
  components: {
    Gallery
  }
}
</script>

<style>
html,body{
  padding: 0;
  margin: 0;
}

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;

}
</style>
