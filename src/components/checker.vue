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
            target: '',
            targetWidth: 0,
            targetHeight: 0,
            moveY: 0,
            currTarget: ''
        }
    },
    methods: {
        getimageUrl(){
            this.$eventOne.$on('sendImageUrl', (url) => {
                this.imageUrl = url
            })
        },
        checkerStyle(item){
            this.targetHeight = item.height
            this.targetWidth = item.width
            return `background: url("${this.imageUrl}") ${item.x}px ${item.y}px / 460px 460px;
            width: ${item.width}px; height: ${item.height}px;`
        },
        touchstart(event, index){
          this.target = event.target
          this.iTouchY = event.changedTouches[0].clientY
          this.iSlideL = this.target.offsetTop
          console.log(this.iSlideL)
        },
        touchmove(event, index){
            let x = this.target.style.height
            let top = this.target.style.top
            this.moveY = event.changedTouches[0].clientY - this.iTouchY
            let y = event.changedTouches[0].clientY - this.iTouchY + this.iSlideL
            
            if(this.moveY > 0 && this.iSlideL > this.targetHeight * 2){
                //   鼠标向下移动，offsetTop 大于自身高度的两倍
                this.target.style.top = (this.targetHeight * 2) + 'px'
            }else if(this.moveY < 0 && this.iSlideL < this.targetHeight){
                //   鼠标向上移动，offsetTop 小于自身高度
                this.target.style.top = this.targetHeight + 'px'
            }else{
                this.target.style.top = (this.moveY + this.iSlideL) + 'px'
            }
        },
        touchend(event, index){
            if(this.moveY > 0 && this.iSlideL > this.targetHeight * 2){
                //   鼠标向下移动，offsetTop 大于自身高度的两倍
                this.target.style.top = (this.targetHeight * 2) + 'px'
            }else if(this.moveY < 0 && this.iSlideL < this.targetHeight){
                //   鼠标向上移动，offsetTop 小于自身高度
                this.target.style.top = this.targetHeight + 'px'
            }else{
                this.target.style.top = (this.moveY + this.iSlideL) + 'px'
            }
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
    transition: all 0.1s linear;
}
.start-checker{
    position: absolute;
}
</style>
