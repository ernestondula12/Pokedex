<template>
  <div id="app">
  
      <div class="column is-half is-offset-one-quarter">
        <img src="./assets/guia.png" style="width: 200px" alt="">
        <h4 class="is-size-4" style="color: #fff">POKEDEX</h4>
        <input type="text" name="" id="" placeholder="Buscar pokemon pelo nome" class="input is-rounded" v-model="busca">
        <button class="button is-fullwidth is-success" id="buscaBtn" @click="buscar">Buscar</button>
         <div v-for="(poke, index) in filteredPokemons" :key="poke.url">
        <PokemonResult :name="poke.name" :url="poke.url" :num="index+1"/>
      </div>
  </div>
    
  </div>
</template>

<script>
//Importando o axios
import axios from 'axios';
//Importando o nosso componente Pokemon
import PokemonResult from './components/PokemonResult.vue';
export default {
  name: 'App',
  data(){
    return{
        pokemons: [],
        //Criando um novo array de pokemons filtrados
        filteredPokemons: [],
        //Criando uma variavel busca que será responsavel por receber todos os dados do pokemon para faciliat a busca
        busca: ''
    }
  },
  created: function(){
      axios.get("https://pokeapi.co/api/v2/pokemon?limit=151&offset=0").then(res => {

        console.log("Pegou a lista de pokemons");
        this.pokemons = res.data.results
        //Preenchendo o array de pokemons filtrados
        this.filteredPokemons = res.data.results;
      })  
  },
  components:{
    PokemonResult
  },
  methods:{
      buscar: function(){
          this.filteredPokemons = this.pokemons;
          if(this.busca == '' || this.busca == ' '){
            //Se busca for vazia ela retorna todos os pokemons
            this.filteredPokemons = this.pokemons;
          }else{
              this.filteredPokemons = this.pokemons.filter(pokemon => pokemon.name == this.busca);
          }
      }
  },
  computed: {
      /*
      resultadoBusca: function(){
          if(this.busca == '' || this.busca == ' '){
              return this.pokemons;
          }else{
              return this.pokemons.filter(pokemon => pokemon.name == this.busca)
          }
      }
      */
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
  margin-top: 60px;
}

#buscaBtn{
  margin-top: 2%;
  font-weight: bold;
}
</style>
