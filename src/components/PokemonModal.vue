<template>
    <section v-if="pokemon" class="modal">
        <div class="modal-conteudo">
            <span class="close" @click="fechaModal">X</span>
            <h2>{{ pokemon.name }}</h2>
            <p>ID: {{ pokemon.id }}</p>

            <PokemonSprites :sprites="pokemon.sprites" />

            <PokemonMoves :moves="movesDeAtaquePokemon" />

            <PokemonGames :games="gamesDoPokemon" />

            <PokemonEvolution :pokemon="pokemon" />
        </div>
    </section>
</template>

<script>
import PokemonSprites from "./PokemonSprites.vue";
import PokemonEvolution from "./PokemonEvolution.vue";
import PokemonGames from "./PokemonGames.vue";
import PokemonMoves from "./PokemonMoves.vue";

export default {
    props: {
        pokemon: Object,
    },

    components: {
        PokemonSprites,
        PokemonEvolution,
        PokemonGames,
        PokemonMoves,
    },

    methods: {
        fechaModal() {
            this.$emit("close");
        },
    },

    computed: {
        movesDeAtaquePokemon() {
            if (!this.pokemon) return [];

            return this.pokemon.moves.filter((item) => {
                let incluiNaTabela = false;

                for (let versao of item.version_group_details) {
                    incluiNaTabela = true;
                }
                return incluiNaTabela;
            });
        },

        gamesDoPokemon() {
            if (!this.pokemon) return [];

            return this.pokemon.game_indices.map((game) => {
                return game.version.name.replace(/-/g, " ");
            });
        },
    },
};
</script>

<style scoped>
.modal {
    position: fixed;
    top: 0;
    left: 0;

    background-color: rgba(0, 0, 0, 0.6);
    color: black;

    width: 100%;
    height: 100vh;

    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
}

.modal-conteudo {
    background-color: #d9d9d9;

    width: 90%;
    max-width: 800px;
    max-height: 60vh;
    overflow-y: scroll;
}

.lista-movimentos,
.lista-games {
    max-height: 200px;

    padding: 0;

    overflow-y: auto;
}

.lista-movimentos li,
.lista-games li {
    padding: 8px;

    border-bottom: 1px solid #b6b6b6;
}
</style>
