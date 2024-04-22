<template>
    <div @click="mostrarPokemon(pokemon.id)" :flat="flat" className="container">
        <img v-if="pokemon.image" :src="pokemon.image" :alt="pokemon.name" />
        <h2>{{ pegarNomePokemon(pokemon) }}</h2>
        <div class="typesContainer">
            <span v-for="type in pokemon.types">
                {{ type }}
            </span>
        </div>
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

<style scoped>
.container {
    display: flex;
    flex-direction: column;
    align-items: center;

    cursor: pointer;

    width: 128px;
    height: 223px;
    padding: 28px 12px;
}
div h2 {
    margin-top: 20px;

    font-size: 18px;
    font-weight: 700;
    text-transform: uppercase;
}
img {
    width: 60px;
}
.typesContainer {
    display: flex;
    flex-direction: column;
    gap: 7px;

    width: 100%;

    margin-top: 10px;

    span {
        text-align: center;
        background-color: #122657;
        color: white;

        padding: 6px 9px;

        border-radius: 20px;
    }
}
</style>
