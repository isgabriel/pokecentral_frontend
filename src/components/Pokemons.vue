<template>
    <h1>PokeCentral</h1>

    <ul>
        <li v-for="pokemon in pokemons" :key="pokemon.name">
            <PokemonCard :pokemon="pokemon" @clicked="mostraModalPokemon" />
        </li>
    </ul>

    <PokemonModal
        v-if="mostrarModal"
        :pokemon="pokemonSelecionado"
        @close="fechaModal"
    />
</template>

<script>
import axios from "axios";
import PokemonCard from "./PokemonCard.vue";
import PokemonModal from "./PokemonModal.vue";

export default {
    name: "Pokemons",

    components: { PokemonCard, PokemonModal },

    data() {
        return {
            pokemons: [],
            mostrarModal: false,
            pokemonSelecionado: null,
        };
    },

    mounted() {
        axios
            .get("https://pokeapi.co/api/v2/pokemon?limit=151")
            .then((response) => {
                this.pokemons = response.data.results;
            });
    },

    methods: {
        mostraModalPokemon(id) {
            axios
                .get(`https://pokeapi.co/api/v2/pokemon/${id}`)
                .then((response) => {
                    this.pokemonSelecionado = response.data;
                    this.mostrarModal = true;
                });
        },

        fechaModal() {
            this.mostrarModal = false;
        },
    },
};
</script>

<style scoped></style>
