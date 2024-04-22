<template>
    <main class="containerPrincipal">
        <a href="#" class="logo">
            <img src="/logo.svg" alt="" />
        </a>

        <section class="secaoFiltros">
            <div class="filtroContainer">
                <label for="inputNome">FILTRAR POR NOME</label>
                <input
                    type="text"
                    v-model="filtroNome"
                    placeholder="Digite um nome..."
                    id="inputNome"
                />
            </div>
            <div class="filtroContainer">
                <label for="inputId">FILTRAR POR ID</label>
                <input
                    type="text"
                    v-model.number="filtroID"
                    placeholder="Digite um ID..."
                    id="inputId"
                    @input="filtrarNumerosDoInput"
                />
            </div>

            <div class="filtroContainer">
                <label for="inputTipo">FILTRAR POR TIPO</label>
                <select v-model="filtroTipo" id="inputTipo">
                    <option value="">Todos</option>
                    <option
                        v-for="tipo in tiposUnicos"
                        :key="tipo"
                        :value="tipo"
                    >
                        {{ tipo }}
                    </option>
                </select>
            </div>

            <div class="filtroContainer">
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
        </section>

        <section class="secaoPokemonsTextos">
            <h1>Pokemons</h1>
            <p>
                Você pode acessar mais informações do Pokemon clicando no card
                que desejar!
            </p>
        </section>
        <ul class="listaPokemons" role="list">
            <li
                v-for="pokemon in pokemonsFiltrados"
                :key="pokemon.name"
                class="listaItem"
            >
                <PokemonCard :pokemon="pokemon" @clicked="mostraModalPokemon" />
            </li>
        </ul>

        <PokemonModal
            v-if="mostrarModal"
            :pokemon="pokemonSelecionado"
            @close="fechaModal"
        />
    </main>
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

<style>
.containerPrincipal {
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;

    width: 100%;
}
.logo {
    margin: 40px 0;
}
.secaoFiltros {
    display: flex;
    flex-direction: column;
    align-items: center;
    gap: 15px;

    width: 90%;

    margin-bottom: 40px;

    @media (min-width: 769px) {
        flex-direction: row;
        flex-wrap: wrap;
        justify-content: center;
        gap: 30px;
    }
}
.filtroContainer {
    display: flex;
    flex-direction: column;
    gap: 2px;

    > label {
        color: white;
        font-size: 12px;
        letter-spacing: 0.6px;
    }
    > input,
    > select {
        width: 288px;

        padding: 12px;

        border: 1px solid #d9d9d9;

        background-color: rgba(0, 0, 0, 0.35);
        color: #d9d9d9;
        &::placeholder {
            color: #d9d9d960;
        }

        &:focus {
            outline: 0;
            border: 1px solid #ff1c1c;
        }
    }
}

.secaoPokemonsTextos {
    display: flex;
    flex-direction: column;
    gap: 10px;

    width: 90%;

    color: white;

    margin-bottom: 20px;

    > h1 {
        font-size: 26px;
        font-weight: 700;
    }
    > p {
        font-size: 12px;
        font-weight: 300;
        line-height: 20px;
    }

    @media (min-width: 769px) {
        margin-bottom: 40px;
        > h1 {
            font-size: 56px;
        }

        > p {
            font-size: 16px;
        }
    }
}
.listaPokemons {
    padding: 0;
    width: 90%;

    display: flex;
    flex-direction: row;
    flex-wrap: wrap;
    justify-content: center;
    align-items: center;
    row-gap: 32px;
    column-gap: 15px;
}
.listaItem {
    background-color: #d2a700;
}
</style>
