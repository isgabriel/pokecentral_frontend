<template>
    <div>
        <h4>Games que este pokemon está presente</h4>
        <ul class="lista-games">
            <li v-for="game in games" :key="game">{{ game }}</li>
        </ul>
    </div>
</template>

<script>
export default {
    props: {
        games: Array,
    },
};
</script>

<style scoped>
/* Estilos específicos do componente, se necessário */
</style>
