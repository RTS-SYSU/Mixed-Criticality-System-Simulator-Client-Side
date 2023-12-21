<template>
    <!-- 甘特图时间轴 -->
    <div class="chart-row" :style="columnNumbers">
        <span class="chart-item" style="width: 120px;"></span>
        <!-- 创建时间项（即横坐标）-->
        <span class="chart-item" v-for="item in timeAxisLength" v-bind:key="item">{{ item }}</span>
    </div>
</template>

<script>
    export default {
        name : "GanttTimeAxis",

        // 父组件传递给子组件的值
        props: {
            "timeAxisLength" : {
                type : Number,
                default: 100
            }
        },

        // 计算属性
        computed: {
            /* 指定网格的列数，每个元素占的格子大小为 50px */
            columnNumbers: function() {
                return { 'grid-template-columns': '120px ' + 'repeat(' + this.timeAxisLength + ', 80px)' }
            }
        }
    }
</script>

<style scoped>
    /* 甘特图的每一行样式 */
    .chart-row {
        /* 定义网格线名称以及网格轨道的尺寸大小 */
        display: grid;

        /* 指定时间轴的背景颜色 */
        background-color: #fff;
    }

    /* 甘特图元素的样式 */
    .chart-item {
        /* 设置颜色 */
        color:rgb(208, 211, 212);

        /* 字体大小 */
        font-size: 20px;

        /* 字体加粗的效果 */
        font-weight: bold;

        /* 字体居中 */
            /* 字体在水平方向上居中 */
        text-align: center;
            /* 设置成和 div 一样的高度即可使得字体在垂直方向上居中 */
        line-height: 40px;

        /* 设置每一行的之间的边框样式 */
            /* border-width: top right bottom left, 按顺时针依次作用 */
        border-width: 0px 2px 2px 0px; 
            /* border-style: 线框的样式 */
        border-style: solid;
            /* 线框的颜色 */
        border-color: rgb(208, 211, 212); 

        width: 80px;
        height: 40px;
    }
</style>