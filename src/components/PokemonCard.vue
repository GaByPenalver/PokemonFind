<template>
    <div class="pokemon-card">
    <h2 v-if="isDiscovered">{{ pokemon.name }}</h2>
    <img :src="pokemon.imageUrl" :class="{ blurred: !isDiscovered }" alt="Pokemon"/>
    <div v-if="!isDiscovered">
        <input v-model="userInput" @keyup.enter="checkName" type="text"/>
        <button @click="checkName"><strong>Descubrir</strong></button>
    </div>
    </div>
</template>

<script>
export default {
    name: "PokemonCard",
    props: {
    pokemon: {
        type: Object,
        required: true,
    },
    },
    data() {
    return {
        userInput: "",
        isDiscovered: false,
    };
    },
    methods: {
    checkName() {
        if (this.userInput.toLowerCase() === this.pokemon.name.toLowerCase()) {
        this.isDiscovered = true;
        this.$emit("pokemon-discovered");
        } else {
        alert("¡Nombre incorrecto! Intenta de nuevo.");
        }
    },
    },
};
</script>

<style scoped>
.pokemon-card {
    padding: 1rem;
    text-align: center;
    width: 200px;
}
.pokemon-card img {
    width: 100%;
    transition: filter 0.3s ease;
}
.pokemon-card img.blurred {
    filter: blur(5px);
}
input {
    background-color: #33332f;
    color: white;
    margin: 0.5rem 0;
    border: 2px solid #82827d;
    padding: 7px;
    width: 85%;
    margin-bottom: 15px;
}

button {
    box-shadow: 0 0 5px #FF2929, 
                0 0 15px #FF2929,
                0 0 25px #FF2929, 
                0 0 50px #FF2929;
    color: white;
    background-color: #FF2929; 
    padding: 15px 35px; 
    border: none;
    border-radius: 5px;
    font-size: 18px;
    cursor: pointer;
    transition: transform 0.3s ease, box-shadow 0.3s ease;
}
button:hover {
    transform: scale(1.2); 
    box-shadow: 0 0 10px #FF2929, 
                0 0 20px #FF2929,
                0 0 40px #FF2929, 
                0 0 80px #FF2929; 
}
</style>
