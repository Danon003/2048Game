<template>
  <div class="tile" :style="{ backgroundColor: getTileColor }">
    {{ tile.value > 0 ? tile.value : '' }}
  </div>
</template>

<script>
export default {
  props: {
    tile: {
      type: Object,
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
      return colors[this.tile.value] || '#3c3a32'; // Цвет по умолчанию
    },
  },
};
</script>

<style>
.tile {
  width: 90px; /* ширина плитки */
  height: 90px; /* высота плитки */
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
