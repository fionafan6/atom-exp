<template>
    <div class="entry-container" :style="`background-image: url('${this.address}')`">
        <div class="experiment-info">
            <div class="go-test-group">
                <!-- 实验展示图片 -->
                <div v-if="showEnterButton" class="goTest" type="is-primary" @click="startTest">继续</div>
            </div>
        </div>
    </div>
</template>
<script>
export default {
    data() {
        return {
            userInfo: {},
            showEnterButton: false,
            address: 'http://10.12.207.24:1080/Pictures/Fiona-atom-v2/instructions.jpg',
            isfirst: true,
            issecond: false
        }
    },
    methods: {
        startTest() {
            this.$router.push({
                name: 'UserInfo',
            }) 
        },

    },
    mounted() {


            setTimeout(() => {
                this.showEnterButton = true
            },3000)
        
    }
}
</script>
<style lang="less" scoped>
.entry-container {
    width: 100vw;
    height: 100vh;
    box-sizing: border-box;
    padding: 4rem 5rem;
    background-size: 100% 100%;
    text-align: center;
    .goTest {
        padding: 0px;
        border-radius: 1px;
        position: absolute;
        right: 0px;
        bottom: 0px;
        transform: translate(-50%);
        background-color: rgb(127,127,127);
        border-color: rgb(0, 0, 0);
        color: rgb(0, 0, 0);
        font-size: 0.5cm;
    }
}
</style>
