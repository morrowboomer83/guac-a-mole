<template>
  <h1>Guac-a-Mole</h1>
  <hr/>
  <Welcome v-if="appPhase === 'welcome'" @getready="getReady" />
  <GetReady v-if="appPhase === 'getReady'" :delay="getReadyDuration" @runmain="runMain" />
  <GameMain v-if="appPhase === 'playing'" @gameover="gameOver" />
  <GameOver v-if="appPhase === 'gameOver'" :win="win" @restart="restart"/>
</template>

<script>
import Welcome from './components/Welcome.vue'
import GetReady from './components/GetReady.vue'
import GameMain from './components/GameMain.vue'
import GameOver from './components/GameOver.vue'

export default {
  name: 'App',
  components: {
    Welcome,
    GetReady,
    GameMain,
    GameOver
  },
  data() {
    return {
      appPhase: 'welcome',
      getReadyDuration: null,
      win: false
    }
  },
  methods: {
    getReady() {
      this.win = false
      this.appPhase = 'getReady'
      this.getReadyDuration = 2000 + Math.random() * 5000
    },
    runMain() {
      this.appPhase = 'playing'
    },
    gameOver(isWin) {
      this.win = isWin
      this.appPhase = 'gameOver'
    },
    restart() {
      this.appPhase = 'welcome'
    }
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
