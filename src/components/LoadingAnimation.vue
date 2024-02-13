<template>
    <div class="loader-container" v-bind:style="{'display' : 'none'}">
        <svg class="spinner" width="120px" height="120px" viewBox="0 0 120 120">
            <circle class="path" cx="60" cy="60" r="35" fill="none" stroke-width="5"></circle>
        </svg>
        <span class="span-style"> 正在模拟... </span>
    </div>
</template>

<script>
    export default {
        name : "LoadingAnimation",

        mounted() {
            // 
            this.$bus.$on('ShowLoadingAnimation', ()=>{
                this.$el.style.display = 'block'
            })

            this.$bus.$on('CloseLoadingAnimation', ()=>{
                this.$el.style.display = 'none'
            })
        }
    }
</script>

<style>
    .loader-container {
        display: block;
        position: absolute;
        left: 1206px;
        top: 300px;
    }

    .span-style {
        display: block;
        width: 120px;
        height: 40px;
        text-align: center;
        line-height: 40px;
        /* 字体加粗 */
        font-weight: bold;

        /* 调整字号 */
        font-size : 22px;

        /* 字体颜色 */
        color : rgb(110, 110, 110);
    }


    .spinner {
        display: block;
        animation: spin 1s linear infinite;
    }

    @keyframes spin {
        0% { transform: rotate(0deg); }
        100% { transform: rotate(360deg); }
    }

    .path {
        stroke: #565656; /* 加载器颜色 */
        stroke-linecap: round; /* 圆形的线帽使得端点更圆润 */
    }
</style>
