<template>
  <div>
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
  data: () => ({
    board: [
      ["", "", ""],
      ["", "", ""],
      ["", "", ""]
    ],
    currentPlayer: "X",
    gameOver: false,
    winner: null
  }),
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
      let items = 0;
      for (let i = 0; i < this.board.length; i++) {
        const row = this.board[i];
        for (let j = 0; j < row.length; j++) {
          const col = row[j];
          console.log({ x: i, y: j, items });
          if (col === this.currentPlayer) {
            items += 1;
          } else {
            items = 0;
          }
        }
        if (items === 3) {
          return true;
        }
      }
      for (let i = 0; i < this.board.length; i++) {
        const col = this.board[i][i];
        if (col === this.currentPlayer) {
          items += 1;
        } else {
          items = 0;
        }
        if (items === 3) {
          return true;
        }
      }
      for (let i = 0; i < this.board.length; i++) {
        const col = this.board[i][0];
        if (col === this.currentPlayer) {
          items += 1;
        } else {
          items = 0;
        }
        if (items === 3) {
          return true;
        }
      }
      for (let i = 0; i < this.board.length; i++) {
        const col = this.board[i][this.board.length - 1 - i];
        if (col === this.currentPlayer) {
          items += 1;
        } else {
          items = 0;
        }
        if (items === 3) {
          return true;
        }
      }
    }
  }
};
</script>

<style  scoped>
.row {
  display: flex;
  margin: auto;
  width: 150px;
}
.column {
  width: 50px;
  height: 50px;
}
.column:disabled {
  font-weight: bold;
  color: black;
  border: 1px solid red;
}
</style>