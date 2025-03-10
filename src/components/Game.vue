<template>
  <div class="game">
    <game-menu :score="score" @newGame="init" />
    <div class="board">
      <div
          v-for="(row, rowIndex) in rows"
          :key="rowIndex"
          class="row"
      >
        <span
            v-for="(cell, colIndex) in row"
            :key="colIndex"
            class="cell"
            :style="{ backgroundColor: getTileColor(cell) }"
        >
          {{ cell > 0 ? cell : '' }}
        </span>
      </div>
    </div>
    <div class="buttons">
      <button class="btn" @click="move('ArrowUp')">&#8593;</button>
      <div class="horizontal-buttons">
        <button class="btn" @click="move('ArrowLeft')">&#8592;</button>
        <button class="btn" @click="move('ArrowRight')">&#8594;</button>
      </div>
      <button class="btn" @click="move('ArrowDown')">&#8595;</button>
    </div>
  </div>
</template>

<script>
import GameMenu from './GameMenu.vue';

export default {
  name: 'Game',
  components: {
    GameMenu,
  },
  data() {
    return {
      cells: [],
      score: 0,
    };
  },
  computed: {
    rows() {
      return [
        this.cells.slice(0, 4),
        this.cells.slice(4, 8),
        this.cells.slice(8, 12),
        this.cells.slice(12, 16),
      ];
    },
  },
  methods: {
    init() {
      this.cells = this.shuffle([...Array(16).fill(0)]);
      this.addCells();
      this.addCells();
      console.log("Initial cells:", this.cells);
    },
    shuffle(arr) {
      for (let i = arr.length - 1; i > 0; i--) {
        const j = Math.floor(Math.random() * (i + 1));
        [arr[i], arr[j]] = [arr[j], arr[i]];
      }
      return arr;
    },
    addCells() {
      let emptyCells = this.cells.map((value, index) => value === 0 ? index : -1).filter(index => index !== -1);
      if (emptyCells.length > 0) {
        let index = emptyCells[Math.floor(Math.random() * emptyCells.length)];
        this.cells[index] = 2;
      } else if (!this.checkForMoves()) {
        alert("Game Over!");
      }
      console.log("After adding cells:", this.cells);
    },
    checkForMoves() {
      for (let i = 0; i < 16; i++) {
        if (this.cells[i] === 0) return true;
        if (i % 4 < 3 && this.cells[i] === this.cells[i + 1]) return true;
        if (i < 12 && this.cells[i] === this.cells[i + 4]) return true;
      }
      return false;
    },
    move(direction) {
      let moved = false;
      for (let k = 0; k < 4; k++) {
        moved = this[`move${direction}`](k) || moved;
      }
      if (moved) {
        this.addCells();
      }
      console.log("Cells after move:", this.cells);
    },
    moveArrowLeft(k) {
      let moved = false;
      for (let i = 0; i < 16; i++) {
        if (this.cells[i] !== 0 && i % 4 !== 0) {
          let prevIndex = i - 1;
          if (this.cells[prevIndex] === this.cells[i]) {
            this.cells[prevIndex] += this.cells[i];
            this.score += this.cells[prevIndex];
            this.cells[i] = 0;
            moved = true;
          } else if (this.cells[prevIndex] === 0) {
            this.cells[prevIndex] = this.cells[i];
            this.cells[i] = 0;
            moved = true;
          }
        }
      }
      return moved;
    },
    moveArrowRight(k) {
      let moved = false;
      for (let i = 15; i >= 0; i--) {
        if (this.cells[i] !== 0 && i % 4 !== 3) {
          let nextIndex = i + 1;
          if (this.cells[nextIndex] === this.cells[i]) {
            this.cells[nextIndex] += this.cells[i];
            this.score += this.cells[nextIndex];
            this.cells[i] = 0;
            moved = true;
          } else if (this.cells[nextIndex] === 0) {
            this.cells[nextIndex] = this.cells[i];
            this.cells[i] = 0;
            moved = true;
          }
        }
      }
      return moved;
    },
    moveArrowUp(k) {
      let moved = false;
      for (let i = 0; i < 16; i++) {
        if (this.cells[i] !== 0 && i >= 4) {
          let prevIndex = i - 4;
          if (this.cells[prevIndex] === this.cells[i]) {
            this.cells[prevIndex] += this.cells[i];
            this.score += this.cells[prevIndex];
            this.cells[i] = 0;
            moved = true;
          } else if (this.cells[prevIndex] === 0) {
            this.cells[prevIndex] = this.cells[i];
            this.cells[i] = 0;
            moved = true;
          }
        }
      }
      return moved;
    },
    moveArrowDown(k) {
      let moved = false;
      for (let i = 15; i >= 0; i--) {
        if (this.cells[i] !== 0 && i < 12) {
          let nextIndex = i + 4;
          if (this.cells[nextIndex] === this.cells[i]) {
            this.cells[nextIndex] += this.cells[i];
            this.score += this.cells[nextIndex];
            this.cells[i] = 0;
            moved = true;
          } else if (this.cells[nextIndex] === 0) {
            this.cells[nextIndex] = this.cells[i];
            this.cells[i] = 0;
            moved = true;
          }
        }
      }
      return moved;
    },
    getTileColor(value) {
      const colors = {
        0: '#ccc0b3',
        2: '#eee4da',
        4: '#ede0c8',
        8: '#f2b179',
        16: '#f59563',
        32: '#f67c5f',
        64: '#edcf72',
        128: '#edcc61',
        256: '#edc650',
        512: '#edc53f',
        1024: '#edc52f',
        2048: '#edc529',
      };
      return colors[value] || '#3c3a32'; // Цвет по умолчанию
    },
  },
  created() {
    this.init(); // Инициализация игры
  },
};
</script>

<style>
.game {
  display: flex;
  flex-direction: column;
  align-items: center;
}

.board {
  display: flex;
  flex-direction: column;
  background-color: #d0d6da;
  border-radius: 8px;
  margin: 20px 0;
}

.row {
  display: flex;
}

.cell {
  width: 140px; /* ширина плитки */
  height: 140px; /* высота плитки */
  display: flex;
  justify-content: center;
  align-items: center;
  font-size: 34px;
  margin: 5px;
  border-radius: 5px;
  color: #fff;
  font-weight: bold;
}

.buttons {
  display: flex;
  flex-direction: column;
  align-items: center;
}

.horizontal-buttons {
  display: flex;
  justify-content: center;
}

.btn {
  width: 80px;
  height: 80px;
  font-size: 36px;
  color: white;
  background-color: #3498db; /* Цвет кнопки */
  border: none;
  cursor: pointer;
  border-radius: 5px;
  margin: 5px;
  transition: background-color 0.3s;
}

.btn:hover {
  background-color: #2980b9; /* Цвет кнопки при наведении */
}
</style>