<template lang="html">
  <div class="winner">
    <h1>{{ pokemon.name }} Wins!</h1>
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
</style>
