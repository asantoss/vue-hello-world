<template>
  <div>
    <h4 v-if="gameOver">Game Over {{winner}} wins!</h4>
    <h3 v-else>{{currentPlayer}}'s Turn!</h3>
    <button @click="resetBoard" type="reset">Reset</button>
    <div class="row" v-for="(row, i) in board" :key="i">
      <button class="column" v-for="(col,j) in row" :key="j" @click="markColum(i, j)">{{col}}</button>
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
      const newBoard = [...this.board];
      newBoard[row][column] = this.currentPlayer;
      this.board = newBoard;
      if (this.checkWinner(this.board, this.currentPlayer)) {
        this.winner = this.currentPlayer;
        this.gameOver = !this.gameOver;
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
    checkWinner(board, currentPlayer) {
      let items;
      for (let i = 0; i < board.length; i++) {
        const row = board[i];
        if (items === currentPlayer.repeat(3)) {
          return true;
        } else {
          items = "";
        }
        for (let j = 0; j < row.length; j++) {
          const col = row[j];
          if (col === currentPlayer) {
            items += col;
          }
        }
      }
      for (let i = 0; i < board.length; i++) {
        const col = board[i][i];
        if (col === currentPlayer) {
          items += col;
        } else {
          items = "";
        }
        if (items === currentPlayer.repeat(3)) {
          return true;
        }
      }

      for (let i = 0; i < board.length; i++) {
        const row = board[i];
        for (let j = 0; j < row.length; j++) {
          const col = board[j][i];
          if (col === currentPlayer) {
            items += currentPlayer;
          } else {
            items = "";
          }
          if (items === currentPlayer.repeat(3)) {
            return true;
          }
        }
      }
    }
  }
};
</script>

<style scoped>
.row {
  display: flex;
  margin: auto;
  width: 150px;
}
.column {
  width: 50px;
  height: 50px;
}
</style>