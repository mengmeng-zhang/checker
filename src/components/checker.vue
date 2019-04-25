<template>
    <div class="checkers" ref="checker">
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
            position: [],
            iTouchX: 0,
            iSlideL: 0,
            target: ''
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
          this.target = event.target
          this.iTouchX = event.changedTouches[0].clientY
          this.iSlideL = this.target.offsetTop
        },
        touchmove(event, index){
          let target = event.target
          let x = target.style.height
          let top = target.style.top
          // console.log(this.iTouchX, this.iSlideL)
          // console.log(Number(top.substr(0, top.length - 2)))
          let y = event.changedTouches[0].clientY - this.iTouchX + this.iSlideL
          if(y < this.iSlideL){
            target.style.top = this.iSlideL + 'px'
          }else{
            target.style.top = y + 'px'
          }
          // console.log(event.changedTouches[0].clientY - this.iTouchX)
          // target.style.top = event.changedTouches[0].clientY - this.iTouchX + this.iSlideL + 'px'
        },
        touchend(event, index){
            console.log(event, index)
        },
        startGame(){
            let self = this
            this.$eventOne.$on('startGame', (flag) => {
              if(self.start){
                return
              }
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
                  let node = self.$refs.checker.children
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
    transition: all 0.5s;
}
.start-checker{
    position: absolute;
}
</style>
