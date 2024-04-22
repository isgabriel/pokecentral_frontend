<template>
    <div class="sprites">
        <h4>Sprites do {{ pokemon.name }}</h4>
        <ul class="sprites-container">
            <li v-for="(sprite, key) in sprites" :key="key">
                <img
                    :src="sprite"
                    :alt="key"
                    v-if="typeof sprite === 'string' && sprite !== 'null'"
                />
            </li>
        </ul>
    </div>
</template>

<script>
export default {
    props: {
        sprites: Object,
        pokemon: Object,
    },
};
</script>

<style scoped>
.sprites > h4 {
    text-align: center;
    font-size: 18px;

    margin-bottom: 20px;

    @media (min-width: 769px) {
        font-size: 24px;
    }
}

.sprites-container {
    display: flex;
    flex-direction: row;
    flex-wrap: wrap;
    align-items: center;
    justify-content: center;
}

.sprites-container li img {
    width: 100px;
    height: 100px;
}
</style>
