<template>
    <div v-bind="WidthAndHeight()">
        <!-- 第一个空白格 -->
        <div class="chart-item" style="height: 40px;"></div>
        <!-- CPU ID :  -->
        <div class="chart-item" v-for="(item, index) in this.range(cpuPageNow * maxItemSize, Math.min((cpuPageNow + 1) * maxItemSize, totalItemNum))" v-bind:key="index">{{ "CPU ID : " + item }}</div>
        <!-- cpuPageFlip : 翻页功能 -->
        <cpuPageFlip v-if="cpuTotalPageSize > 1" v-bind:cpuPageNow="cpuPageNow" v-bind:cpuTotalPageSize="cpuTotalPageSize"/>
    </div>
</template>

<script>
    import cpuPageFlip from './cpuPageFlip.vue'
    export default {
        name : "cpuGanttBasicInformation",

        components : {
            cpuPageFlip : cpuPageFlip
        },

        props : {
            // 处理器个数
            "totalItemNum" : {
                type : Number,
                default : 1
            },

            // 当前显示页数
            "cpuPageNow" : {
                type : Number,
                default : 0
            },

            // 一次最多显示的处理器甘特图个数
            "maxItemSize" : {
                type : Number,
                default : 7
            },

            // 总页数
            "cpuTotalPageSize" : {
                type : Number,
                default : 1
            }
        },

        methods : {
            // 处理器甘特图的宽度和高度
            WidthAndHeight() {
                return {
                    'width' : '120px',
                    'height' : (this.cpuNum * 80 + 40) + 'px'
                }
            },

            range(start, end) {
                var arr = []
                for (let i = start; i < end; ++i) {
                    arr.push(i);
                }

                return arr;
            }
        }
    }
</script>

<style scoped>
    * {
        box-sizing: border-box;
    }

    /* 元素的样式 */
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
        line-height: 80px;

        /* 设置每一行的之间的边框样式 */
            /* border-width: top right bottom left, 按顺时针依次作用 */
        border-width: 0px 2px 2px 0px; 
            /* border-style: 线框的样式 */
        border-style: solid;
            /* 线框的颜色 */
        border-color: rgb(208, 211, 212); 

        width: 120px;
        height: 80px;
    }
</style>