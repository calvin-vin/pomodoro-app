<template>
  <Pomodoro />
</template>

<script>
import Pomodoro from './components/Pomodoro.vue'

export default {
  name: 'App',
  components: {
    Pomodoro
  }
}
</script>

<style>
#app {
  font-family: Roboto, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
