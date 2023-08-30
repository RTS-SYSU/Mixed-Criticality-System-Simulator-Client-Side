<template>
    <div class="chart-row-bars" :style="columnNumbers">
        <span class="chart-item" style="width: 120px;">{{ "CPU ID: " + cpuId }}</span>
        <span class="chart-item" v-for="item in timeAxisLength" v-bind:key="item"></span>

        <!-- 
        根据传递进来的数据绘制对应的甘特图（状态）
        -->
        <span v-bind:class="taskInformation.state" v-for="(taskInformation, index) in taskInformations" 
                v-bind:key="'block:' + (taskInformation.startTime + index)" 
                v-bind:style="duration(taskInformation)"> {{ "Task ID: " + taskInformation.dynamicPid }} </span>
    </div>
</template>

<script>
    export default {
        name: "GanttChartLines",

        props: {
            // CPU Id : 表示现在是第几个 CPU 的甘特图
            "cpuId" : {
                type: Number,
                default : 0
            },

            // 时间轴长度
            "timeAxisLength" : {
                type : Number,
                default: 100
            },
            // 传递过来的任务信息
            "taskInformations" : {
                type : Array,
                default: function(){
                    return [
                        {
                            staticPid: 0,
                            dynamicPid: 1,
                            state: "normal-execution",
                            startTime: 5,
                            endTime: 10
                        },
                        {
                            staticPid: 0,
                            dynamicPid: 1,
                            state: "spinning",
                            startTime: 20,
                            endTime: 23
                        },
                        {
                            staticPid: 0,
                            dynamicPid: 1,
                            state: "access-resource",
                            startTime: 24,
                            endTime: 25
                        },
                        {
                            staticPid: 0,
                            dynamicPid: 1,
                            state: "normal-execution",
                            startTime: 26,
                            endTime: 30
                        }
                    ]
                }
            }
        },

        computed: {
            // 时间轴的长度
            columnNumbers: function() {
                return { 
                    'grid-template-columns': '120px ' + 'repeat(' + this.timeAxisLength + ', 80px)'
                }
            }
        },

        methods: {
            // 任务在这一状态下的持续时间
            duration: function(taskInformation) {
                return { 
                        'width': (taskInformation.endTime - taskInformation.startTime + 1) * 80 + 'px',
                        'left' : (80 * (taskInformation.startTime - 1) + 120) + 'px'
                        }
            }
        }
    }
</script>

<style scoped>
    .chart-row-bars {
        /* 定义网格线名称以及网格轨道的尺寸大小 */
        display: grid;

        position: relative;
        /* 指定时间轴的背景颜色 */
        background-color: #fff;
    }

/* 甘特图元素的样式 */
    .chart-item {
        /* 设置字体颜色 */
        color:black;

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

    /* normal-execution: 正常执行时的样式 */
    .normal-execution {
        /*  */
        position: absolute;
        /* 背景颜色为黑色 */
        background-color: rgb(133, 193, 233);
        /* background-color: rgb(0, 0, 0); */

        /* 设置宽度和高度 */
        width: 80px;
        height:30px;

        /* 设置边框四周有角度 */
        border-radius: 40px;

        /* 设置距离顶端 4px */
        top: 5px;

        /* 设置字体大小 */
        font-size: 20px;

        /* 字体居中 */
            /* 字体在水平方向上居中 */
        text-align: center;
            /* 设置成和 div 一样的高度即可使得字体在垂直方向上居中 */
        line-height: 30px;
    }

    /* access-resource: 访问资源（critical section）*/
    .access-resource {
        /*  */
        position: absolute;
        /* 背景颜色 */
        background-color: rgb(130, 224, 170);

        /* 设置宽度和高度 */
        width: 80px;
        height:30px;

        /* 设置边框四周有角度 */
        border-radius: 40px;

        /* 设置距离顶端 4px */
        top: 5px;

        /* 设置字体大小 */
        font-size: 20px;

        /* 字体居中 */
            /* 字体在水平方向上居中 */
        text-align: center;
            /* 设置成和 div 一样的高度即可使得字体在垂直方向上居中 */
        line-height: 30px;
    }

    /* spinning: 自旋等待资源 */
    .spinning {
        position: absolute;
        /* 背景颜色 */
        background-color: rgb(236, 112, 99);

        /* 设置宽度和高度 */
        width: 80px;
        height:30px;

        /* 设置边框四周有角度 */
        border-radius: 40px;

        /* 设置距离顶端 4px */
        top: 5px;

        /* 设置字体大小 */
        font-size: 20px;

        /* 字体居中 */
            /* 字体在水平方向上居中 */
        text-align: center;
            /* 设置成和 div 一样的高度即可使得字体在垂直方向上居中 */
        line-height: 30px;
    }

</style>