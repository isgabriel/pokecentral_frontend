<template>
    <div @click="mostrarPokemon(pokemon.id)" :flat="flat">
        <h2>{{ pegarNomePokemon(pokemon) }}</h2>
        <img v-if="pokemon.image" :src="pokemon.image" :alt="pokemon.name" />
    </div>
</template>

<script>
export default {
    props: {
        pokemon: Object,
        flat: { type: Boolean, default: false },
    },
    methods: {
        pegarNomePokemon(pokemon) {
            return pokemon.name;
        },
        mostrarPokemon(id) {
            this.$emit("clicked", id);
        },
    },
};
</script>
