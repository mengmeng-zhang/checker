<template>
    <div class="image-list">
        <h3>点击更换图片</h3>
        <ul>
            <li v-for="(item, index) in imageList" :key="index" @click="changeImage(item)">
                <img :src="item" alt="">
            </li>
        </ul>
    </div>
</template>
<script>
export default {
    data(){
        return {
            imageList: []
        }
    },
    methods: {
        changeImage(url){
            this.$eventOne.$emit('sendImageUrl', url)
        },
        getFile(){
            let files = require.context('../assets/image', false, /.(png|jpg|jpeg|gif|bmp|webp)$/).keys()
            files.forEach(element => {
                this.imageList.push(require('../assets/image' + element.substr(1)))
            });
        }
    },
    mounted(){
        this.getFile()
    }
}
</script>
<style lang="scss" scoped>
.image-list{
    h3{
        margin-bottom: 20px;
        padding-bottom: 5px;
        border-bottom: 1px solid #ccc;
    }
    ul{
        display: flex;
        li{
            width: 50px;
            height: 50px;
            margin-right: 10px;
            border: 1px solid #ccc;
            img{
                width: 100%;
                height: 100%;
            }
        }
    }
}
</style>
