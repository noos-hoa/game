<template>
    <article>
        <div class="grid">
            <div
                class="grid-row"
                v-for="(row, rowIndex) in rows"
                :key="`row-${rowIndex}`"
            >
                <div
                    class="grid-cell"
                    :class="cellClass(rowIndex, cellIndex)"
                    :key="`cell-${rowIndex}-${cellIndex}`"
                    v-for="(cell, cellIndex) in cells"
                    @click="onCellClick(rowIndex, cellIndex)"
                ></div>
            </div>
        </div>
        <footer class="row pt-2">
            <div class="col-4">
                <label for="time">Time</label>
                <input class="form-control" id="time" type="text" v-model="time">
            </div>
            <div class="col-4">
                Gamer: {{scores.gamer}} / PC: {{scores.pc}}
            </div>
            <div class="col-4 text-right">
                <button
                    class="btn btn-primary"
                    @click="onStartClick"
                >
                    Start
                </button>
            </div>
        </footer>
        <PopUp
            :is-show-modal="isShowModal"
            :modal-text="modalText"
            @closePopUp="onCloseModal"
        />
    </article>
</template>

<script>
    import { random } from 'lodash';
    import PopUp from './PopUp';


    export default {
        name: "Game",
        components: {
            PopUp
        },
        data() {
            return {
                time: 100, // ms,
                highlight: {
                    row: -1,
                    cell: -1
                },
                scores: {
                    gamer:0,
                    pc: 0
                },
                misses: [],
                hits: [],
                wasShows: [],
                modalText: '',
                isShowModal: false,
            }
        },
        props: {
            rows: {
                type: Number,
                default: 10
            },
            cells: {
                type: Number,
                default: 10
            },
            maxScore: {
                type: Number,
                default: 10
            }
        },
        computed: {
            isHighlight() {
                return (row, cell) => {
                    return this.highlight.row === row && this.highlight.cell === cell;
                }
            },
            cellClass(){
                return (row,cell) => {
                    const key = `${row},${cell}`;
                    if(this.isHighlight(row,cell)){
                        return 'bg-warning';
                    }
                    if(this.hits.includes(key)){
                        return  'bg-success'
                    }
                    if(this.misses.includes(key)){
                        return  'bg-danger'
                    }
                    return 'bg-primary'
                }
            }
        },
        methods: {
            onCellClick(row, cell) {
                if(this.isHighlight(row, cell)){
                    this.scores.gamer++;
                    this.hits.push(`${row},${cell}`);
                    this.runStep();
                }
            },
            onStartClick() {
                this.resetData();
                this.runStep();
            },
            onCloseModal(){
                this.resetData();
                this.isShowModal = false;
            },
            runStep(){
                if(this.stepTimeout) clearTimeout(this.stepTimeout);
                if(this.scores.gamer < this.maxScore && this.scores.pc < this.maxScore){
                    this.highlight = this.getNewPosition();
                    this.wasShows.push(`${this.highlight.row},${this.highlight.cell}`);

                    this.stepTimeout = setTimeout(()=>{
                        this.scores.pc++;
                        this.misses.push(`${this.highlight.row},${this.highlight.cell}`);
                        this.runStep();
                    }, this.time);
                } else {
                    this.highlight.row = -1;
                    this.highlight.cell = -1;
                    this.showResults();
                }
            },
            getNewPosition(){
                const position = {
                    row: random(0,9),
                    cell: random(0,9)
                };
                if(this.wasShows.includes(`${position.row},${position.cell}`)){
                    return this.getNewPosition();
                } else {
                    return position;
                }
            },
            showResults(){
                if(this.scores.gamer > this.scores.pc){
                    this.modalText = 'Congratulations, You are win';
                } else {
                    this.modalText = 'Sorry, But you are lose';
                }
                this.isShowModal = true;

            },
            resetData(){
                this.scores.pc = 0;
                this.scores.gamer = 0;
                this.hits = [];
                this.misses = [];
                this.wasShows = [];
            }

        }
    }
</script>

<style scoped lang="scss">
    footer {
        display: flex;

        > div {
            padding: 0 10px;
        }
    }

    .grid {
        &-row {
            display: flex;

        }

        &-cell {
            height: 80px;
            border: 1px solid #fff;
            flex-grow: 1;
            cursor: pointer;
        }
    }

</style>