<template lang="html">
  <div id="search-box" >
    <h2>Choose 2nd Pokemon</h2>
    <img src="https://static.skillshare.com/uploads/parentClasses/c1f31e7b1049b383197c23589d6c3ede/77881667" alt="Img not available">
    <Dropdown v-if="allPokemon"
    id="searchBox"
    :options="allPokemon"
    :disabled="false"
    v-on:selected="selectedPokemonTwo"
    placeholder="Search for pokemon here">
  </Dropdown>
  <pokemon-detail v-if="pokemonTwo" :pokemon="pokemonTwo"></pokemon-detail>
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
        eventBus.$emit("pokemon-two-selected", this.foundPokemon)
      })
    },
    getDropdownValues(){
      return this.filteredOptions
    },
    selectedPokemonTwo(selected){
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
  width: 70px;
  animation: wiggle 2s ease-out infinite;
}

@keyframes wiggle {
  20% {
    box-shadow:  inset -5px 0 5px 0 rgba(0,0,0,.4);
    transform: rotate(7deg);
  }

  40% {
    box-shadow:  inset -11px 0 5px 0 rgba(0,0,0,.4);
    transform: rotate(-14deg);
  }

  60% {
    box-shadow:  inset -5px 0 5px 0 rgba(0,0,0,.4);
    transform: rotate(4deg);
  }

  80% {
    box-shadow:  inset -8px 0 5px 0 rgba(0,0,0,.4);
    transform: rotate(-2deg);
  }

  100% {
    box-shadow:  inset -7px 0 5px 0 rgba(0,0,0,.4);
    transform: rotate(0deg);
  }
}

</style>
