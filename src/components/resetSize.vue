<template>
    <div class="reset-size">
        <h3>请输入要显示的格数:(2-5的数字)</h3>
        <div class="count">
            <label for="w">列数：</label>
            <input type="number" v-model="x" name="w" id="w">
        </div>
        <div class="count">
            <label for="h">行数：</label>
            <input type="number" v-model="y" name="h" id="h">
        </div>
        <div>
            <button type="button" @click="sendNumber">确认</button>
            <button type="button" @click="reset">重置</button>
        </div>
    </div>
</template>
<script>
export default {
    data(){
        return {
            x: 3,
            y: 3
        }
    },
    methods: {

        sendNumber(){
            this.$eventOne.$emit('sendCount', {
                x: this.x,
                y: this.y
            })
        },
        reset(){
            this.x = 3
            this.y = 3
            this.$eventOne.$emit('sendCount', {
                x: 3,
                y: 3
            })
        }
    }
}
</script>
<style lang="scss" scoped>
.reset-size{
    padding: 20px 0;
    h3{
        margin-bottom: 20px;
        padding-bottom: 5px;
        border-bottom: 1px solid #ccc;
    }
    .count{
        label{
            display: flex;
        }
    }
}
</style>
