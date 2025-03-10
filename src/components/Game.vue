<template>
  <div class="game">
    <div class="header">
      <div style="display: flex;
              justify-content: space-between;
              margin: 0 auto;
              max-width: 600px;
              align-items: center;
              gap: 80px;">
        <h1 class="game-title">2048</h1>
        <div class="score-box">
          <span class="score-label">Счет:</span>
          <span class="score-value">{{ score }}</span>
        </div>
        <button class="new-game-btn" @click="init">Новая игра</button>
      </div>
    </div>
    <div class="board">
      <div
          v-for="(row, rowIndex) in rows"
          :key="rowIndex"
          class="row"
      >
        <tile
            v-for="(cell, colIndex) in row"
            :key="colIndex"
            :tile="cell"
        />
      </div>
      <div v-if="gameOver" class="game-over">End Game</div>
      <div v-if="victory" class="victory">Victory</div>
    </div>
  </div>
</template>

<script>
import Tile from './Tile.vue';

export default {
  name: 'Game',
  components: {
    Tile,
  },
  data() {
    return {
      cells: [],
      score: 0,
      gameOver: false,
      victory: false,
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
      this.gameOver = false;
      this.victory = false;
      this.score = 0;
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
      }
      if (!this.checkForMoves()) {
        this.gameOver = true; // Игра окончена
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
            if (this.cells[prevIndex] === 2048) {
              this.victory = true; // Показать Victory
            }
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
            if (this.cells[nextIndex] === 2048) {
              this.victory = true; // Показать Victory
            }
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
            if (this.cells[prevIndex] === 2048) {
              this.victory = true; // Показать Victory
            }
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
            if (this.cells[nextIndex] === 2048) {
              this.victory = true; // Показать Victory
            }
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
    handleKeyDown(event) {
      // Предотвращает прокрутку страницы при нажатии стрелок
      if (event.key === 'ArrowUp' || event.key === 'ArrowDown') {
        event.preventDefault();
      }

      switch (event.key) {
        case 'ArrowUp':
          this.move('ArrowUp');
          break;
        case 'ArrowDown':
          this.move('ArrowDown');
          break;
        case 'ArrowLeft':
          this.move('ArrowLeft');
          break;
        case 'ArrowRight':
          this.move('ArrowRight');
          break;
      }
    },
  },
  created() {
    this.init(); // Инициализация игры
    document.addEventListener('keydown', this.handleKeyDown);
  },
  beforeDestroy() {
    document.removeEventListener('keydown', this.handleKeyDown);
  },
};
</script>

<style>
.game {
  display: flex;
  flex-direction: column;
  align-items: center;
  overflow: hidden; /* Убираем прокрутку */
  height: 100vh; /* Ограничиваем высоту */
}

.header {
  display: flex;
  justify-content: space-between;
  width: 100%; /* занимает всю ширину */
  margin-bottom: 20px;
}

.game-title {
  color: #bb8213;
  font-weight: bold;
  margin-left: 0; /* Отступ слева для названия игры */
}

.board {
  display: flex;
  flex-direction: column;
  background-color: #d0d6da;
  border-radius: 8px;
  margin: 20px 0;
  position: relative; /* Для позиционирования текста Game Over и Victory */
}

.row {
  display: flex;
}

.new-game-btn {
  width: 100px;
  height: 50px;
  background: indianred;
  border-radius: 5px;
  border: none;
  color: white;
  cursor: pointer;
  font-size: 16px;
  margin: 14px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.5); /* Лёгкая тень */
  transition: transform 0.1s ease, box-shadow 0.1s ease; /* Плавный переход */
}

.new-game-btn:active {
  transform: scale(0.95);
  box-shadow: 0 1px 2px rgba(0, 0, 0, 0.3);
}

.game-over, .victory {
  position: absolute;
  top: 50%; /* По центру по вертикали */
  left: 50%; /* По центру по горизонтали */
  transform: translate(-50%, -50%); /* Нормализуем позицию */
  font-size: 36px;
  font-weight: bold;
  color: red;
  text-align: center;
}
.victory {
  color: green;
}
.score-box {
  display: flex;
  flex-direction: column; /* Стекаем их вертикально */
  align-items: center; /* Выравниваем по центру */
  justify-content: center; /* Центрируем содержимое */
  width: 100px; /* Фиксированная ширина */
  height: 50px; /* Фиксированная высота */
  background-color: #f3bb4c; /* Цвет фона */
  border-radius: 7px;
  padding: 10px; /* Внутренние отступы */
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.5); /* Лёгкая тень */
}

.score-label {
  font-size: 17px;
  margin-bottom: 5px;
}

.score-value {
  font-size: 20px;
  font-weight: bold;
}
</style>
