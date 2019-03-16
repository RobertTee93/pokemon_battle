<template lang="html">
  <div>
    <h2>{{ pokemon.name }}</h2>
    <img :src="pokemon.sprites.front_default" alt="">
    <div class="poke-info">
      <h3>Types:</h3>
      <p v-for="types in pokemon.types">{{ types.type.name }}</p>
      <hr>
      <p>Height: {{ pokemon.height }}0cm</p>
      <p>Weight: {{ pokemon.weight }}kg</p>
    </div>



  </div>
</template>

<script>
export default {
  name: "pokemonDetail",
  props: ["pokemon"],
  data(){
    return {

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

div {
  text-align: center;
}


</style>
