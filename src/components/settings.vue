<template>
    <div class="settings">
        <SelectImage />
        <ResetSize />
        <Game />
    </div>
</template>
<script>
import SelectImage from './selectImage'
import ResetSize from './resetSize'
import Game from './game'
export default {
    components: {
        SelectImage,
        ResetSize,
        Game
    }
}
</script>
<style lang="scss" scoped>
.settings{
    height: 100vh;
    width: 25vw;
    padding: 50px 20px;
    box-sizing: border-box;
}
</style>
