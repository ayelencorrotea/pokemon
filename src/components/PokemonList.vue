<script lang="ts" setup>
import axios from 'axios';
import { PokemonResponse, FullPokemon, Move } from '../interfaces/pokemonApi';
import { ref, onMounted } from 'vue';

const pokemonList = ref<FullPokemon[]>([])
const dialog = ref(false);
const dialogMoves = ref<Move[]>([])

//componente se monta  
onMounted(async() => {
   try {
    const response = await axios.get<PokemonResponse>("https://pokeapi.co/api/v2/pokemon?limit=150&offset=0");
     const pokemons = response.data.results
     for (let index = 0; index < pokemons.length; index++){
       const response = await axios.get<FullPokemon>(pokemons[index].url);
       pokemonList.value.push(response.data)
     }
  } catch (error) {
    console.log(error)
  } 
  })
const catchPokemon = ((pokemon: FullPokemon) => {
    
  const storagePokemon = localStorage.getItem('pokemons') || ""
  console.log(storagePokemon)
  const pokemonsListFromStorage = storagePokemon != "" ? JSON.parse(storagePokemon) : []

  if (pokemonsListFromStorage.length == 4) {
    alert("ya paso el maximo de almacenamiento")
   return
  }
  pokemonsListFromStorage.push(pokemon)
  localStorage.setItem("pokemons", JSON.stringify(pokemonsListFromStorage))
  dialog.value = true
  dialogMoves.value = pokemon.moves.slice(0, 2)
    console.log(pokemon.moves)
})
</script>

<template>
 <div class="pokemon-list">
      <ul>
        <li v-for="(pokemon) in pokemonList" :key="pokemon.id">
    <v-card class="mx-auto my-4" width="700" variant="outlined">
        <v-card-item>
          {{ pokemon.name }}
          {{ pokemon.id }}
          
          <div class="text-overline mb-1" v-for="(abilitie, index) in pokemon.abilities" :key="index">
            {{ abilitie.ability.name }}
          </div>
          
          <img :src="`https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/${pokemon.id}.png`" :alt="pokemon.name">
         <v-card-actions>
          <v-btn @click="catchPokemon(pokemon)" variant="outlined" color = "red">
            Capturar
          </v-btn>
        </v-card-actions>
        </v-card-item>
    </v-card>
        </li>
      </ul>
       <v-dialog
            v-model="dialog"
            width="auto"
          >
            <v-card>
              <div v-for="(move, index) in dialogMoves" :key="index">
              <v-card-text>
                {{ move.move.name }}
              </v-card-text>
              </div>
              <v-card-actions>
                <v-btn color="primary" block @click="dialog = false">Close Dialog</v-btn>
              </v-card-actions>
            </v-card>
          </v-dialog>
    </div>
</template>

<style scoped>
.read-the-docs {
  color: #888;
}
</style>
