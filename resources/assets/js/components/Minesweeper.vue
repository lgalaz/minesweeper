<template>
    <div class="container">
        <grid></grid>
    </div>
</template>

<script>
    import Grid from './grid.vue';

    export default {
        components: {
            Grid
        },

        mounted() {
            console.log('Component mounted.')
        }
    }
</script>
