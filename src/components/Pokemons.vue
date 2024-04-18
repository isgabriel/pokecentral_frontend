<script setup></script>

<template>
    <h1>PokeCentral</h1>

    <ul>
        <li v-for="pokemon in pokemons" :key="pokemon.name">
            <PokemonCard :pokemon="pokemon" @clicked="mostrar_pokemon" />
        </li>
    </ul>

    <section v-if="mostra_modal" :class="mostra_modal ? 'mostra' : 'esconde'">
        <div>
            <h2>{{ pokemon_selecionado.name }}</h2>
            <p>ID: {{ pokemon_selecionado.id }}</p>

            <!-- Aqui vou implementar todas as sprites (penso em fazer em forma de carrossel) -->
            <div>
                <h4>Sprites</h4>
            </div>

            <div class="tabela">
                <h4>Movimentos de ataque</h4>

                <table>
                    <thead>
                        <tr>
                            <th>Nome</th>
                        </tr>
                    </thead>
                    <tbody>
                        <tr
                            v-for="item in movimentos_filtrados(
                                pokemon_selecionado
                            )"
                            :key="item.move.name"
                        >
                            <td>
                                <span>{{ item.move.name }}</span>
                            </td>
                        </tr>
                    </tbody>
                </table>
            </div>

            <div>
                <h4>Evoluções</h4>
                <!-- Aqui vou implementar todas as evoluções do pokemon, caso ele tenha -->
            </div>

            <div>
                <h4>Games que este pokemon está presente</h4>
                <!-- Aqui vou implementar os games que o pokemon está presente -->
            </div>
        </div>
    </section>
</template>

<script>
import axios from "axios";
import PokemonCard from "./PokemonCard.vue";

export default {
    name: "Pokemons",

    components: { PokemonCard },

    data() {
        return {
            pokemons: [],
            mostra_modal: false,
            pokemon_selecionado: null,
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
        mostra_pokemon(id) {
            axios
                .get(`https://pokeapi.co/api/v2/pokemon/${id}`)
                .then((response) => {
                    this.pokemon_selecionado = response.data;
                    this.mostra_modal = !this.mostra_modal;
                });
        },

        //refatorar para componente modal
        movimentos_filtrados(pokemon) {
            return pokemon.moves.filter((item) => {
                let incluiNaTabela = false;

                for (let versao of item.version_group_details) {
                    if (
                        versao.version_group.name == "sword-shield" ||
                        versao.move_learn_method.name == "machine" ||
                        versao.move_learn_method.name == "egg" ||
                        versao.move_learn_method.name == "tutor"
                    ) {
                        incluiNaTabela = true;
                    }
                }
                return incluiNaTabela;
            });
        },
    },

    computed: {
        // Aqui eu vou implementar os filtros depois de desenvolver a lógica do card
    },
};
</script>
<style scoped>
.mostra {
    position: fixed;
    top: 0;
    left: 0;
    z-index: 10;

    background-color: rgba(0, 0, 0, 0.8);

    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;

    > div {
        background-color: #d9d9d9;

        width: 80%;
        display: flex;
        flex-direction: column;
        align-items: center;
    }

    width: 100%;
    height: 100%;
}
.esconde {
    display: none;
}

.tabela {
    max-height: 50vh;
    overflow-y: scroll;

    > h4 {
        position: sticky;
        top: 0;
        background-color: #d9d9d9;
    }
}
</style>
