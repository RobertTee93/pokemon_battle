<template lang="html">
  <div>
    <div v-if="!winner" id="game-container">
      <img id="pokemon-one" :src="pokemonOne.sprites.back_default" alt="">
      <img id="pokemon-two" :src="pokemonTwo.sprites.front_default" alt="">
      <div id="pokemon-one-health">
        <div :style="{width: pokemonOneHealth + '%' }"></div>
      </div>
      <div id="pokemon-two-health">
        <div :style="{width: pokemonTwoHealth + '%' }"></div>
      </div>
    </div>
      <button v-if="!winner" v-on:click="handleAttack">{{ currentPlayer.name }} Attack</button>
      <winner v-if="winner" :pokemon="winner"></winner>
  </div>
</template>

<script>
import Winner from "./winner.vue"
export default {
  name: "pokeBattle",
  props: ["pokemonOne", "pokemonTwo"],
  data(){
    return{
      pokemonOneHealth: 0,
      pokemonTwoHealth: 0,
      currentPlayer: null,
      winner: null
    }
  },
  components: {
    Winner
  },
  methods: {
    getPokemonHealth(){
      this.pokemonOneHealth = 100
      this.pokemonTwoHealth = 100
    },
    getFirstPlayer(){
      if (this.pokemonOne.stats[0].base_stat >
          this.pokemonTwo.stats[0].base_stat){
            this.currentPlayer = this.pokemonOne
          } else {
            this.currentPlayer = this.pokemonTwo
      }
    },
    handleAttack(){
      if (this.currentPlayer.name === this.pokemonOne.name){
        this.pokemonTwoHealth -= this.randomAttackNumber()
        this.checkifKo()
        if (this.winner === null) {
          this.currentPlayer = this.pokemonTwo
        }
      } else if (this.currentPlayer.name === this.pokemonTwo.name){
        this.pokemonOneHealth -= this.randomAttackNumber()
        this.checkifKo()
        if (this.winner === null) {
          this.currentPlayer = this.pokemonOne
        }
      }
    },
    randomAttackNumber(){
      return Math.floor(Math.random() * this.currentPlayer.base_experience / 10)
    },
    checkifKo(){
      if (this.pokemonOneHealth < 0 || this.pokemonTwoHealth < 0 ){
        this.winner = this.currentPlayer
      }
    }
  },
  created(){
    this.getPokemonHealth()
    this.getFirstPlayer()
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

#pokemon-one-health {
  width: 100px;
  border: 2px solid #000;
  border-radius: 5px;
  position: absolute;
  top: 150px;
  left: 5px;
}

#pokemon-two-health {
  width: 100px;
  border: 2px solid #000;
  border-radius: 5px;
}

#pokemon-one-health div {
  height: 8px;
  border-radius: 5px;
  background: green;
}
#pokemon-two-health div {
  height: 8px;
  border-radius: 5px;
  background: green;
}

</style>
