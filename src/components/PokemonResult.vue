<template>
    <div id="pokemon">
        <div class="card">
        <div class="card-image">
            <figure>
            <img
                :src="currentImg" alt="Placeholder image"/>
            </figure>
        </div>
        <div class="card-content">
            <div class="media">
            <div class="media-content">
                <p class="title is-4">{{num}} - {{name | upper}}</p>
                <p class="subtitle is-6">{{pokemon.type}}</p>
            </div>
            </div>

            <div class="content">
                <button class="button is-medium is-fullwidth" @click="mudarSprite">Mudar Sprite</button>
            </div>
        </div>
        </div>

    </div>
</template>

<script>

import axios from 'axios';

export default {
    //Metodo create que é o metodo para criar dados na requisição na api do Pokedex
    created: function(){
        axios.get(this.url).then(response => {
            //Criando uma variavel dentro do nosso objeto pokemon que vai receber os dados 
            //Criando variaveis dicamente para o objeto pokemon
            this.pokemon.type = response.data.types[0].type.name;
            //Pegando os sprietes dos pokemons
            this.pokemon.front = response.data.sprites.front_default;
            this.pokemon.back  = response.data.sprites.back_default;
            //Confirmando a imagem atual do pokemon
            this.currentImg = this.pokemon.front;
            console.log(this.pokemon);
        })
    },
    //Metodo data para retornar objeto
    data(){
        return {
            isFront: true,
            currentImg: '',
            pokemon: {
                type: '',
                front: '',
                back: ''
            }
        }
    },
    props:{
        num: Number,
        name: String,
        url: String
    },
    filters:{
        upper: function(value){
            var newName = value[0].toUpperCase() + value.slice(1);
            return newName;
        }
    },
    //Criando um objeto methods onde podemos criar a nossa função responsavel por alternar os sprites
    methods:{
        mudarSprite: function(){
            if(this.isFront){
                this.isFront = false;
                this.currentImg = this.pokemon.back;
            }else{
                this.isFront = true;
                this.currentImg = this.pokemon.front;
            }
        }
    }
}
</script>

<style>
    #pokemon{

        margin-top: 2%;
    }
</style>