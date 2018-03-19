<template>
    <div class="grid">
        <div class="row" v-for="row in this.grid">
            <template v-for="column in row">
                <cell :item="column"></cell>
            </template>
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
                grid : [],
                hasRemainingCells : true
            }
        },

        components: {
            Cell
        },

        created() {
            this.createGrid();

            // this.grid.forEach((cell) => { 
            //     this.setAdjacentCells(cell);
            // });
        },

        methods: {
            createGrid() {
                for (let x = 0; x < this.rows; x++) {
                    let row = [];

                    for (let y = 0; y < this.columns; y++) {
                        let cell = this.createCell(x, y);

                        row.push(cell);
                    }

                    this.grid.push(row);
                }

                return this;
            },

            createCell(row, column) {
                return {
                    row,
                    column,
                    isMine : false,
                    isEmpty : false,
                    adjacentcells : [],
                    adjacentMines:  0,
                    status : 'facedown',
                };
            },

            setRandomMines() {
                for (let i = 0; i < mines; i++) {
                    let randomRow = Math.floor(Math.random() * this.rows);
                    let randomColumn = Math.floor(Math.random() * this.columns);

                    if (this.isCellOnGrid({randomRow, randomColumn})) {
                        let candidateCell = this.grid[randomRow][randomColumn];

                        if (! candidateCell.isMine) {
                            candidateCell.isMine = true;
                        }
                    }
                }

                return this;
            }, 

            

            isCellOnGrid(x, y) {
                return x < 0 || x >= rows ||
                    y < 0 || y >= columns;
            },

            setAdjacentCells(cell) {                
                [
                    {row : -1, column : -1}, 
                    {row : -1, column : 0}, 
                    {row : 1, column : 1}
                ].forEach( (coord) => {
                    if (isCellOnGrid(coord.row + cell.row, coord.column + cell.column))  {
                        let neighborCell = this.grid[cell.row][cell.column];

                        adjacentCells.push(neighborCell);

                        if (neighborCell.isMine) {
                            adjacentMines++;
                        }
                    }
                })

                return adjacentCells;
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
