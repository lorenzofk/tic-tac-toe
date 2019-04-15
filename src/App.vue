<template>
  <div>
    <div class="scoreBoard">
      <span>You have {{ results.O }} wins</span>
      <h2>Score</h2>
      <span>Machine has {{ results.X }} wins</span>
    </div>
    <div id="app" v-cloak>
      <div id="details">
        <h2>MATCH #{{ numberOfMatches }}</h2>
      </div>
      <Board></Board>
      <button class="restart" @click="restart">Restart</button>
    </div>
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
  created() {
    // Listener to set the score for the winner
    Event.$on('winner', (winner) => this.results[winner]++);
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
.scoreBoard {
  display: flex;
  flex-direction: row;
  justify-content: space-around;
  align-items: center;
  width: 100%;
  height: 15px;
  background-color: #3684d2;
  box-shadow: 10px solid #fff;
  padding: 20px 0px 20px 0px;
  overflow-x: none;
}
.scoreBoard h2 {
  margin: 0px;
}
.scoreBoard span {
  float: right;
  font-size: 1.5em;
  font-weight: bold;
  margin-left: 20px;
}
</style>
