<template>
    <div>
        <h4>Movimentos de ataque</h4>
        <ul class="lista-movimentos">
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
    },
};
</script>

<style scoped></style>
