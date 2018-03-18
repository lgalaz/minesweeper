<template>
    <div class="grid">
        <div class="row" v-for="row in rows">
            <cell v-for="column in columns"></cell>
        </div>
    </div>
</template>

<script>
    import Cell from './cell.vue';

    export default {
        data() {
            return {
                rows: 10,
                columns: 10,
                mines: 20,
                cells : [],
                hasRemainingCells : true
            }
        },

        components: {
            Cell
        },

        watch: {
            cells() {
                this.hasRemainingCells = true;
            }
        },

        methods: {
            startNewGame() {

            },

            createGrid() {

            },

            flipCell() {

            },

            isCellInGrid() {

            },

            getAdjacentCells(row, column) {
                
            } 
        }
    }
</script>

<style>
    .grid {
        text-align: center;
        margin-top: 100px;
    }
    .row {
        display: flex;
    }
</style>
