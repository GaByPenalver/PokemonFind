<template>
    <div>
    <img
        src="https://concordeeducation.com/games/wp-content/uploads/sites/3/2022/08/Pokemon-Logo.png"
        alt="Pokemon Logo"
    />
    <h1>¿Quién es ese Pokémon?</h1>
    <h2>Pokémon descubiertos: {{ discoveredCount }}</h2>
    <div class="pokemon">
        <PokemonCard v-for="pokemon in eventpokemon" :key="pokemon.name" :pokemon="pokemon" @pokemon-discovered="increaseDiscoveredCount"/>
    </div>
    </div>
</template>

<script>
import axios from 'axios';
import PokemonCard from './PokemonCard.vue';

export default {
    name: 'EventPokemon',
    components: {
        PokemonCard,
    },
    data() {
        return {
            eventpokemon: [],
        };
    },
    async mounted() {
        try {
            const response = await axios.get('https://pokeapi.co/api/v2/pokemon?limit=200');
            const allPokemon = response.data.results;

            const shuffledPokemon = this.shuffleArray(allPokemon).slice(0, 20);
            this.eventpokemon = shuffledPokemon.map((pokemon) => {
                const id = pokemon.url.split('/').filter(Boolean).pop();
                return {
                    name: pokemon.name,
                    imageUrl: `https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/${id}.png`,
                };
            });
        } catch (error) {
            console.error('Error fetching Pokémon data:', error);
        }
    },
    methods: {
        shuffleArray(array) {
            for (let i = array.length - 1; i > 0; i--) {
                const j = Math.floor(Math.random() * (i + 1));
                [array[i], array[j]] = [array[j], array[i]];
            }
            return array;
        },
    },
};
</script>

<style scoped>
.pokemon {
    display: flex;
    flex-wrap: wrap;
    gap: 1rem;
    justify-content: center;
}

img {
    width: 280px;
    height: 180px;
    display: block;
    margin: auto;
}

h1, h2 {
    text-align: center;
    margin-top: 0;
}
</style>