<template>
    <div class="cell" 
        :class="classes.join(' ')"
        @contextmenu.prevent="flag"
    >
        <template v-if="status === statuses.OPEN">
            <template v-if="isEmpty"></template>

            <template v-else-if="isMine"> * </template>

            <template v-else-if="adjacentBombs != 0">
                {{ adjacentBombs }}
            </template>
        </template>   

        <template v-else-if="status === statuses.CHECKED">
            ?
        </template>
    </div>
</template>

<script>
    export default {
        data() {
            return {
                isMine: false,
                isEmpty: false, 
                classes: ['facedown'],
                statuses: {
                    FACEDOWN: 'facedown',
                    OPEN : 'open',
                    FLAGGED : 'flagged',
                    CHECKED : 'checked'
                },
                status: 'facedown',
                adjacentCells : [],
                adjacentBombs : 0
            }
        },

        watch: {
            status() {
                this.setClasses(this.status);
            }
        },

        methods: {
            setClasses(status) {
                if (this.status === this.statuses.CHECKED) {
                    this.classes = [
                        this.statuses.FACEDOWN,
                        this.statuses.CHECKED,
                    ];
                } else if (this.status === this.statuses.FLAGGED) {
                    this.classes = [
                        this.statuses.FACEDOWN,
                        this.statuses.FLAGGED,
                    ];
                } else {
                    this.classes = [this.status];
                }
            },

            flip() {
                if ([this.statuses.FACEDOWN, this.statuses.CHECKED].includes(this.status)) {
                    this.open();
                }
            },

            open() {
                if (this.isMine) {
                    this.addClass('is-mine');

                    eventBus.$emit('gameOver');
                } else {
                    this.status = this.status.OPEN;
                }
            },

            flag() {
                if (this.status == this.status.OPEN) {
                    return;
                }

                if (this.status === this.statuses.FACEDOWN) {
                    this.status = this.statuses.CHECKED
                } else if (this.status === this.statuses.CHECKED) {
                    this.status = this.statuses.FLAGGED
                } else {
                    this.status = this.statuses.FACEDOWN;
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
        background-color: lightskyblue;
    }

    .open {
        background-color: lightyellow;
    }

    .flagged {
        background-color: red;
    }

    .checked {
        display: flex;
        align-items: center;
        justify-content: center;
        font-size: 200%;
        font-weight: bolder;
        text-shadow: 2px 2px rgba(52, 58, 64, 0.5);
        color: black;
    }

    .is-mine {
        color: red;
    }
</style>
