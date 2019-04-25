<template>
    <div class="game-bg">
        <Checkerboard :x="Number(x)" :y="Number(y)"/>
        <Settings />
    </div>
</template>
<script>
import Checkerboard from './checkerboard'
import Settings from './settings'
export default {
    components: {
        Checkerboard,
        Settings
    },
    data(){
        return {
            x: 3,
            y: 3
        }
    },
    methods: {
        getCount(){
            this.$eventOne.$on('sendCount', (count) => {
                this.x = count.x
                this.y = count.y
            })
        }
    },
    mounted(){
        this.getCount()
    }
}
</script>
<style lang="scss" scoped>
.game-bg{
    width: 100vw;
    height: 100vh;
    background-color: #000;
    color: #fff;
    display: flex;
    justify-content: center;
    align-items: center;
}
</style>
