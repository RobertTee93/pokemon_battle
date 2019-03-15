<template lang="html">
  <div class="">
    <!-- <poke-nav></poke-nav> -->
    <pokemon-list :allPokemon="allPokemon"></pokemon-list>
    <pokemon-detail v-if="selectedPokemon" :pokemon="selectedPokemon"></pokemon-detail>
  </div>
</template>

<script>
import PokemonList from "./components/pokemonList.vue"
import PokemonDetail from "./components/pokemonDetail.vue"
import PokeNav from "./components/pokeNav.vue"
import { eventBus } from "./main.js"
export default {
  name: "App",
  data(){
    return {
      allPokemon: null,
      selectedPokemon: null
    }
  },
  methods: {
    fetchPokemon(){
    return fetch("https://pokeapi.co/api/v2/pokemon?offset=0&limit=807")
      .then(result => result.json())
      .then(result => this.allPokemon = result["results"])
    }
  },
  mounted(){
    this.fetchPokemon();

    eventBus.$on("pokemon-selected", (pokemon) => {
      this.selectedPokemon = pokemon
    })
  },
  components: {
    PokemonList,
    PokemonDetail,
    PokeNav
  }
}
</script>

<style lang="css" scoped>

div {
  text-align: center;
}

div input {
  text-align: center;
}

</style>
