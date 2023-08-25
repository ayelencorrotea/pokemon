<script lang="ts" setup>
import { FullPokemon } from '../interfaces/pokemonApi';
import { ref } from 'vue';

const pokemonList = ref<FullPokemon[]>([])
const dialog = ref(false); 

const getPokemons = (() => {

    const storagePokemon = localStorage.getItem('pokemons') || ""
    const pokemonsListFromStorage = storagePokemon != "" ? JSON.parse(storagePokemon) : []

    if (pokemonsListFromStorage.length == 0) {
        alert("no tienes pokemones guardados")
        return
    }
    pokemonList.value = pokemonsListFromStorage;
    console.log(pokemonList.value)
    dialog.value = true
})

</script>

<template>
    <div>
         <v-btn @click="getPokemons()" >
            <v-icon>mdi-heart</v-icon>
             Mis pokemones
        </v-btn>
        <v-dialog v-model="dialog" width="600px">
            <v-card>
                Mis Pokemones
                <ul>
            <li v-for="(pokemon) in pokemonList" :key="pokemon.id">
        <v-card class="ma-4" max-width="700" variant="outlined">
            <v-card-item>
              {{ pokemon.name }}
              {{ pokemon.id }}
          
              <div class="text-overline mb-1" v-for="(abilitie, index) in pokemon.abilities" :key="index">
                {{ abilitie.ability.name }}
              </div>
          
              <img :src="`https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/${pokemon.id}.png`" :alt="pokemon.name">
             <v-card-actions>
            </v-card-actions>
            </v-card-item>
        </v-card>
            </li>
          </ul>  
            </v-card>
        </v-dialog>
    </div>
</template>

<style scoped>
.read-the-docs {
    color: #401e1e;
}
</style>
