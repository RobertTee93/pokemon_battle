<template lang="html">
  <div class="winner">
    <h2>{{ pokemon.name }} Wins!</h2>
    <img :src="pokemon.sprites.front_default" alt="">
    <img :src="pokemon.sprites.back_default" alt="">
    <button v-on:click="resetGame">Reset Game</button>
  </div>
</template>

<script>
import { eventBus } from "../main.js"
export default {
  name: "winner",
  props: ["pokemon"],
  methods: {
    resetGame(){
      eventBus.$emit("reset-game")
    }
  }
}
</script>

<style lang="css" scoped>

h2 {
  color: #dd0021;
  font-weight: 500;
  font-size: 30px;
  text-shadow: 0 0 1px black;
}

button {
  display: block;
  margin: auto;
}

</style>
