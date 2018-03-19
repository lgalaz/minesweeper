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
                rows: 2,
                columns: 2,
                mines: 1,
                grid : [],
                hasRemainingCells : true
            }
        },

        components: {
            Cell
        },

        created() {
            this.createGrid().setRandomMines().setAdjacentCells();
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
                    adjacentCells : [],
                    adjacentMines:  0,
                    status : 'facedown',
                };
            },

            setRandomMines() {
                for (let i = 0; i < this.mines; i++) {
                    let randomRow = Math.floor(Math.random() * this.rows);
                    let randomColumn = Math.floor(Math.random() * this.columns);

                    if (this.isCellOnGrid(randomRow, randomColumn)) {
                        let candidateCell = this.grid[randomRow][randomColumn];

                        if (! candidateCell.isMine) {
                            console.log('setting mine');
                            candidateCell.isMine = true;
                        }
                    }
                }

                return this;
            }, 

            isCellOnGrid(x, y) {
                return x >= 0 && x < this.rows &&
                    y >= 0 && y < this.columns;
            },

            setAdjacentCells(cell) {     
                let borderCoords =  [
                    {row : -1, column : -1}, 
                    {row : -1, column : 0}, 
                    {row : -1, column : 1},
                    {row : 0, column : -1},
                    {row : 0, column : 1},
                    {row : 1, column : -1},
                    {row : 1, column : 0},
                    {row : 1, column : 1}
                ]

                this.grid.forEach((row) => { 
                    row.forEach((cell) => {
                        borderCoords.forEach((coord) => {
                            let neighborRow = coord.row + cell.row;
                            let neighborColumn = coord.column + cell.column;

                            if (this.isCellOnGrid(neighborRow, neighborColumn))  {
                                let neighborCell = this.grid[neighborRow][neighborColumn];

                                cell.adjacentCells.push(neighborCell);

                                if (neighborCell.isMine) {
                                    cell.adjacentMines++;
                                }
                            }
                        })
                    });
                });       

                return this;
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
