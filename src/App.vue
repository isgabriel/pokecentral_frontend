<script setup>
import Pokemons from "./components/Pokemons.vue";
</script>

<template>
    <Pokemons />
</template>
<style>
* {
    font-family: "Quicksand", sans-serif;
    margin: 0;
    padding: 0;
    box-sizing: border-box;
}
#app {
    width: 100%;
    height: 100%;

    min-height: 100vh;

    background-color: #122657;
}

ul {
    list-style-type: none;
}
</style>
