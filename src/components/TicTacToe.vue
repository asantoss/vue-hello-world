<template>
  <div class="container">
    <h4 v-if="gameOver">Game Over {{winner}} wins!</h4>
    <h3 v-else>{{currentPlayer}}'s Turn!</h3>
    <button @click="resetBoard" type="reset">Reset</button>
    <div class="row" v-for="(row, i) in board" :key="i">
      <button
        v-bind:disabled="Boolean(col) || gameOver"
        class="column"
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
      size: 3,
      board: Array.from({ length: 3 }, () => Array.from({ length: 3 }))
    };
  },
  methods: {
    markColum(row, column) {
      if (!this.board[row][column]) {
        this.board[row][column] = this.currentPlayer;
      } else {
        return;
      }
      if (this.checkWinner(this.board, this.currentPlayer)) {
        this.winner = this.currentPlayer;
        this.gameOver = !this.gameOver;
      } else if (this.board.every(e => e.every(e => e))) {
        this.gameOver = !this.gameOver;
        this.winner = "no one";
      }
      this.currentPlayer = this.currentPlayer === "X" ? "O" : "X";
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
    checkWinner() {
      let row = 0;
      let column = 0;
      let cornerAsc = 0;
      let cornerDesc = 0;
      for (let i = 0; i < this.board.length; i++) {
        if (this.board[i][i] === this.currentPlayer) {
          cornerAsc++;
        } else {
          cornerAsc--;
        }
        if (this.board[i][this.board.length - 1 - i] === this.currentPlayer) {
          cornerDesc++;
        } else {
          cornerDesc--;
        }
        for (let j = 0; j < this.board.length; j++) {
          if (this.board[i][j] === this.currentPlayer) {
            row += 1;
          } else {
            row = 0;
          }
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
          return true;
        }
      }
      //   for (let i = 0; i < this.board.length; i++) {
      //     const col = this.board[i][i];
      //     if (col === this.currentPlayer) {
      //       items += 1;
      //     } else {
      //       items = 0;
      //     }
      //     if (items === 3) {
      //       return true;
      //     }
      //   }
      //   for (let i = 0; i < this.board.length; i++) {
      //     const col = this.board[i][0];
      //     if (col === this.currentPlayer) {
      //       items += 1;
      //     } else {
      //       items = 0;
      //     }
      //     if (items === this.board.length) {
      //       return true;
      //     }
      //   }
      //   for (let i = 0; i < this.board.length; i++) {
      //     const col = this.board[i][this.board.length - 1 - i];
      //     if (col === this.currentPlayer) {
      //       items += 1;
      //     } else {
      //       items = 0;
      //     }
      //     if (items === this.board.length) {
      //       return true;
      //     }
      //   }
      //   for (let i = 0; i < this.board.length; i++) {
      //     for (let j = 0; j < this.board.length; j++) {
      //       const col = this.board[j][i];
      //       console.log({ x: i, y: j, col });
      //       if (col === this.currentPlayer) {
      //         items += 1;
      //       } else {
      //         items = 0;
      //       }
      //       if (items === 3) {
      //         return true;
      //       }
      //     }
      //   }
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
</style>