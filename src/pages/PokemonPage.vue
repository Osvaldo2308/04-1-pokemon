<template>
    <h1 v-if="!pokemon">Espere por favor....</h1>
<div v-else>   
    <h1>¿Quién es este Pokemon?</h1>
    <!-- TODO: img -->
    <PokemonImg :pokemon-id="pokemon.id " :showPokemon="showPokemon"/>
    <!-- TODO: Opciones -->
    <PokemonOp :pokemons="pokemonArr"
    @selection="checkAnswer"
    />
    <template v-if="showAnswer">
        <h2  class="fade-in">{{ message }}</h2>
        <button @click="newGame">
            New Game
        </button>
    </template>
</div>

</template>

<script>
import PokemonImg from '@/components/PokemonImg.vue';
import PokemonOp from '@/components/PokemonOp.vue';

import getPokemonOptions from '@/helpers/getPokemonOptions'
import { Template } from 'webpack';

//console.log(getPokemonOptions() )

export default {
    components:{
    PokemonImg,
    PokemonOp,
    Template
},
    data(){
        return{
            pokemonArr: [],
            pokemon: null,
            showPokemon: false,
            showAnswer: false,
            message: ''
        }
    },
    methods:{
        async mixPokemonArray(){
            this.pokemonArr = await getPokemonOptions()

            const rndInt = Math.floor(Math.random()*4)
            
            this.pokemon = this.pokemonArr[rndInt]
        },
        checkAnswer(selectedId){
            this.showPokemon = true
            this.showAnswer = true

            if(selectedId === this.pokemon.id){
                this.message = `Correcto, ${this.pokemon.name}`
            }else{
                this.message = `Oops, era ${this.pokemon.name}`
            }
        },
        newGame(){
            this.showPokemon = false
            this.showAnswer = false
            this.pokemonArr = []
            this.pokemon = null
            this.mixPokemonArray ()
        }
    },
    mounted(){
        this.mixPokemonArray()
    }
}
</script>

<style>

</style>