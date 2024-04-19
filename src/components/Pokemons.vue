<template>
    <div>
        <h1>PokeCentral</h1>

        <!-- Filtros -->
        <div>
            <label for="inputNome">FILTRAR POR NOME</label>
            <input
                type="text"
                v-model="filtroNome"
                placeholder="Filtrar por nome"
                id="inputNome"
            />
        </div>
        <div>
            <label for="inputId">FILTRAR POR ID</label>
            <input
                type="number"
                v-model.number="filtroID"
                placeholder="Filtrar por ID"
                id="inputId"
            />
        </div>

        <div>
            <label for="inputTipo">FILTRAR POR TIPO</label>
            <select v-model="filtroTipo" id="inputTipo">
                <option value="">Todos</option>
                <option v-for="tipo in tiposUnicos" :key="tipo" :value="tipo">
                    {{ tipo }}
                </option>
            </select>
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
            filtroTipo: "",
            tiposUnicos: [],
        };
    },

    mounted() {
        this.fetchPokemons();
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

            if (this.filtroTipo) {
                listaFiltrada = listaFiltrada.filter((pokemon) => {
                    return pokemon.types.includes(this.filtroTipo);
                });
            }

            return listaFiltrada;
        },
    },

    methods: {
        async fetchPokemons() {
            try {
                const response = await axios.get(
                    "https://pokeapi.co/api/v2/pokemon?limit=151"
                );
                const promises = response.data.results.map((result) => {
                    const url = result.url;
                    return axios.get(url).then((res) => res.data);
                });

                const pokemonData = await Promise.all(promises);

                this.pokemons = pokemonData.map((data, index) => ({
                    name: data.name,
                    image: data.sprites.other["official-artwork"].front_default, //imagens mais bonitas
                    types: data.types.map((type) => type.type.name),
                    id: data.id,
                }));
                await this.atualizaTiposUnicos();
            } catch (error) {
                console.error("Erro ao carregar os pokémons:", error);
            }
        },

        atualizaTiposUnicos() {
            const tipos = new Set();
            this.pokemons.forEach((pokemon) => {
                pokemon.types.forEach((type) => tipos.add(type));
            });
            this.tiposUnicos = Array.from(tipos);
        },

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
