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
    import Cell from './Cell.vue';
    import EventBus from './eventBus.js';

    export default {
        data() {
            return {
                rows: 8,
                columns: 8,
                mines: 8,
                grid : [],
                remainingCells : 0
            }
        },

        components: {
            Cell
        },

        created() {
            let self = this;

            EventBus.$on('mineFound', () => {
                self.flipAll();

                EventBus.$emit('gameLost');
            });

            EventBus.$on('newGame', () => {
                self.grid = [];

                self.createGrid().setRandomMines().setAdjacentCells();
            });

            EventBus.$on('flippedCell', (cell) => {
                this.remainingCells--;

                if (cell.isEmpty) {
                    self.flipNeighbours(cell);
                }
            });
        },

        watch: {
            remainingCells(newValue) {
                if (newValue === 0) {
                    console.log('emmiting won');
                    EventBus.$emit('gameWon');
                }
            }
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

                this.remainingCells = this.rows * this.columns - this.mines;

                return this;
            },

            createCell(row, column) {
                return {
                    row,
                    column,
                    isMine : false,
                    isEmpty : false,
                    isLocked : false,
                    adjacentCells : [],
                    adjacentMines:  0,
                    status : 'facedown',
                };
            },

            setRandomMines() {
                let remainingMines = this.mines;

                while (remainingMines > 0) {
                    let randomRow = Math.floor(Math.random() * this.rows);
                    let randomColumn = Math.floor(Math.random() * this.columns);

                    if (this.isCellOnGrid(randomRow, randomColumn)) {
                        let candidateCell = this.grid[randomRow][randomColumn];

                        if (! candidateCell.isMine) {
                            candidateCell.isMine = true;

                            remainingMines--;
                            console.log('setmine', candidateCell);
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

                        cell.isEmpty = cell.isMine === false && cell.adjacentMines === 0;
                    });
                });       

                return this;
            }, 

            flipAll() {
                  this.grid.forEach((row) => { 
                    row.forEach((cell) => {
                        cell.status = "open";
                    });
                });       
            },

            flipNeighbours(cell) {
                cell.isLocked = true;

                cell.adjacentCells.forEach((neighbour) => {
                    if (neighbour.status !== "open") {
                        neighbour.status = "open";

                        this.remainingCells--;
                    }
                });

                cell.adjacentCells.forEach((neighbour) => {
                    if (! neighbour.isLocked && neighbour.isEmpty) {
                        this.flipNeighbours(neighbour);
                    }
                });
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
