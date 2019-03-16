<template lang="html">
  <div>
    <div id="game-container">
      <img id="pokemon-one" :src="pokemonOne.sprites.back_default" alt="">
      <img id="pokemon-two" :src="pokemonTwo.sprites.front_default" alt="">
      <p>{{ pokemonOneHealth }}</p>
      <p>{{ pokemonTwoHealth }}</p>
    </div>
  </div>
</template>

<script>
export default {
  name: "pokeBattle",
  props: ["pokemonOne", "pokemonTwo"],
  data(){
    return{
      pokemonOneHealth: 0,
      pokemonTwoHealth: 0
    }
  },
  methods: {
    getPokemonHealth(){
      this.pokemonOneHealth = this.pokemonOne.base_experience
      this.pokemonTwoHealth = this.pokemonTwo.base_experience
    }
  },
  created(){
    this.getPokemonHealth()
  }
}
</script>

<style lang="css" scoped>

#game-container {
  position: relative;
  background-image: url("http://www.pokemontopaz.net/uploads/misc/battlegrass.png");
  height: 224px;
  width: 480px;
  overflow: hidden;
}

#pokemon-one {
  position: absolute;
  top: 150px;
  left: 20px;
}
#pokemon-two {
  position: absolute;
  left: 350px;
  top: 20px;
}

</style>
