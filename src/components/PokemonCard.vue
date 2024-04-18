<template>
    <li @click="mostrar_pokemon(pegar_id(pokemon))" :flat="flat">
        <h2>{{ pegar_name(pokemon) }}</h2>

        <img
            :src="`https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/${pegar_id(
                pokemon
            )}.png`"
            :alt="pokemon.name"
        />
    </li>
</template>
<script>
export default {
    props: {
        pokemon: Object,
        flat: { type: Boolean, default: false },
    },

    methods: {
        pegar_id(pokemon) {
            return Number(pokemon.url.split("/")[6]);
        },

        pegar_name(pokemon) {
            return pokemon.name;
        },

        mostrar_pokemon(id) {
            this.$emit("clicked", id);
            console.log(id);
        },
    },
};
</script>
