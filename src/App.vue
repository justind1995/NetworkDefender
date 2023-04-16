
<template>
  <GameScreen msg="Welcome to Network Defender"/>
  <LevelOne/>
</template>

<script>
import GameScreen from './components/GameScreen.vue'
import LevelOne from './components/LevelOne.vue'

export default {
  name: 'App',
  components: {
    GameScreen, LevelOne
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
