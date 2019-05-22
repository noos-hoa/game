<template>
  <div id="app" class="container">
    <header class="text-center">
      <h1>The Game</h1>
    </header>
    <main>
      <Game :time="timeForClick" :rows="10" :ceils="10" />
    </main>
    <footer>

    </footer>
  </div>
</template>

<script>
import Game from './components/Game.vue'
import 'bootstrap/dist/css/bootstrap.css'
import 'bootstrap-vue/dist/bootstrap-vue.css'

export default {
  name: 'app',
  components: {
    Game
  },
  data(){
    return {
      timeForClick: 200 // ms,
    }
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}
</style>
