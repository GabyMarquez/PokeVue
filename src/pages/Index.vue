<template>
  <q-page class="flex column">
     <template v-if="pokemon">
      <div class="fixed-center">
        <div class="col text-white text-center">
          <img class="center" :src="pokemon.sprites.front_default">
          <div class="div text-h4 text-weight-bold">{{pokemonNameToSearch}}</div>
          <div class="div text-h6 text-weight-bold">We choose you!</div>
          <div class="col text-white text-center">
            <div class="q-pa-md q-gutter-sm">
              <q-btn @click="getCountPokemon" class="col" outline>Get a Pokemon</q-btn>
              <q-btn @click="exit" class="col" outline>Exit</q-btn>
            </div>
          </div>
        </div>
      </div>
    </template>
   <template v-else>
     <div class="fixed-center">
       <div class="col text-white text-center">
        <div class="div text-h4 text-weight-bold">Pokemon APP with Quasar</div>
        <div class="q-pa-md q-gutter-sm">
            <q-btn @click="getCountPokemon" class="col" outline>Get a Pokemon</q-btn>
        </div>
      </div>
     </div>
   </template>
  </q-page>
</template>

<script>
import { defineComponent } from 'vue';

export default defineComponent({
  name: 'PageIndex',
  data() {
    return {
      pokemon: null,
      apiUrl: 'https://pokeapi.co/api/v2/',
      responseData: null,
      countOfPokemons: null,
      min: 0,
      arrayPos: null,
      pokemonNameToSearch: null
    }
  },
  methods: {
    getPokemon() {
      console.log("You will get a Pokemon")
      this.getAPokemon()
    },
    getCountPokemon() {
      this.$axios(`${ this.apiUrl }type/10`).then(response => {
        this.responseData = response.data.pokemon
        console.log('response : ', this.responseData)
        this.countOfPokemons = Object.keys(this.responseData).length;        
        console.log('size : ', this.countOfPokemons)
        this.getRandomNumber()
      })
    },
    getRandomNumber() {
      this.arrayPos = this.generateNumber()
      console.log('arrayPos : ', this.arrayPos)
      this.getPokemonNameToSearch()
    },
    generateNumber() {
      return Math.floor(Math.random()*(this.countOfPokemons-this.min+1)+this.min);
    },
    getPokemonNameToSearch() {
      this.pokemonNameToSearch = this.responseData[this.arrayPos].pokemon.name
      console.log('pokemonNameToSearch : ', this.pokemonNameToSearch)
      this.getPokemonData( )
    },
    getPokemonData() {
      this.$axios(`${ this.apiUrl }pokemon/${this.pokemonNameToSearch}`).then(response => {
        this.pokemon = response.data
        console.log('responsePokemon : ', this.pokemon)
      })
    },
    exit() {
      this.pokemon = null
    }
  }
})
</script>

<style lang="sass">
  .q-page
    background: linear-gradient(to top, #f12711, #f5af19)
</style>
