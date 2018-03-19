<template>
    <div class="container">
        <button @click.prevent="newGame">New Game</button>
        <grid></grid>
        <div class="message" :class="[success ? 'win' : 'lose']" v-if="status = 'gameover'">
            {{ message }}
        </div>
    </div>
</template>

<script>
    import Grid from './Grid.vue';
    import EventBus from './eventBus.js';

    export default {
        data() {
            return {
                status: '', 
                success: false,
                message: ''
            }
        },

        components: {
            Grid
        },

        created() {
            let self = this;

            this.status = 'playing';

            EventBus.$on('gameLost', () => {
                self.status = 'gameover';
                self.success = false;
                self.message = 'YOU LOSE!';
            });

            EventBus.$on('gameWon', () => {
                self.status = 'gameover';
                self.success = true;
                self.message = 'YOU WIN!';
            });
        },

        mounted() {
            EventBus.$emit('newGame');
        },

        methods: {
            newGame() {
                this.status = '';
                this.message = '';
                this.success = false;

                EventBus.$emit('newGame');
            }
        }
    }
</script>

<style>
    .container {
        margin-top: 100px;
    }

    .message {
        font-size: large;
        font-weight: bold;
        paddig-top: 200px;
    }

    .win {
        color: blue;
    }

    .lose {
        color: red;
    }
</style>

