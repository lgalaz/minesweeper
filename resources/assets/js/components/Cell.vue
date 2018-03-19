<template>
    <div class="cell" 
        :class="classes.join(' ')"
        @contextmenu.prevent="flag"
        @click="flip"
    >
        <template v-if="status === statuses.OPEN">
            <template v-if="this.isEmpty"></template>

            <template v-else-if="this.item.isMine"> * </template>

            <template v-else-if="this.item.adjacentMines != 0">
                {{ this.item.adjacentMines }}
            </template>
        </template>   

        <template v-else-if="status === statuses.CHECKED">
            ?
        </template>
    </div>
</template>

<script>
    export default {
        props : {
            item: {
                type: Object,
                required: true
            },
        },

        data() {
            return {
                classes: ['facedown'],
                statuses: {
                    FACEDOWN: 'facedown',
                    OPEN : 'open',
                    FLAGGED : 'flagged',
                    CHECKED : 'checked'
                },
                status: this.item.status,
            }
        },

        computed: {
            isEmpty() {
                return ! this.item.isMine && this.item.adjacentMines === 0;
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
                } else if (this.status === this.statuses.OPEN) {
                    this.classes = [this.status, 'invalidate'];
                } else {
                    this.classes = [this.status];
                }
            },

            flip() {
                if ([this.statuses.OPEN, this.statuses.FLAGGED].includes(this.status)) {
                    console.log('cant flip');
                    
                    return;
                }

                if (this.item.isMine) {
                    eventBus.$emit('gameOver');
                } else {
                    this.status = this.statuses.OPEN;
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

    .invalidate {
        pointer-events: none;
    }
</style>
