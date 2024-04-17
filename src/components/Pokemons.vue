<script setup></script>

<template>
    <h1>PokeCentral</h1>

    <ul>
        <li v-for="pokemon in pokemons" :key="pokemon.name">
            {{ pegar_id(pokemon) }}
            <h2>{{ pokemon.name }}</h2>

            <img
                :src="`https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/${pegar_id(
                    pokemon
                )}.png`"
                :alt="pokemon.name"
            />
        </li>
    </ul>
</template>

<script>
import axios from "axios";

export default {
    name: "Pokemons",

    components: {},

    data() {
        return {
            pokemons: [],
        };
    },

    mounted() {
        axios
            .get("https://pokeapi.co/api/v2/pokemon?limit=150")
            .then((response) => {
                this.pokemons = response.data.results;
            });
    },

    methods: {
        pegar_id(pokemon) {
            return Number(pokemon.url.split("/")[6]);
        },
    },
};
</script>
