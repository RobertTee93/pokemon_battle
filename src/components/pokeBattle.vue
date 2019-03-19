<template lang="html">
  <div id="battle-container">
    <div v-if="!winner" id="game-container">
      <button id="a-btn" v-if="!winner" v-on:click="handleAttack">Attack</button>
      <img v-if="pokemonOneAttack" id="pokemon-one-attack" :src="pokemonOne.sprites.back_default" alt="">
      <img v-if="!pokemonOneAttack" id="pokemon-one" :src="pokemonOne.sprites.back_default" alt="">
      <img v-if="pokemonTwoAttack" id="pokemon-two-attack" :src="pokemonTwo.sprites.front_default" alt="">
      <img v-if="!pokemonTwoAttack" id="pokemon-two" :src="pokemonTwo.sprites.front_default" alt="">
      <div id="pokemon-one-health">
        <div :style="{width: pokemonOneHealth / 5 + '%' }"></div>
      </div>
      <div id="pokemon-two-health">
        <div :style="{width: pokemonTwoHealth / 5 + '%' }"></div>
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
      pokemonOneAttack: false,
      pokemonTwoHealth: 0,
      pokemonTwoAttack: false,
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
      this.pokemonOneHealth = 500
      this.pokemonTwoHealth = 500
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
        this.pokemonTwoAttack = false
        this.pokemonOneAttack = true
        this.checkifKo()
        if (this.winner === null) {
          this.currentPlayer = this.pokemonTwo
          this.previousPlayer = this.pokemonOne
        }
      } else if (this.currentPlayer.name === this.pokemonTwo.name){
        const power = this.randomAttackNumber()
        this.pokemonOneHealth -= power
        this.damage = power
        this.pokemonTwoAttack = true
        this.pokemonOneAttack = false
        this.checkifKo()
        if (this.winner === null) {
          this.currentPlayer = this.pokemonOne
          this.previousPlayer = this.pokemonTwo
        }
      }
    },
    randomAttackNumber(){
      return Math.floor(Math.random() * this.currentPlayer.stats[4].base_stat + 1)
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

#battle-container {
  width: 480px;
  margin: auto;
  height: 300px;
}

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
  animation: pokeOneMove 5s ease-out infinite;
}

#pokemon-one-attack {
  position: absolute;
  top: 150px;
  left: 20px;
  animation: pokeOneAttack 0.5s ease-out 1;
}

#pokemon-two {
  position: absolute;
  left: 350px;
  top: 20px;
  animation: pokeTwoMove 1s ease-out infinite;
}

#pokemon-two-attack {
  position: absolute;
  left: 350px;
  top: 20px;
  animation: pokeTwoAttack 0.5s ease-out 1;
}

#pokemon-one-health {
  width: 100px;
  border: 2px solid #000;
  border-radius: 5px;
  position: absolute;
  top: 140px;
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

@keyframes pokeOneMove {
  20% {
    position: absolute;
    top: 147px;
    left: 18px;
  }
  40% {
    position: absolute;
    top: 145px;
    left: 16px;
  }
  60% {
    position: absolute;
    top: 147px;
    left: 16px;
  }
  80% {
    position: absolute;
    top: 149px;
    left: 18px;
  }
  100% {
    position: absolute;
    top: 150px;
    left: 20px;
  }
}

@keyframes pokeTwoMove {
  20% {
    position: absolute;
    left: 352px;
    top: 22px;
  }
  40% {
    position: absolute;
    left: 354px;
    top: 24px;
  }
  60% {
    position: absolute;
    top: 24px;
    left: 355px;
  }
  80% {
    position: absolute;
    top: 22px;
    left: 353px;
  }
  100% {
    position: absolute;
    left: 350px;
    top: 20px;
  }
}

@keyframes pokeOneAttack {
  0% {
    position: absolute;
    top: 150px;
    left: 20px;
  }
  50% {
    position: absolute;
    top: 70px;
    left: 200px;
  }
  100% {
    position: absolute;
    top: 150px;
    left: 20px;
  }
}

@keyframes pokeTwoAttack {
  0% {
    position: absolute;
    left: 350px;
    top: 20px;
  }
  50% {
    position: absolute;
    left: 200px;
    top: 70px;
  }
  100% {
    position: absolute;
    left: 350px;
    top: 20px;
  }
}


</style>
