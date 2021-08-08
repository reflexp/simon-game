<template>  
  <Simon/>
</template>

<script>
import 'animate.css';
import Simon from './components/Simon.vue';

export default {
  /* Component's name */
  name: 'App',

  /* Components */
  components: {
    Simon
  }
}
</script>

<style lang="scss">
  *, *::before, *::after {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
    font-family: 'Montserrat', sans-serif;
  }

  body {
    width: 100vw;
    height: 100vh;
    display: flex;
    padding: 1rem;
    overflow: hidden;
    align-items: center;
    justify-content: center;
  }
</style>
