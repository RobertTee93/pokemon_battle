<template lang="html">
  <div id="search-box" >
    <h1>Choose Your Pokemon</h1>
    <img src="https://static.skillshare.com/uploads/parentClasses/c1f31e7b1049b383197c23589d6c3ede/77881667" alt="Img not available">
    <Dropdown v-if="allPokemon"
    id="searchBox"
    :options="allPokemon"
    :disabled="false"
    v-on:selected="selectedPokemon"
    placeholder="Search for pokemon here">
  </Dropdown>
</div>
</template>

<script>
import { eventBus } from "../main.js"
import Dropdown from "vue-simple-search-dropdown"
export default {
  name: "pokemonList",
  data(){
    return {
      currentPokemon: null,
    }
  },
  props: ["allPokemon"],
  methods: {
    getPokemonInfo(){
      fetch(this.currentPokemon)
      .then(result => result.json())
      .then((result) => {
        eventBus.$emit("pokemon-selected", result)
      })
    },
    getDropdownValues(){
      return this.filteredOptions
    },
    selectedPokemon(selected){
      if (selected.name){
        this.currentPokemon = selected.url
        this.getPokemonInfo()
      }

    }
  },
  components: {
    Dropdown
  }
}
</script>

<style lang="css" scoped>

img {
  width: 50px;
}

div {
  margin: auto;
  margin-top: 40px;
  width: 250px;
}

</style>
