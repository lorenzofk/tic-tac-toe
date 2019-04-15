<template>
  <div id="app" v-cloak>
    <div id="details">
      <h1>Tic Tac Toe</h1>
      <h2>Match #{{ numberOfMatches }}</h2>
    </div>
    <Board></Board>
    <button class="restart" @click="restart">Restart</button>
  </div>
</template>

<script>

import Board from '@/components/Board.vue';

export default {
  name: 'game',
  components: { Board },
  data() {
    return {
      matches: 0,
      results: {
        O: 0,
        X: 0
      }
    }
  },
  computed: {
    numberOfMatches() {
      return this.matches + 1;
    }
  },
  methods: {
    restart () {
      Event.$emit('clearCell');
      Event.$emit('resetBoard');
      this.matches++;
    }
  }
}
</script>

<style>
body {
  background-color: #fff;
  color: #fff;
  font-family: 'Dosis', Helvetica, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  margin: 0px;
}
#app {
  margin: 0 auto;
  max-width: 600px;
  color: #34495e;
}
h1 {
  text-transform: uppercase;
  font-weight: bold;
  font-size: 2em;
}
.restart {
  background-color: #ce7121;
  color: #fff;
  border: 0px;
  border-bottom-left-radius: 10px;
  border-bottom-right-radius: 10px;
  font-family: 'Dosis', Helvetica, sans-serif;
  font-size: 1.4em;
  font-weight: bold;
  margin: 0px;
  padding: 15px;
  width: 100%;
}
.restart:hover {
  background-color: #b5590a;
  cursor: pointer;
}
</style>
