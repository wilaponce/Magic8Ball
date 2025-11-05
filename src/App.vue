
<template>
  <div id="app">
    <Magic8Ball />
  </div>
</template>

<script>
import Magic8Ball from './components/Magic8Ball.vue';

export default {
  components: {
    Magic8Ball
  }
};
</script>

<style>
#app {
  text-align: center;
  background-color: #111;
  color: white;
  height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
}
</style>
