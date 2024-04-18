<template>
    <div @click="mostrarPokemon(pegarId(pokemon))" :flat="flat">
        <h2>{{ pegarNomePokemon(pokemon) }}</h2>
        <img
            :src="`https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/${pegarId(
                pokemon
            )}.png`"
            :alt="pokemon.name"
        />
    </div>
</template>

<script>
export default {
    props: {
        pokemon: Object,
        flat: { type: Boolean, default: false },
    },
    methods: {
        pegarId(pokemon) {
            return Number(pokemon.url.split("/")[6]);
        },
        pegarNomePokemon(pokemon) {
            return pokemon.name;
        },
        mostrarPokemon(id) {
            this.$emit("clicked", id);
        },
    },
};
</script>
