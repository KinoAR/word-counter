<template>
  <div class="container">
    <div id="app">
      <Main/>
    </div>
    <Footer />
  </div>
</template>

<script>
import Main from './components/Main';
import Footer from "./components/Footer";

export default {
  name: 'App',
  components: {
    Main,
    Footer
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
</style>
