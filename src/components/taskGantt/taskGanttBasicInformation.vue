<template>
    <div v-bind="WidthAndHeight()">
        <!-- 前三个空白格 -->
        <div class="chart-item" style="width: 140px; height: 40px; line-height:40px; display: inline-block;"> Task id </div>
        <div class="chart-item" style="width: 140px; height: 40px; line-height:40px; display: inline-block;"> Job id </div>
        <div class="chart-item" style="width: 160px; height: 40px; line-height:40px; display: inline-block;"> Allocation </div>

        <!-- static pid | dynamic pid | running cpu core -->
        <template v-for="(item, index) in this.range(taskPageNow * maxItemSize, Math.min((taskPageNow + 1) * maxItemSize, totalItemNum))">
            <div style="width: 440px; height: 80px;" v-bind:key="index">
                <div class="chart-item" style="width: 140px; height: 80px; display: inline-block;"> {{taskGanttInformations[item].staticPid}} </div>
                <div class="chart-item" style="width: 140px; height: 80px; display: inline-block;"> {{taskGanttInformations[item].dynamicPid}} </div>
                <div class="chart-item" style="width: 160px; height: 80px; display: inline-block;"> {{taskGanttInformations[item].runningCPUCore}} </div>
            </div>
        </template>

        <!-- 任务甘特图翻页 -->
        <taskPageFlip
                v-if="taskTotalPageSize > 1"
                v-bind:taskPageNow="taskPageNow"
                v-bind:taskTotalPageSize="taskTotalPageSize"/>
    </div>
</template>

<script>
    import taskPageFlip from './taskPageFlip.vue'
    export default {
        name : 'taskGanttBasicInformation',

        components : {
            taskPageFlip : taskPageFlip
        },

        props : {
            "totalItemNum" : {
                type : Number,
                default : 1
            },

            "taskPageNow" : {
                type : Number,
                default : 1
            },

            "maxItemSize" : {
                type : Number,
                default : 7
            },

            "taskTotalPageSize" : {
                type : Number,
                default : 1
            },

            "taskGanttInformations" : {
                type : Array,
                default : function() {
                    return [
                        {
                            staticPid : 2,
                            dynamicPid : 1,
                            runningCPUCore : 1,
                            taskGanttInformation : 
                            {
                                eventInformations : 
                                [
                                    {
                                        staticPid : 2,
                                        dynamicPid : 1,
                                        event : "normal-execution",
                                        startTime : 2,
                                        endTime : 89
                                    }
                                ],
                                timeAxisLength : 89,
                                eventTimePoints : 
                                [
                                    {
                                        staticPid : 2,
                                        dynamicPid : 1,
                                        event : "release",
                                        eventTime : 1,
                                        resourceId : -1
                                    },
                                    {
                                        staticPid : 2,
                                        dynamicPid : 1,
                                        event : "completion",
                                        eventTime : "89",
                                        resourceId : -1
                                    }
                                ]
                            }
                        }
                    ]
                }
            }
        },

        methods :{
            WidthAndHeight() {
                return {
                    'width' : 140 + 140 + 160 + 'px',
                    'height' : (40 + this.taskGanttInformations.length * 80) + 'px'
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

        height: 80px;
    }
</style>