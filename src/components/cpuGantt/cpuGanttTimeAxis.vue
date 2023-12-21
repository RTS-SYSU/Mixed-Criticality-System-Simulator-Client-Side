<template>
    <div class="cpuGanttTimeAxisLength" :style="columnNumbers">
        <!-- 绘制时间轴 -->
        <span class="chart-item" v-for="(item, index) in timeAxisLength" v-bind:key="index">{{ index }}</span>
    </div>
</template>

<script>
    export default {
        name : "cpuGanttTimeAxis",

        // 父组件传递给子组件的值
        props: {
            "timeAxisLength" : {
                type : Number,
                default: 30
            }
        },

        // 计算属性
        computed: {
            /* 指定网格的列数，每个元素占的格子大小为 50px */
            columnNumbers: function() {
                return { 'grid-template-columns': 'repeat(' + (this.timeAxisLength) + ', 80px)' }
            }
        }
    }
</script>

<style scoped>
    * {
        box-sizing : border-box;
    }
    /* 甘特图的每一行样式 */
    .cpuGanttTimeAxisLength {
        display: grid;

        /* 指定时间轴的背景颜色 */
        background-color: #fff;

        width: 1380px;
        height: 40px;
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