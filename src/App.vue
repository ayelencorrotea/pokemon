<script setup lang="ts">
import PokemonList from "./components/PokemonList.vue";
import 'vuetify/styles'
import MyPokemons from "./components/MyPokemons.vue";
</script>

<template>
  
    <v-layout class="rounded rounded-md">
      <v-app-bar color="surface-variant" title="POKEDEX">
      <MyPokemons/>
      </v-app-bar>
      
      <v-main class="d-flex align-center justify-center" style="min-height: 300px;" >
        <PokemonList msg="Vite + Vue" />
      </v-main>
    </v-layout>
  
</template>

<style scoped>
.logo {
  height: 6em;
  padding: 1.5em;
  will-change: filter;
  transition: filter 300ms;
}
.logo:hover {
  filter: drop-shadow(0 0 2em #646cffaa);
}
.logo.vue:hover {
  filter: drop-shadow(0 0 2em #42b883aa);
}
</style>
