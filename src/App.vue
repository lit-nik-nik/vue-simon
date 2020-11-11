<template>
  <div id="simon">
    <h1>Simon Says</h1>
    <Simon />
  </div>
</template>

<script>
import Simon from './components/Simon';

export default {
  name: 'App',
  components: {
    Simon,
  }
}
</script>

<style>
#container {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

h1 {
  text-align: center;
}
</style>
