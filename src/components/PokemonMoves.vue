<template>
    <div class="movimentosDeAtaque">
        <h4>Movimentos de ataque do {{ pokemon.name }}</h4>
        <ul class="listaMovimentos">
            <li v-for="item in moves" :key="item.move.name">
                <span>{{ item.move.name }}</span>
            </li>
        </ul>
    </div>
</template>

<script>
export default {
    props: {
        moves: Array,
        pokemon: Object,
    },
};
</script>

<style scoped>
.movimentosDeAtaque > h4 {
    text-align: center;
    font-size: 18px;

    margin-bottom: 20px;

    @media (min-width: 769px) {
        font-size: 24px;
    }
}

.listaMovimentos {
    display: flex;
    flex-direction: row;
    flex-wrap: wrap;
    gap: 12px;

    max-height: 130px;
    overflow-y: scroll;

    > li {
        background-color: #122657;
        color: white;

        padding: 4px 12px;
    }
}
</style>
