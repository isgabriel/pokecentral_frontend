<!-- PokemonModal.vue -->
<template>
    <section v-if="pokemon" class="modal">
        <div class="modal-conteudo">
            <span class="close" @click="fechaModal">X</span>
            <h2>{{ pokemon.name }}</h2>
            <p>ID: {{ pokemon.id }}</p>

            <!-- Aqui vou implementar todas as sprites (penso em fazer em forma de carrossel) -->
            <div>
                <h4>Sprites</h4>
            </div>

            <div class="tabela">
                <h4>Movimentos de ataque</h4>

                <table>
                    <thead>
                        <tr>
                            <th>Nome</th>
                        </tr>
                    </thead>
                    <tbody>
                        <tr
                            v-for="item in movimentosDeAtaquePokemon(pokemon)"
                            :key="item.move.name"
                        >
                            <td>
                                <span>{{ item.move.name }}</span>
                            </td>
                        </tr>
                    </tbody>
                </table>
            </div>

            <div>
                <h4>Evoluções</h4>
                <!-- Aqui vou implementar todas as evoluções do pokemon, caso ele tenha -->
            </div>

            <div>
                <h4>Games que este pokemon está presente</h4>
                <!-- Aqui vou implementar os games que o pokemon está presente -->
            </div>
        </div>
    </section>
</template>

<script>
export default {
    props: {
        pokemon: Object,
    },

    methods: {
        fechaModal() {
            this.$emit("close");
        },

        movimentosDeAtaquePokemon(pokemon) {
            return pokemon.moves.filter((item) => {
                let incluiNaTabela = false;

                for (let versao of item.version_group_details) {
                    if (
                        versao.version_group.name == "sword-shield" ||
                        versao.move_learn_method.name == "machine" ||
                        versao.move_learn_method.name == "egg" ||
                        versao.move_learn_method.name == "tutor"
                    ) {
                        incluiNaTabela = true;
                    }
                }
                return incluiNaTabela;
            });
        },
    },

    computed: {
        // Aqui eu vou implementar os filtros depois de desenvolver a lógica do card
    },
};
</script>

<style scoped>
.modal {
    position: fixed;
    top: 0;
    left: 0;

    background-color: rgba(0, 0, 0, 0.6);
    color: white;

    width: 100%;
    height: 100vh;

    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
}

.modal-conteudo {
    background-color: #d9d9d9;
    height: 60vh;
    width: 90%;
    max-width: 800px;

    .tabela {
        background-color: #b6b6b6;
        max-height: 200px;
        overflow-y: scroll;

        > h4 {
            margin: 0;
            background-color: inherit;
            position: sticky;
            top: 0;
        }
    }
}

.close {
    cursor: pointer;
    font-size: 35px;
}
</style>
