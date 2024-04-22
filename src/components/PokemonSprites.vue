<template>
    <div class="sprites">
        <h4>Sprites</h4>
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
    },
};
</script>

<style scoped>
.sprites-container {
    padding: 0;

    list-style: none;

    display: flex;
    flex-direction: row;
    flex-wrap: wrap;
    align-items: center;
    justify-content: center;
}

.sprites-container li img {
    margin: 5px;
    max-width: 100px;
    max-height: 100px;
}
</style>
