<template>
    <div>
        <h1>PokeCentral</h1>

        <!-- Filtros -->
        <div>
            <label for="">FILTRAR POR NOME</label>
            <input
                type="text"
                v-model="filtroNome"
                placeholder="Filtrar por nome"
            />
        </div>
        <div>
            <label for="">FILTRAR POR ID</label>
            <input
                type="number"
                v-model.number="filtroID"
                placeholder="Filtrar por ID"
            />
        </div>

        <ul>
            <li v-for="pokemon in pokemonsFiltrados" :key="pokemon.name">
                <PokemonCard :pokemon="pokemon" @clicked="mostraModalPokemon" />
            </li>
        </ul>

        <PokemonModal
            v-if="mostrarModal"
            :pokemon="pokemonSelecionado"
            @close="fechaModal"
        />
    </div>
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
            filtroNome: "",
            filtroID: null,
        };
    },

    mounted() {
        axios
            .get("https://pokeapi.co/api/v2/pokemon?limit=151")
            .then((response) => {
                this.pokemons = response.data.results.map((pokemon, index) => ({
                    ...pokemon,
                    id: index + 1, //+1 porque os ids da API iniciam em 1
                }));
            });
    },

    computed: {
        pokemonsFiltrados() {
            let listaFiltrada = this.pokemons;

            if (this.filtroNome) {
                listaFiltrada = listaFiltrada.filter((pokemon) => {
                    return pokemon.name
                        .toLowerCase()
                        .includes(this.filtroNome.toLowerCase());
                });
            }

            if (this.filtroID !== null) {
                listaFiltrada = listaFiltrada.filter((pokemon) => {
                    return pokemon.id === parseInt(this.filtroID);
                });
            }

            if (!this.filtroNome && this.filtroID === null) {
                return this.pokemons;
            }

            return listaFiltrada;
        },
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
