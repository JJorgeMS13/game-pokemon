<template>
    <h1 v-if="!pokemon">Espere por favor...</h1>
    <div v-else>
     <h1>¿Quién es este  pokemón?</h1>
     <PokemonPicture :pokemonId="pokemon.id" :showPokemon="showPokemon"/>
     <PokemonOpcion :pokemons="pokemonArr" @selection-pokemon="checkAnswer"/>
     <template v-if="showAnswer">
        <h2 class="fade-in">{{ message }}</h2>
        <button @click="reset">Nuevo Juego</button>
     </template>
    </div>
</template>

<script>
import PokemonOpcion from '@/components/PokemonOption.vue';
import PokemonPicture from '@/components/PokemonPicture.vue';
import getPokemonOptions from "@/helpers/getPokemonOptions";


export default {
    name: 'PokemonPage',
    components: {
        PokemonOpcion,
        PokemonPicture
    },
    data(){
        return {
            pokemonArr: [],
            pokemon: null,
            showPokemon: false,
            showAnswer: false,
            message: '',
        }
    }, 
    methods: {
        async mixPokemonArray() {
            this.pokemonArr = await getPokemonOptions()
            const rndInt = Math.floor(Math.random() * 4)
            this.pokemon =  this.pokemonArr[rndInt]
        },
        checkAnswer(selectId){
            this.showPokemon =  true
            this.showAnswer = true
            if (selectId === this.pokemon.id) {
                this.message = `Correcto, ${this.pokemon.name}`
            }
            else {
                this.message = `Oops, era, ${this.pokemon.name}`
            }            
        },
        reset() {
            this.showPokemon = false
            this.showAnswer = false
            this.pokemonArr = []
            this.pokemon = null
            this.mixPokemonArray()
        }

    },
    mounted() {
       this.mixPokemonArray()
    }
}
</script>

<style lang="css" scoped>

</style>