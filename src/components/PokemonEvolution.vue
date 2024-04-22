<template>
    <div class="evolucaoContainer">
        <h4>Linha de Evolução do {{ pokemon.name }}</h4>

        <section class="divEvolucao">
            <div v-for="(evolution_detail, index) in evolucoesPokemon()">
                <div
                    :key="`evolucao-${index}`"
                    v-if="typeof evolution_detail == 'object'"
                >
                    <img
                        :src="`https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/${
                            evolution_detail.url.split('/')[6]
                        }.png`"
                        :alt="pokemon.name"
                        class="imgPokemon"
                    />
                    <h1 class="pokemonNome">{{ evolution_detail.name }}</h1>
                </div>
                <section :key="`evolucao${index}`" v-else>
                    <p class="nivelEvolucao">lvl {{ evolution_detail }}</p>
                </section>
            </div>
        </section>
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
.evolucaoContainer {
    padding: 15px 0;
    > h4 {
        text-align: center;
        font-size: 18px;

        margin-bottom: 10px;

        @media (min-width: 769px) {
            font-size: 24px;
        }
    }
}
.divEvolucao {
    display: flex;
    flex-direction: column;
    align-items: center;
    position: relative;
    gap: 20px;

    > div > div {
        display: flex;
        flex-direction: column;
        align-items: center;
    }

    .pokemonNome {
        font-size: 18px;
    }
    .nivelEvolucao {
        font-size: 14px;
        font-weight: 700;
    }
    @media (min-width: 769px) {
        flex-direction: row;
        justify-content: center;
        .pokemonNome {
            font-size: 20px;
        }
        .nivelEvolucao {
            font-size: 16px;
            font-weight: 700;
        }
    }
}

.imgPokemon {
    width: 100px;
    height: 100px;
}
</style>
