<template>
    <div class="jogosPresente">
        <h4>Games que {{ pokemon.name }} está presente</h4>
        <ul class="listaGames">
            <li v-for="game in games" :key="game">{{ game }}</li>
        </ul>
    </div>
</template>

<script>
export default {
    props: {
        games: Array,
        pokemon: Object,
    },
};
</script>

<style scoped>
.jogosPresente > h4 {
    text-align: center;
    font-size: 18px;

    margin-bottom: 20px;

    @media (min-width: 769px) {
        font-size: 24px;
    }
}

.listaGames {
    display: flex;
    flex-direction: row;
    flex-wrap: wrap;
    gap: 12px;

    > li {
        background-color: #122657;
        color: white;

        padding: 4px 12px;
    }
}
</style>
