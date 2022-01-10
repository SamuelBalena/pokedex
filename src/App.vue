<template>
  <div id="app">
    <nav class="navbar is-primary turquoise" role="navigation" aria-label="main navigation">
      <div class="container">
        <div class="navbar-brand">
          <a class="navbar-item" href="#">
            <h4 class="is-size-4">PokéApi</h4>
          </a>

          <a role="button" class="navbar-burger" aria-label="menu" aria-expanded="false" data-target="navbarBasicExample">
            <span aria-hidden="true"></span>
            <span aria-hidden="true"></span>
            <span aria-hidden="true"></span>
          </a>
        </div>

          <div class="navbar-end">
            <div class="navbar-item">
              <input class="input" type="text" placeholder="Buscar pokemon" v-model="busca">
            </div>
            <div>
              <button class="button is-info" @click="buscar">Buscar</button>
            </div>
          </div>
      </div>
    </nav>
    <div class="column is-half is-offset-one-quarter">
      <div v-for="(poke,index) in filteredPokemons" v-bind:key="poke.url">
      <Pokemon :name="poke.name" :url="poke.url" :num="index+1"/>
    </div>
    </div>
  </div>
</template>

<script>
import axios from "axios"
import Pokemon from "../src/components/Pokemon.vue"

export default {
  name: 'App',
  data(){
    return {
      pokemons: [],
      filteredPokemons: [],
      busca: ""
    }
  },
  created: function(){
    axios.get("https://pokeapi.co/api/v2/pokemon?limit=151&offset=0").then(res =>{
      console.log("Pegou a lista de dados")
      this.pokemons = res.data.results;
      this.filteredPokemons = res.data.results;
    })
  },
  methods: {
    buscar: function() {
      this.filteredPokemons = this.pokemons;
      if(this.busca == "" || this.busca == " ") {
        this.filteredPokemons = this.pokemons;
      }else{
        this.filteredPokemons = this.pokemons.filter(pokemon => pokemon.name == this.busca)
      }
    }
  },
  computed: {
    /*resultadoBusca: function(){
      if(this.busca == "" || this.busca == " ") {
        return this.pokemons;
      }else{
        return this.pokemons.filter(pokemon => pokemon.name == this.busca)
      }
    }
    */
  },
  components:{
    Pokemon
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}

@media screen and (min-width: 1024px) {
  .navbar, .navbar-menu, .navbar-start, .navbar-end {
    align-items: center !important; 
    display: flex;
}
}

</style>