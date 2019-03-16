<template lang="html">
  <div class="">
    <div v-if="!battleStarted" id="pokemon-container">
      <pokemon-one :allPokemon="allPokemon"></pokemon-one>
      <pokemon-detail v-if="selectedPokemonOne" :pokemon="selectedPokemonOne"></pokemon-detail>
      <pokemon-two :allPokemon="allPokemon"></pokemon-two>
      <pokemon-detail v-if="selectedPokemonTwo" :pokemon="selectedPokemonTwo"></pokemon-detail>
    </div>
    <button v-on:click="startBattle">Start Battle</button>
    <poke-battle v-if="battleStarted" :pokemonOne="selectedPokemonOne" :pokemonTwo="selectedPokemonTwo"></poke-battle>
  </div>
</template>

<script>
import PokemonOne from "./components/pokemonOne.vue"
import PokemonTwo from "./components/pokemonTwo.vue"
import PokeBattle from "./components/pokeBattle"
import PokemonDetail from "./components/pokemonDetail.vue"
import { eventBus } from "./main.js"
export default {
  name: "App",
  data(){
    return {
      allPokemon: null,
      selectedPokemonOne: null,
      selectedPokemonTwo: null,
      battleStarted: false
    }
  },
  methods: {
    fetchPokemon(){
    return fetch("https://pokeapi.co/api/v2/pokemon?offset=0&limit=807")
      .then(result => result.json())
      .then(result => this.allPokemon = result["results"])
    },
    startBattle(){
      this.battleStarted = true
    }
  },
  mounted(){
    this.fetchPokemon();

    eventBus.$on("pokemon-one-selected", (pokemon) => {
      this.selectedPokemonOne = pokemon
    })

    eventBus.$on("pokemon-two-selected", (pokemon) => {
      this.selectedPokemonTwo = pokemon
    })
  },
  components: {
    PokemonOne,
    PokemonTwo,
    PokeBattle,
    PokemonDetail
  }
}
</script>

<style lang="css" scoped>

#pokemon-container {
  display: flex;
  flex-wrap: wrap;
  justify-content: space-around;
}

</style>
