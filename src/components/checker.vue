<template>
    <div class="checkers" refs="checker">
        <div :class="`checker-item ${start ? 'start-checker' : ''}`" v-for="(item, index) in list" 
            :key="index"
            :style="checkerStyle(item)"
            @touchstart.stop.prevent="touchstart($event, item.index)"
            @touchmove.stop.prevent="touchmove($event, item.index)"
            @touchend.stop.prevent="touchend($event, item.index)">
        </div>
    </div>
</template>
<script>
import image1 from '../assets/image/bg.jpg'
import { debug } from 'util';
import { resolve } from 'url';
export default {
    props:{
        list: {
            default: [],
            type: Array
        }
    },
    data(){
        return {
            imageUrl: image1,
            start: false,
            position: []
        }
    },
    methods: {
        getimageUrl(){
            this.$eventOne.$on('sendImageUrl', (url) => {
                this.imageUrl = url
            })
        },
        checkerStyle(item){
            return `background: url("${this.imageUrl}") ${item.x}px ${item.y}px / 460px 460px; 
            width: ${item.width}px; height: ${item.height}px;`
        },
        touchstart(event, index){
            console.log(event, index)
        },
        touchmove(event, index){
            console.log(event, index)
        },
        touchend(event, index){
            console.log(event, index)
        },
        startGame(){
            let self = this
            this.$eventOne.$on('start', (flag) => {
                if(flag){
                    self.start = true
                    self.list.map(item => {
                        self.position.push({
                            x: Math.abs(item.x),
                            y: Math.abs(item.y)
                        })
                    })
                    self.position.sort(function(x, y) {
                        if (x % 2 ==0) return 1;
                        if (x % 2 !=0) return -1;
                    })
                    let node = self.$el.children
                    self.position.forEach((item, index) => {
                        node[index].style.top = item.y + 'px'
                        node[index].style.left = item.x + 'px'
                    })
                }
            })
        },
        pos(index){
            let p = this.position[index]
            console.log(p)
            return `top: ${p.y}px; left: ${p.x}px`
        }
    },
    mounted(){
        this.getimageUrl()
        this.startGame()
    },
    watch: {
        list(newValue){
            this.start = false
        }
    }
}
</script>
<style lang="scss" scoped>
.checkers{
    display: flex;
    flex-wrap: wrap;
    position: relative;
}
.checker-item{
    border:1px solid #ccc;
    box-sizing: border-box;
    cursor: pointer;
}
.start-checker{
    position: absolute;
}
</style>
