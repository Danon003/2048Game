<template>
  <div id="app">
    <game @new-game="newGame" />
  </div>
</template>

<script>
import Game from "@/components/Game.vue";

export default {
  components: {
    Game,
  },
};
</script>

<style>
#app {
  text-align: center;
  padding: 20px;
}
</style>
