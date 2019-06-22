<template>
  <div>
    <h1>Color Game!</h1>
    <button @click="initBoard()">Start</button>
    <button @click="newGame()">New Game</button>

    <div id="game" v-if="board.length">
      <ul class="demo">
        <li class="column" v-for="row in board">
          <ul class="row">
            <li
              :x="cell.x"
              :y="cell.y"
              @click="cellClick($event,cell)"
              class="cell"
              v-for="cell in row"
              v-bind:style="{ backgroundColor: cell.color }"
            >{{ cell.axis }}</li>
          </ul>
        </li>
      </ul>
      <div>
        <label>Moves: {{ counter }}</label>
        <p>siblings: {{ siblings }}</p>
        <p v-bind:style="{ backgroundColor: activeColor }" >Active Color: {{ activeColor }}</p>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "app",
  data() {
    return {
      dimension: 14,
      activeColor: '',
      board: [],
      counter: 0,
      siblings: []
    };
  },
  created: function() {
    this.board = this.initBoard(this.dimension);
    this.activeColor = this.board[0][0].color
  },
  methods: {
    initBoard: dimension => {
      var colors = ["#ff0000", "#3399ff", "#00ffcc", "#ffff66"];
      var x = new Array(dimension);
      for (var i = 0; i < x.length; i++) {
        x[i] = new Array(dimension).fill(0).map((element, index) => {
          return {
            axis: `${i}.${index}`,
            x: i,
            y: index,
            color: colors[Math.floor(Math.random() * colors.length)]
          };
        });
      }
      return x;
    },
    newGame() {
      this.reset();
      this.board = this.initBoard(this.dimension);
      this.prepareBoard()
    },
    cellClick(event, clickedCellData) {
      console.log(clickedCellData);
      this.findSiblings(clickedCellData);
      this.counter++;
    },
    reset() {
      this.counter = 0;
      this.siblings = [];
    },
    findSiblings(cell) {
     this.activeColor = cell.color
        this.board[cell.x+1][cell.y].color = cell.color
        this.board[cell.x-1][cell.y].color = cell.color
        this.board[cell.x][cell.y+1].color = cell.color
        this.board[cell.x][cell.y-1].color = cell.color
      
    },
    prepareBoard(){
        this.activeColor = this.board[0][0].color
    }
  }
};
</script>

<style>
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

#game {
  padding: 10px;
  margin: 0 auto;
}

.column {
  padding: 0;
  display: inline-block;
  position: relative;
  list-style: none;
}

.row {
  padding: 0;
  display: block;
  position: relative;
  list-style: none;
}

.cell {
  padding: 20px;
}
</style>

