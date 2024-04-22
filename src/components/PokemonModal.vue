<template>
    <section v-if="pokemon" class="modal">
        <div class="modal-conteudo">
            <div class="headerModal">
                <h2>Informações do {{ pokemon.name }}</h2>
                <span class="close" @click="fechaModal">X</span>
            </div>

            <!-- <p>ID: {{ pokemon.id }}</p> -->

            <PokemonSprites :sprites="pokemon.sprites" :pokemon="pokemon" />

            <PokemonMoves :moves="movesDeAtaquePokemon" :pokemon="pokemon" />

            <PokemonEvolution :pokemon="pokemon" />

            <PokemonGames :games="gamesDoPokemon" :pokemon="pokemon" />
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
    background-color: #d2a700;

    position: relative;

    width: 90%;
    max-width: 800px;
    min-height: 60vh;
    max-height: 90vh;
    overflow-y: scroll;

    padding: 0 18px 18px 18px;

    display: flex;
    flex-direction: column;
    align-items: center;
    gap: 30px;
}
.headerModal {
    display: flex;
    flex-direction: row;
    justify-content: space-between;
    align-items: center;

    position: sticky;
    top: 0;

    width: 100%;

    background-color: #d2a700;

    padding: 18px;
    margin: 0 auto;

    text-transform: uppercase;
    font-size: 12px;
    font-weight: 500;

    > span {
        font-size: 28px;
        font-weight: 700;
        cursor: pointer;
    }
}
</style>
