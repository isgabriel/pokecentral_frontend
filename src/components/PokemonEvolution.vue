<template>
    <div>
        <h4>Evoluções</h4>
        <ul>
            <li v-for="(evolution_detail, index) in evolucoesPokemon()">
                <div
                    :key="`evolucao-${index}`"
                    v-if="typeof evolution_detail == 'object'"
                >
                    <h1>{{ evolution_detail.name }}</h1>
                    <img
                        :src="`https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/${
                            evolution_detail.url.split('/')[6]
                        }.png`"
                        :alt="pokemon.name"
                        class="imgPokemon"
                    />
                </div>
                <div :key="`evolucao${index}`" v-else>
                    <h3>L {{ evolution_detail }}</h3>
                </div>
            </li>
        </ul>
    </div>
</template>

<script>
import axios from "axios";

export default {
    data() {
        return {
            evolution: null,
        };
    },
    props: {
        pokemon: Object,
    },
    mounted() {
        this.fetchEvolucao();
    },
    methods: {
        fetchEvolucao() {
            axios
                .get(
                    `https://pokeapi.co/api/v2/pokemon-species/${this.pokemon.id}`
                )
                .then((response) => {
                    axios
                        .get(response.data.evolution_chain.url)
                        .then((response) => {
                            this.evolution = response.data.chain;
                        });
                });
        },
        evolucoesPokemon() {
            let correnteEvolucao = [];
            let evolucao = this.evolution;
            if (evolucao && evolucao.species) {
                correnteEvolucao.push(evolucao.species);
            }

            while (evolucao && evolucao.species) {
                if (evolucao.evolves_to.length > 0) {
                    evolucao = evolucao.evolves_to[0];

                    if (
                        evolucao.evolution_details.length > 0 &&
                        evolucao.evolution_details[0].min_level
                    ) {
                        correnteEvolucao.push(
                            evolucao.evolution_details[0].min_level
                        );
                    }

                    if (evolucao.species) {
                        correnteEvolucao.push(evolucao.species);
                    }
                } else {
                    break;
                }
            }

            return correnteEvolucao;
        },
    },
    watch: {
        pokemon() {
            this.fetchEvolucao();
        },
    },
};
</script>

<style scoped>
.imgPokemon {
    width: 60px;
    height: 60px;
}
</style>
