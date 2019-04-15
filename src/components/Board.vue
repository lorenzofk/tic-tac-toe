<template>
  <div>
    <div class="gameStatus" :class="gameStatusClass">
        {{ gameStatusMessage }}
    </div>
    <table class="board">
      <tr>
        <Cell name="1"></Cell>
        <Cell name="2"></Cell>
        <Cell name="3"></Cell>
      </tr>
      <tr>
        <Cell name="4"></Cell>
        <Cell name="5"></Cell>
        <Cell name="6"></Cell>
      </tr>
      <tr>
        <Cell name="7"></Cell>
        <Cell name="8"></Cell>
        <Cell name="9"></Cell>
      </tr>
    </table>
  </div>
</template>

<script>

import Cell from '@/components/Cell.vue';

export default {
  name: 'Board',
  components: { Cell },
  data() {
    return {
      gameStatus: 'O',
      gameStatusMessage: 'O`s turn',
      gameStatusClass: 'statusTurn',
      currentPlayer: 'O',
      moves: 0,
      cells: {
          1: '', 
          2: '', 
          3: '',
          4: '', 
          5: '', 
          6: '',
          7: '', 
          8: '', 
          9: ''
      },
      winConditions: [
          [1, 2, 3], // row
          [4, 5, 6], // row
          [7, 8, 9], // row
          [1, 4, 7], // column
          [2, 5, 8], // column
          [3, 6, 9], // column
          [1, 5, 9], // diagonal
          [3, 5, 7]  // diagonal
      ],
    }
  },
  created() {
    // Listener to get every move in the current game
    Event.$on('mark', (cellNumber) => {
      this.cells[cellNumber] = this.currentPlayer;
      this.moves++;
      this.gameStatus = this.changeGameStatus();
      this.changePlayer();
    });

    // Listener to reset the board when the game restarts
    Event.$on('resetBoard', () => {  
      Object.assign(this.$data, this.$options.data())
    });
  },
  computed: {
    nextPlayer() {
      return this.playerIsHuman ? 'X' : 'O';
    },
    playerIsHuman() {
      return this.currentPlayer === 'O';
    }
  },
  watch: {
    gameStatus() {
      switch (this.gameStatus) {
        case 'win':
          this.gameStatusMessage = 'You win :)'.toUpperCase();
          this.gameStatusClass = 'statusWin';
          break;
        case 'lost':
          this.gameStatusMessage = 'You lost :/'.toUpperCase();
          this.gameStatusClass = 'statusLost';
          break;
        case 'draw':
          this.gameStatusMessage = 'Draw :|'.toUpperCase();
          this.gameStatusClass = 'statusDraw';
          break;
        default:
          this.gameStatusMessage = this.gameStatus + '\'s turn';
      }

    },
  },
  methods: {
    changePlayer() {
      this.currentPlayer = this.nextPlayer;
    },
    changeGameStatus() {
      if (this.hasWinner()) {
          Event.$emit('winner', this.currentPlayer);
          Event.$emit('finish');
          return this.playerIsHuman ? 'win' : 'lost';
      } else if (this.moves === 9) {
          return 'draw';
      }

      return this.nextPlayer;
    },
    hasWinner() {

      for (let i = 0; i < this.winConditions.length; i++) {
        let wc = this.winConditions[i];
        
        if (this.checkWinConditions(wc)) {
          return true;
        }
        
      }

      return false;
    },
    checkWinConditions(conditions) {
      let playerPositions = this.filledCellsByPlayer(this.currentPlayer);

      if (
        playerPositions.includes(conditions[0].toString()) 
        && playerPositions.includes(conditions[1].toString()) 
        && playerPositions.includes(conditions[2].toString())
      ) {
        return true;
      }

      return false;
    },
    filledCellsByPlayer(player) {
      let positions = [];

      Object.keys(this.cells).forEach(key => {
          if (this.cells[key] === player) {
            positions.push(key);
          }
      });
      
      return positions;
    }
  },
}

</script>

<style scoped>
.board {
  background-color: #34495e;
  color: #fff;
  width: 100%;
  border-collapse: collapse;
}
.gameStatus {
  margin: 0px;
  padding: 15px;
  border-top-left-radius: 20px;
  border-top-right-radius: 20px;
  color: #fff;
  font-size: 1.4em;
  font-weight: bold;
}
.statusTurn {
    background-color: #904679;
}
.statusWin {
    background-color: #2ecc71;
}
.statusDraw {
    background-color: #b59b33;
}
.statusLost {
    background-color: #cc113c;
}
</style>
