<template lang="html">
  <div id="search-box" >
    <h2>Choose 1st Pokemon</h2>
    <img src="https://static.skillshare.com/uploads/parentClasses/c1f31e7b1049b383197c23589d6c3ede/77881667" alt="Img not available">
    <Dropdown v-if="allPokemon"
    id="searchBox"
    :options="allPokemon"
    :disabled="false"
    v-on:selected="selectedPokemonOne"
    placeholder="Search for pokemon here">
  </Dropdown>
  <pokemon-detail v-if="pokemonOne" :pokemon="pokemonOne"></pokemon-detail>
</div>
</template>

<script>
import PokemonDetail from "./pokemonDetail"
import { eventBus } from "../main.js"
import Dropdown from "vue-simple-search-dropdown"
export default {
  name: "pokemonList",
  data(){
    return {
      currentPokemon: null,
      foundPokemon: null
    }
  },
  props: ["allPokemon", "pokemonOne", "pokemonTwo"],
  methods: {
    getPokemonInfo(){
      fetch(this.currentPokemon)
      .then(result => result.json())
      .then((result) => {
        this.foundPokemon = result
        eventBus.$emit("pokemon-one-selected", this.foundPokemon)
      })
    },
    getDropdownValues(){
      return this.filteredOptions
    },
    selectedPokemonOne(selected){
      if (selected.name){
        this.currentPokemon = selected.url
        this.getPokemonInfo()
      }
    }
  },
  components: {
    Dropdown,
    PokemonDetail
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

img {
  width: 50px;
}

</style>
