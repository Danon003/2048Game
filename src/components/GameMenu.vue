<template>
  <div class="game-menu">
    <h2 class="welcomeText">Welcome to 2048!</h2>
    <p class="welcomeText">Your score: {{ score }}</p>
    <button @click="$emit('newGame')" class="new-game-btn">New Game</button>
  </div>
</template>

<script>
export default {
  name: 'GameMenu',
  props: {
    score: {
      type: Number,
      default: 0,
    },
  },
};
</script>

<style>
.game-menu {
  text-align: center;
  margin-bottom: 20px;
}
.new-game-btn {
  margin-top: 10px;
  padding: 10px;
}
.welcomeText {
  color: #555;
}
</style>
