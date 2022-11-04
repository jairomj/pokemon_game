<template>
  <h1 v-if="!pokemon">Loading Game</h1>
  <div v-else>
    <h1>Who is this Pokemon?</h1>
    <PokemonPicture :pokemonId="pokemon.id" :showPokemon="showPokemon" />
    <PokemonOption  
            :pokemons = "pokemonArr"
            @selectionPokemon="checkAnswer"/>

            <template class="center" v-if="showAnswer">
              <h2 class="fade-in">{{ message }}</h2>
              <button @click="playAgain">New Game</button>
            </template>
  </div>
</template>

<script>

import PokemonPicture from "@/components/PokemonPicture.vue";
import PokemonOption from "@/components/PokemonOption.vue";
import getPokemonOptions from "@/helpers/getPokemonOptions"



export default {
    components: { PokemonPicture, PokemonOption  },
    data(){
      return {
        pokemonArr: [],
        pokemon: null,
        showPokemon: false,
        showAnswer: false,
        message: ''

      }
    },
    methods: {
     async mixPokemonArray(){
        this.pokemonArr = await getPokemonOptions()
        const rndInt = Math.floor( Math.random() * 4)
        this.pokemon = this.pokemonArr[rndInt]
      },
      checkAnswer(pokemonId){
        this.showAnswer = true
        if(pokemonId === this.pokemon.id){
          this.showPokemon = true;
          this.message = `You Win, ${this.pokemon.name}, is the name`
        }else {
          this.message = `you lost, try again`
        }
        
      },
      playAgain(){
        location.reload()
      }
    },
    mounted(){
      this.showPokemon = false
      this.showAnswer = false
      this.pokemonArr = []
      this.pokemon = null
      this.mixPokemonArray()
    }
}
</script>

<style>

.center {
    display: flex;
    justify-content: center;
}
</style>