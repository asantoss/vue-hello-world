<template>
  <div class="container">
    <h2 v-if="gameOver">
      Game Over!
      <br />
      {{`🚀${winner}`}} wins!
    </h2>
    <h3 v-else>{{currentPlayer}}'s Turn!</h3>
    <button @click="resetBoard" type="reset">Reset</button>
    <div class="row" v-for="(row, i) in board" :key="i">
      <button
        v-bind:class="{active: marked.x === i && marked.y === j, column: true}"
        v-bind:disabled="Boolean(col) || gameOver"
        v-for="(col,j) in row"
        :key="j"
        @click="markColum(i, j)"
      >{{col}}</button>
    </div>
  </div>
</template>

<script>
export default {
  name: "TicTacToe",
  data() {
    return {
      currentPlayer: "X",
      gameOver: false,
      winner: null,
      board: Array.from({ length: 3 }, () => Array.from({ length: 3 })),
      marked: [null, null]
    };
  },
  methods: {
    markColum(row, column) {
      if (!this.board[row][column]) {
        this.board.splice(row, 1, [
          ...this.board[row].slice(0, column),
          this.currentPlayer,
          ...this.board[row].slice(column + 1)
        ]);
        // this.board[row][column] = this.currentPlayer;
      } else {
        return;
      }
    },
    resetBoard() {
      this.board = [
        ["", "", ""],
        ["", "", ""],
        ["", "", ""]
      ];
      this.gameOver = false;
      this.winner = null;
    },
    markCell(x, y) {
      return new Promise(resolve => {
        return setTimeout(() => {
          this.marked = { x, y };
          resolve();
        }, 25);
      });
    },
    async checkWinner() {
      let row = 0;
      let column = 0;
      let cornerAsc = 0;
      let cornerDesc = 0;
      let winner = false;
      for (let i = 0; i < this.board.length; i++) {
        //Check the corner from the top right downward!
        if (this.board[i][i] === this.currentPlayer) {
          cornerAsc++;
        } else {
          cornerAsc--;
        }
        //Check the corner from the bottom Right upward
        if (this.board[i][this.board.length - 1 - i] === this.currentPlayer) {
          cornerDesc++;
        } else {
          cornerDesc--;
        }
        for (let j = 0; j < this.board.length; j++) {
          // await this.markCell(i, j);
          await this.markCell(i, j);
          //Check the current row accros starting from left!
          if (this.board[i][j] === this.currentPlayer) {
            row += 1;
          } else {
            row = 0;
          }

          //Check the column downward starting from the top
          if (this.board[j][i] === this.currentPlayer) {
            column += 1;
          } else {
            column = 0;
          }
        }
        if (
          column === this.board.length ||
          row === this.board.length ||
          cornerAsc === this.board.length ||
          cornerDesc === this.board.length
        ) {
          winner = true;
        }
      }
      return winner;
    }
  },
  watch: {
    board: {
      deep: true,
      handler() {
        this.checkWinner(this.board, this.currentPlayer).then(res => {
          if (res) {
            this.winner = this.currentPlayer;
            this.gameOver = !this.gameOver;
          } else if (this.board.every(e => e.every(e => e))) {
            this.gameOver = !this.gameOver;
            this.winner = "no one";
          }
          this.marked = { x: null, y: null };
          this.currentPlayer = this.currentPlayer === "X" ? "O" : "X";
        });
      }
    }
  }
};
</script>

<style  scoped>
.container {
  display: flex;
  align-items: center;
  justify-content: center;
  flex-direction: column;
}
.row {
  display: flex;
  margin: auto;
  flex-wrap: wrap;
}
.column {
  margin: 5px 5px;
  flex-basis: 25%;
  height: 50px;
  width: 50px;
}
.column:disabled {
  font-weight: bold;
  color: black;
  border: 1px solid red;
}
.active {
  border: 4px solid yellow !important;
  background-color: rebeccapurple;
}
</style>

