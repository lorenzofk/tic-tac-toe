<template>
  <td class="cell" @click="mark">{{ symbol }}</td>
</template>

<script>
export default {
  name: 'Cell',
  data() {
    return {
      symbol: '',
      empty: true
    }
  },
  methods: {
    mark() {
      if (this.empty) {
        this.symbol = this.$parent.currentPlayer;
        this.empty = false;
        Event.$emit('mark', this.$attrs.name);
      }
    },
    clearCell() {
      this.symbol = '';
      this.empty = true;
    },
    blockCell() {
      this.empty = false;
    }
  },
  created() {
    // Listener to clear the cell when the game restarts
    Event.$on('clearCell', () => this.clearCell());
    
    // Listener to block cells when the game is finished
    Event.$on('finish', () => this.blockCell());
  }
}
</script>

<style scoped>
.cell {
  width: 33.333%;
  height: 90px;
  border: 6px solid #2c3e50;
  font-size: 3.5em;
  font-family: 'Gochi Hand', sans-serif;
}
.cell:hover {
    background-color: #7f8c8d;
}
.cell::after {
  content: '';
  display: block;
}
.cell:first-of-type {
  border-left-color: transparent;
  border-top-color: transparent;
}
.cell:nth-of-type(2) {
  border-top-color: transparent;
}
.cell:nth-of-type(3) {
  border-right-color: transparent;
  border-top-color: transparent;
}
tr:nth-of-type(3) .cell {
  border-bottom-color: transparent;
}
</style>
