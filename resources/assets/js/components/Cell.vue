<template>
    <div class="cell" :class="classes">
        <template v-if="isEmpty"></template>
        <template v-else-if="isMine"> * </template>
        <template v-else>
            <template v-if="status !== 'facedown'">
                {{ adjacentBombs }}
            </template>
        </template>
    </div>
</template>

<script>
    export default {
        data() {
            return {
                isMine: false,
                isEmpty: false, 
                classes: 'facedown',
                statuses: {
                    FACEDOWN: 'facedown',
                    OPEN : 'open',
                    FLAGGED : 'flagged',
                    CHECKED : 'check'
                },
                status: 'facedown',
                adjacentCells : [],
                adjacentBombs : 0
            }
        },

        watch: {
            status() {
                this.classes = this.status;
            }
        },

        methods: {
            flip() {
                if ([this.statuses.FACEDOWN, this.statuses.CHECKED].includes(this.status)) {
                    this.open();
                }
            },

            open() {
                if (this.isMine) {
                    this.classes = 'is-mine';

                    eventBus.$emit('gameOver');
                } else {
                    this.status = this.status.OPEN;
                }
            }
        }
    }
</script>

<style>
    .cell {
        width: 50px;
        height: 50px;
        margin: 2px;
    }

    .facedown {
        background-color: darkslategray;
    }

    .open {
        background-color: lightyellow;
    }

    .flagged, .is-mine {
        color: red;
    }

    .checked {
        color: black;
    }
</style>
