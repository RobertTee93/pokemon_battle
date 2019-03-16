<template lang="html">
  <div>
    <div v-if="!winner" id="game-container">
      <button id="a-btn" v-if="!winner" v-on:click="handleAttack">A</button>
      <img id="pokemon-one" :src="pokemonOne.sprites.back_default" alt="">
      <img id="pokemon-two" :src="pokemonTwo.sprites.front_default" alt="">
      <div id="pokemon-one-health">
        <div :style="{width: pokemonOneHealth + '%' }"></div>
      </div>
      <div id="pokemon-two-health">
        <div :style="{width: pokemonTwoHealth + '%' }"></div>
      </div>
      <div class="info-box">
        <p v-if="damage && previousPlayer">{{ previousPlayer.name }} has dealt {{ damage }} damage!</p>
      </div>
      <div class="info-box">
        <p v-if="!damage && !previousPlayer">{{ currentPlayer.name }} To make the first move</p>
      </div>
    </div>
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
      previousPlayer: null,
      damage: null,
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
        const power = this.randomAttackNumber()
        this.pokemonTwoHealth -= power
        this.damage = power
        this.checkifKo()
        if (this.winner === null) {
          this.currentPlayer = this.pokemonTwo
          this.previousPlayer = this.pokemonOne
        }
      } else if (this.currentPlayer.name === this.pokemonTwo.name){
        const power = this.randomAttackNumber()
        this.pokemonOneHealth -= power
        this.damage = power
        this.checkifKo()
        if (this.winner === null) {
          this.currentPlayer = this.pokemonOne
          this.previousPlayer = this.pokemonTwo
        }
      }
    },
    randomAttackNumber(){
      return Math.floor(Math.random() * this.currentPlayer.base_experience / 10 + 1)
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
  margin: auto;
  margin-top: 100px;
  box-shadow: inset 0 0 20px black;
  border: 3px black solid;
  border-radius: 20px;
}

#a-btn {
  width: 70px;
  height: 70px;
  border-radius: 100px;
  position: absolute;
  top: 10px;
  left: 20px;
  box-shadow: inset 0 0 8px black;
  background-color: #00ff16;
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
  left: 18px;
}

#pokemon-two-health {
  width: 100px;
  border: 2px solid #000;
  border-radius: 5px;
  position: absolute;
  top: 10px;
  right: 25px;
}

#pokemon-one-health div {
  height: 5px;
  border-radius: 5px;
  background: green;
}
#pokemon-two-health div {
  height: 5px;
  border-radius: 5px;
  background: green;
}

.info-box {
  border: black 2px solid;
  position: absolute;
  bottom: 5px;
  right: 10px;
  height: 70px;
  width: 300px;
  border-radius: 10px;
  background-color: #366eec47;
  text-align: center;
}

</style>
