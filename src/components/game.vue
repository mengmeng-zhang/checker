<template>
    <div>
        <button type="button" @click="resetGame">重置游戏</button>
        <button type="button" @click="start">开始游戏</button>
    </div>
</template>
<script>
export default {
    methods: {
        resetGame(){
            window.location.reload()
        },
        start(){
            this.$eventOne.$emit('start', true)
        }
    }
}
</script>
<style lang="scss" scoped>

</style>
