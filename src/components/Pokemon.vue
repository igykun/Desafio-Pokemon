<template>
  <div class="page-container">
    <h1 class="anime-title">¿Quién es este Pokémon?</h1>
    <div class="row">
      <div class="cards-grid col-6 col-md-3">
        <PokemonCard
          v-for="(pokemon, index) in pokemons"
          :key="index"
          :name="pokemon.name"
          :image="pokemon.image"
          @pokemon-discovered="increaseDiscoveredCount"
        />
      </div>
    </div>
    <p class="discovered-counter">Pokémon descubiertos: {{ discoveredCount }}/{{ pokemons.length }}</p>
  </div>
</template>
  
  <script>
  import PokemonCard from './PokemonCard.vue';
  import axios from 'axios';
  
  export default {
    components: { PokemonCard },
    data() {
      return {
        pokemons: [],
        discoveredCount: 0,
      };
    },
    async mounted() {
      try {
        const response = await axios.get('https://pokeapi.co/api/v2/pokemon?limit=100');
        const shuffledPokemons = response.data.results.sort(() => 0.5 - Math.random());
        const selectedPokemons = shuffledPokemons.slice(0, 20);
  
        
        const pokemonDetails = await Promise.all(
          selectedPokemons.map((pokemon) => axios.get(pokemon.url))
        );
  
        this.pokemons = pokemonDetails.map((detail) => ({
          name: detail.data.name,
          image: detail.data.sprites.other['official-artwork'].front_default,
        }));
      } catch (error) {
        console.error('Error al obtener los Pokémon:', error);
      }
    },
    methods: {
      increaseDiscoveredCount() {
        this.discoveredCount++;
      },
    },
  };
  </script>
  
  <style scoped>
  .pokemon-container {
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    font-family: 'Arial', sans-serif;
    text-align: center;
  }
  
  .cards-grid {
    display: grid;
    grid-template-columns: repeat(4, 1fr);
    gap: 20px;
    margin: 20px 0;
    width: 100%;
    max-width: 1200px;
  }
  
  h1 {
    color: #3c5aa6;
  }
  
  p {
    font-size: 16px;
    color: #555;
  }

  .page-container {
  background: linear-gradient(145deg, #ffcb05, #ff3c00);
  background-image: radial-gradient(circle at 20% 50%, rgba(255, 255, 255, 0.2) 10%, transparent 30%), 
                    repeating-linear-gradient(-45deg, rgba(0, 0, 0, 0.1), rgba(0, 0, 0, 0.1) 5px, transparent 5px, transparent 10px);
  height: 100vh;
  padding: 20px;
  color: #fff;
  display: flex;
  flex-direction: column;
  align-items: center;
  font-family: 'Impact', 'Arial', sans-serif;
}

  .anime-title {
  font-size: 2.5rem;
  text-transform: uppercase;
  color: #ffda00;
  text-shadow: 3px 3px 0px #000, 6px 6px 0px #ff3c00, 9px 9px 0px #000;
  margin-bottom: 30px;
}

  .discovered-counter {
  font-size: 1.2rem;
  font-weight: bold;
  margin-top: 20px;
  text-shadow: 1px 1px 0px #000;
}
  </style>
  
  