<template>
  <span class="cell" :style="{ backgroundColor: getTileColor }">
    {{ tile > 0 ? tile : '' }}
  </span>
</template>

<script>
export default {
  props: {
    tile: {
      type: Number,
      required: true,
    },
  },
  computed: {
    getTileColor() {
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
      return colors[this.tile] || '#3c3a32'; // Цвет по умолчанию
    },
  },
};
</script>

<style>
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
</style>
