<script>
import pokemonData from './assets/pokemonapi.json';

export default {
  data() {
    return {
      pokemonList: pokemonData,
      searchTerm: '',
      selectedPokemon: null
    }
  },
  computed: {
    filteredPokemonList() {
      return this.pokemonList.filter(pokemon => pokemon.name.includes(this.searchTerm));
    }
  },
  methods: {
    async showPokemon(url) {
      const response = await fetch(url);
      if (!response.ok) {
        console.error(`Error fetching Pokemon: ${response.statusText}`);
        return;
      }

      this.selectedPokemon = await response.json();
      console.log(this.selectedPokemon);
    }
  }
}
</script>

<template>
  <header>
    <img alt="Vue logo" class="logo" src="./assets/logo.webp" />
  </header>

  <main>
    <div class="search-container">
      <input class="search-box" type="text" placeholder="Search..." v-model="searchTerm">
    </div>

    <div class="pokemon-details" v-if="selectedPokemon">
      <h2>{{ selectedPokemon.name }}</h2>
      <img :src="selectedPokemon.sprites.front_default" alt="selectedPokemon.name">
      <p>Height: {{ selectedPokemon.height }}</p>
      <p>Weight: {{ selectedPokemon.weight }}</p>

      <div v-for="(stat, index) in selectedPokemon.stats" :key="index">
        <p>{{ stat.stat.name }}: {{ stat.base_stat }}</p>
      </div>
    </div>

    <ul>
      <li v-for="pokemon in filteredPokemonList" :key="pokemon.id" class="pokemon-item">
        <a href="#" @click="showPokemon(pokemon.url)">{{ pokemon.name }}</a>
      </li>
    </ul>
  </main>
</template>

<style scoped>
header {
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: column;
}

.logo {
  margin: 0 2rem 0 0;
}

.search-container {
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: column;
  /* Change the direction to row */
  gap: 10px;
  /* Add some space between the search box and the button */
}

.search-box {
  width: 30%;
  height: 50px;
  font-size: 1.5em;
  padding: 10px;
  border: 1px solid #ccc;
  border-radius: 5px;
  margin: 40px 0;
  text-align: center;
}

.pokemon-item {
  float: left;
  margin: 10px;
}
.pokemon-item a {
  color: #000;
  text-decoration: none;
  font-size: 16px;
  transition: color 0.3s ease;
  text-transform: capitalize;
}

.pokemon-item a:hover {
  color: #3B4CCA; /* Change color on hover */
}

ul {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(150px, 1fr));
  gap: 5px;
  list-style: none;
}

.pokemon-details {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  width: 30%;
  margin: 20px auto;
  padding: 20px;
  border: 1px solid #000;
  border-radius: 10px;
  color: #000;
  text-transform: capitalize;
}

.pokemon-details img {
  width: 100px;
  height: 100px;
}

</style>