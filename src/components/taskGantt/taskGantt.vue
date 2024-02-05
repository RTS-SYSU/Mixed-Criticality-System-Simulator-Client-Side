<template>
    <div class="taskGanttStyle" v-bind:style="calculateHeight()">
        <!-- 左边：任务的基本信息模块 -->
        <taskGanttBasicInformation 
            v-bind:task-gantt-informations="this.taskGanttInformations"
            v-bind:totalItemNum="this.taskGanttInformations.length"
            v-bind:taskPageNow="this.taskPageNow"
            v-bind:maxItemSize="this.maxItemSize"
            v-bind:taskTotalPageSize="this.totalPageSize"/>

        <!-- 右边：任务的甘特图 -->
        <taskGanttChart v-bind:task-gantt-informations="this.taskGanttInformations" 
            v-bind:criticality-switch-time="criticalitySwitchTime"
            v-bind:taskPageNow="this.taskPageNow"
            v-bind:maxItemSize="this.maxItemSize"/>
    </div>
</template>

<script>
    import taskGanttBasicInformation from './taskGanttBasicInformation.vue'
    import taskGanttChart from './taskGanttChart.vue'

    export default {
        name : "taskGantt",

        // 注册组件
        components : {
            taskGanttBasicInformation : taskGanttBasicInformation,
            taskGanttChart : taskGanttChart
        },

        props : {
             // 任务的执行状态以及事件发生信息
            'taskGanttInformations' : {
                type : Array,
                default : function() {
                    return [
                        {
                            // 任务基本信息
                            staticPid : 2,
                            dynamicPid : 1,
                            runningCPUCore : 1,

                            // 任务的执行情况以及事件发生信息
                            taskGanttInformation : 
                            {
                                // 任务的执行情况
                                eventInformations : 
                                [
                                    {
                                        staticPid : 2,
                                        dynamicPid : 1,
                                        state : "normal-execution",
                                        startTime : 2,
                                        endTime : 89
                                    }
                                ],

                                // 甘特图长度
                                timeAxisLength : 89,

                                // 事件发生时间点
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
            },

            // 关键级切换时间点
            'criticalitySwitchTime' : {
                type : Number,
                default : -1
            }
        },

        methods : {
            // 处理器甘特图的高度
            calculateHeight() {
                return {
                    'height' : (Math.min(this.taskGanttInformations.length - this.taskPageNow * this.maxItemSize, this.maxItemSize) * 80 + 40 + 30) + 'px'
                }
            }
        },

        // task 甘特图有多少页
        computed : {
            totalPageSize : function() {
                var ret = Math.ceil(this.taskGanttInformations.length / this.maxItemSize)
                if (this.taskGanttInformations.length % this.maxItemSize == 0) {
                    ret = ret - 1;
                }
                return ret;
            }
        },

        // 用于翻页的数据
        // taskPageNow : 目前处在第几页
        // maxItemSize : 一页可以存放多少数据
        data() {
            return {
                taskPageNow : 0,
                maxItemSize : 7
            }
        },

        // 绑定事件：
        mounted() {
            // 绑定 task 翻页事件：pageNow - 1
            this.$bus.$on('taskPageDown', ()=>{
                this.taskPageNow = Math.max(this.taskPageNow - 1, 0)
            })

            // 绑定 task 翻页事件：pageNow + 1
            this.$bus.$on('taskPageUp', ()=>{
                this.taskPageNow = Math.min(this.taskPageNow + 1, this.totalPageSize - 1)
            })


        }
    }
</script>

<style scoped>
    * {
        box-sizing : border-box
    }

    /* flex 布局 ：cpuGanttBasicInformation 组件和 cpuGanttChart 组件放在同一行  */
    /* 高度大小需要根据传进来的数据进行确定 */
    /* 和上方组件保持 20px 之间的距离 */
    .taskGanttStyle {
        display : flex;
        width : 1400px;
        margin-top: 20px;
        
        /* 设置外围边框样式 */
        /* 边框大小：border-width: top right bottom left, 按顺时针依次作用 */
        border-width: 2px 2px 2px 2px;

        /* 边框样式 */
        border-style: solid;
        border-color: rgb(208, 211, 212);
    }
</style>