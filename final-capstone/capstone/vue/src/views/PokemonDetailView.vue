<template>
  <div>
    <h1>{{ pokemon.name }}</h1>

    <img :src="pokemon.sprites.front_default" alt="Front Image">

    <section>
      <p>Height: {{ pokemon.height }}</p>
      <p>Weight: {{ pokemon.weight }}</p>
    </section>
  </div>
</template>

<script>
import pokeApiService from '../services/PokeApiService.js';

export default {
  created() {
    pokeApiService.getDetailsByIdOrName(this.pokemonId)
      .then(response => {
        this.pokemon = response.data;
      });
  },

  computed: {
    pokemonId() {
      return this.$route.params.id;
    }
  },

  data() {
    return {
      pokemon: {}
    }
  }
}
</script>