<template>
    <!-- 
        先创建一个容器
    -->
    <div class="total-layout">
        <!--
            显示任务的基本信息，和甘特图一起起作用：
            显示甘特图的每一行所代表的含义
        -->
        <div class="task-information">
            <GanttTaskTable></GanttTaskTable>
        </div>
        <!-- 创建一个甘特图：gantt chart -->
        <div class="chart">
            <!-- 这个是第一行，显示时间轴 -->
            <GanttTimeAxis v-bind:time-axis-length="30"></GanttTimeAxis>
            
            <!-- 开始显示任务在哪些时间段里面执行 -->
            <GanttChartLines v-for="cpuId in cpuNumbers" v-bind:key="cpuId" v-bind:time-axis-length="30" v-bind:cpu-id="cpuId"></GanttChartLines>

        </div>

        <!-- 图例: 显示上述甘特图中的颜色代表任务所处的状态(即阐述颜色的含义) -->
        <div class="legend">
            <GanttStateLegend></GanttStateLegend>
        </div>
    </div>

</template>

<script>
    import GanttChartLines from './GanttChartLines.vue';
    import GanttTimeAxis from './GanttTimeAxis.vue';
    import GanttStateLegend from './GanttStateLegend.vue';
    import GanttTaskTable from './GanttTaskTable.vue';

    // 在组件当中，data 必须为函数
    export default {
        data: function() {
            return {
                cpuNumbers: 4
            }
        },
        // 注册组件
        components: {
            GanttTimeAxis: GanttTimeAxis,
            GanttChartLines: GanttChartLines,
            GanttStateLegend : GanttStateLegend,
            GanttTaskTable : GanttTaskTable
        }
    }
</script>


<!-- 
    甘特图样式表的样式
    添加 scoped 表示
-->
<style scoped>
    * {
        box-sizing: border-box;
    }

    .total-layout {
        display:flex;

        /* 创建一个层叠上下文 */
        z-index: 0; 

    }

    /* chart 的样式 */
    .chart {
        /* 控制溢出 */
        overflow: auto;

        /* 网格布局, 所有子元素将自动成为网格项 */
        display: grid;

        /* 设置外围边框样式 */
            /* 边框大小：border-width: top right bottom left, 按顺时针依次作用 */
        border-width: 2px 0px 0px 2px;

            /* 边框样式 */
        border-style: solid;
        border-color: rgb(208, 211, 212);

        /* 设置组件宽度和高度，其中高度必须设置为 100% ，子组件才不会被迫拉伸到跟父组件一样的高度 */
        width: 764px;
        height: 100%;

        /* 和左边元素的间隔 */
        margin-left: 10px;

        /* 创建层叠上下文：比task-information小 */
        z-index: 2;

    }

    /* 图例样式 */
    .legend {
        width: 300px;

        /* 设置其于左边元素之间的间隔 */
        margin-left: 10px;

        height: 100%;

        display: block;
    }

    .task-information {
        display: block;

        /* 设置其为父组件宽度的 20 % */
        width: 300px;
        /* 高度设置成自理 */
        height: 100%;

        /* 有一个触控条可以来调这些头信息 */
        overflow:auto;

        /* 设置外围边框样式 */
            /* 边框大小：border-width: top right bottom left, 按顺时针依次作用 */
        border-width: 2px 0px 0px 2px;

            /* 边框样式 */
        border-style: solid;
        border-color: rgb(208, 211, 212);

        /* 创建层叠上下文 */
        z-index: 10;
    }
</style>