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
                type="text"
                v-model.number="filtroID"
                placeholder="Filtrar por ID"
                id="inputId"
                @input="filtrarNumerosDoInput"
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

        <div>
            <label for="inputEspecie">FILTRAR POR ESPÉCIE</label>
            <select v-model="filtroEspecie" id="inputEspecie">
                <option value="">Todas</option>
                <option
                    v-for="especie in especiesUnicas"
                    :key="especie"
                    :value="especie"
                >
                    {{ especie }}
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
            filtroEspecie: "",
            especiesUnicas: [],
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

            if (this.filtroID) {
                listaFiltrada = listaFiltrada.filter((pokemon) => {
                    return pokemon.id.toString().includes(this.filtroID);
                });
            }

            if (this.filtroTipo) {
                listaFiltrada = listaFiltrada.filter((pokemon) => {
                    return pokemon.types.includes(this.filtroTipo);
                });
            }

            if (this.filtroEspecie) {
                listaFiltrada = listaFiltrada.filter((pokemon) => {
                    return pokemon.species === this.filtroEspecie;
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
                    image: data.sprites.other["official-artwork"].front_default,
                    types: data.types.map((type) => type.type.name),
                    species: data.species.name,
                    id: data.id,
                    sprites: data.sprites,
                }));
                await this.atualizaEspeciesUnicas();
            } catch (error) {
                console.error("Erro ao carregar os pokémons:", error);
            }
        },

        async atualizaEspeciesUnicas() {
            const tipos = new Set();
            const especies = new Set();
            this.pokemons.forEach((pokemon) => {
                pokemon.types.forEach((type) => tipos.add(type));
                especies.add(pokemon.species);
            });
            this.tiposUnicos = Array.from(tipos);
            this.especiesUnicas = Array.from(especies);
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

        filtrarNumerosDoInput(event) {
            const input = event.target;
            const filteredValue = input.value.replace(/\D/g, "");
            this.filtroID =
                filteredValue !== "" ? parseInt(filteredValue) : null;
        },
    },
};
</script>

<style scoped></style>
