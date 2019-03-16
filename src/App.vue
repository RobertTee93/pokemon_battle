<template lang="html">
  <div class="">
    <div v-if="!battleStarted" id="pokemon-container">
      <pokemon-one :pokemonOne="selectedPokemonOne" :allPokemon="allPokemon"></pokemon-one>
      <pokemon-two :pokemonTwo="selectedPokemonTwo" :allPokemon="allPokemon"></pokemon-two>
    </div>
    <button id="start-btn" v-if="!battleStarted && selectedPokemonOne && selectedPokemonTwo" v-on:click="startBattle">Start</button>
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

    eventBus.$on("reset-game", () => {
      this.battleStarted = false
      this.selectedPokemonOne = null
      this.selectedPokemonTwo = null
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

<style media="screen">
  * {
    font-family: 'Sniglet', cursive;
  }
</style>

<style lang="css" scoped>

div {
  text-align: center;
}

#start-btn {
  height: 50px;
  width: 50px;
  border-radius: 30px;
  background-color: #ff0909;
  box-shadow: inset 0 0 7px black;
}

#pokemon-container {
  display: flex;
  flex-wrap: wrap;
  justify-content: space-around;
}

</style>
