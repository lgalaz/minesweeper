<template>
    <div class="cell" 
        :class="classes.join(' ')"
        @contextmenu.prevent="flag"
        @click="flip"
    >
        <template v-if="item.status === statuses.OPEN">
            <template v-if="isEmpty"></template>

            <template v-else-if="item.adjacentMines != 0">
                {{ item.adjacentMines }}
            </template>

            <template v-else-if="item.isMine">
                <span class="is-mine"> * ! </span>
            </template>
        </template>   

        <template v-else-if="item.status === statuses.CHECKED"> ? </template>
    </div>
</template>

<script>
    import EventBus from './eventBus.js';

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
                }
            }
        },

        computed: {
            isEmpty() {
                return ! this.item.isMine && this.item.adjacentMines === 0;
            },

            status() {
                return this.item.status;
            }
        },

        watch: {
            status(newStatus, oldStatus) {
                if (newStatus === this.statuses.CHECKED) {
                    this.classes = [
                        this.statuses.FACEDOWN,
                        this.statuses.CHECKED,
                    ];
                } else if (newStatus === this.statuses.FLAGGED) {
                    this.classes = [
                        this.statuses.FACEDOWN,
                        this.statuses.FLAGGED,
                    ];
                } else if (newStatus === this.statuses.OPEN) {
                    this.classes = [this.statuses.OPEN, 'invalidate'];
                } else {
                    this.classes = [this.statuses.FACEDOWN];
                }
            }
        },

        methods: {
            flip() {
                if ([this.statuses.OPEN, this.statuses.FLAGGED].includes(this.item.status)) {
                    console.log('cant flip');
                    
                    return;
                }

                this.item.status = this.statuses.OPEN;

                if (this.item.isMine) {
                    EventBus.$emit('gameOver');

                    console.log('emmiting');
                } 
            },

            flag() {
                if (this.item.status == this.statuses.OPEN) {
                    return;
                }

                if (this.item.status === this.statuses.FACEDOWN) {
                    this.item.status = this.statuses.CHECKED
                } else if (this.item.status === this.statuses.CHECKED) {
                    this.item.status = this.statuses.FLAGGED
                } else {
                    this.item.status = this.statuses.FACEDOWN;
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
        display: flex;
        align-items: center;
        justify-content: center;
        font-size: 200%;
        font-weight: bolder;
        text-shadow: 2px 2px rgba(52, 58, 64, 0.5);
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
        color: black;
    }

    .is-mine {
        color: red;
    }

    .invalidate {
        pointer-events: none;
    }
</style>
