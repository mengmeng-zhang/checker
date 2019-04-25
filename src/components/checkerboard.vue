<template>
    <div class="checker-board">
        <div class="checker-content">
            <div class="checker">
                <Checker :list="checkerList"/>
            </div>
        </div>
    </div>
</template>
<script>
import Checker from './checker'
import utils from '../utils/utils'
export default {
    components: {
        Checker
    },
    props: {
        x: {
            default: 4,
            type: Number
        },
        y: {
            default: 3,
            type: Number
        }
    },
    data(){
        return {
            checkerList: []
        }
    },
    methods: {
        changeSize(){
            let width = 460 / this.x
            let height = 460 / this.y
            this.checkerList = []
            for(let x = 0; x < this.y; x++){
                for(let y = 0; y < this.x; y++){
                    this.checkerList.push({
                        index: [[x, y], [this.x, this.y]],
                        width: width,
                        height: height,
                        y: -(x * height),
                        x: -(y * width)
                    })
                }
            }
            this.checkerList.pop()
        },
        startGame(){
            this.$eventOne.$on('start', (flag) => {
                if(flag){
                    this.checkerList.sort(function(x, y) {
                        if (x % 2 ==0) return 1;
                        if (x % 2 !=0) return -1;
                    })
                }
            })
        }
    },
    created () {
        this.changeSize()
        // this.startGame()
    },
    watch: {
        x(){
            this.changeSize()
        },
        y(){
            this.changeSize()
        }
    }
}
</script>
<style lang="scss" scoped>
.checker-board{
    height: 100vh;
    width: 75vw;
    display: flex;
    justify-content: center;
    align-items: center;
    .checker-content{
        width: 500px;
        height: 500px;
        border: 8px ridge #ccc;
        background: gray;
        box-sizing: border-box;
        display: inherit;
        align-items: center;
        justify-content: center;
        .checker{
            width: 460px;
            height: 460px;
            border: 8px ridge #ccc;
        }
    }
}
</style>
