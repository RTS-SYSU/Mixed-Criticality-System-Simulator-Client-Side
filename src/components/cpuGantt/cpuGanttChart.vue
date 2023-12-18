<template>
    <div class="cpuGanttChartStyle" v-bind:style="calculateHeight()">
        <!-- 
            1. cpuGanttTimeAxis 组件：呈现处理器甘特图的时间轴
            cpuGanttTimeAxis 组件需要以下数据：
                1. timeAxisLength:处理器甘特图的时间轴长度
         -->
        <cpuGanttTimeAxis v-bind:time-axis-length="this.cpuGanttInformations[0].timeAxisLength"></cpuGanttTimeAxis>

        <!-- 
            2. cpuGanttChartLines 组件：绘制出处理器对应的甘特图
            cpuGanttChartLines 组件需要以下数据：
                1. criticalitySwitchTime : 关键级切换的时间点
                2. timeAxisLength : 处理器甘特图的长度
                3. eventInformations : 在处理器上运行的任务的运行状态信息
                4. eventTimePoints : 在处理器上运行的任务触发的事件
         -->
        <template v-for="(item, index) in this.range(cpuPageNow * maxItemSize, Math.min((cpuPageNow + 1 ) * maxItemSize, this.totalItemNum))">
            <cpuGanttChartLines v-bind:key="index"
                v-bind:criticality-switch-time="criticalitySwitchTime"
                v-bind:time-axis-length="cpuGanttInformations[item].timeAxisLength"
                v-bind:event-informations="cpuGanttInformations[item].eventInformations"
                v-bind:event-time-points="cpuGanttInformations[item].eventTimePoints"></cpuGanttChartLines>
        </template>
    </div>
</template>

<script>
    import cpuGanttTimeAxis from './cpuGanttTimeAxis.vue'
    import cpuGanttChartLines from './cpuGanttChartLines.vue'

    export default {
        name : 'cpuGanttChart',

        components : {
            cpuGanttChartLines : cpuGanttChartLines,
            cpuGanttTimeAxis : cpuGanttTimeAxis
        },

        props : {
            // 处理器执行任务的状态以及事件
            "cpuGanttInformations" : {
                type : Array,
                default : function() {
                    return [
                        {
                            // 在处理器上运行的任务的运行状态信息
                            eventInformations : [
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
                            ],

                            // 事件发生的时间点
                            eventTimePoints : [
                                {
                                    staticPid: 0,
                                    dynamicPid: 1,
                                    event: "release",
                                    eventTime: 5
                                },
                                {
                                    staticPid: 0,
                                    dynamicPid: 1,
                                    event: "lock-attempt",
                                    eventTime: 20
                                },
                                {
                                    staticPid: 0,
                                    dynamicPid: 1,
                                    event: "locked",
                                    eventTime: 24
                                },
                                {
                                    staticPid: 0,
                                    dynamicPid: 1,
                                    event: "unlocked",
                                    eventTime: 25
                                },
                                {
                                    staticPid: 0,
                                    dynamicPid: 1,
                                    event: "completion",
                                    eventTime: 30
                                }
                            ],
                            
                            // 甘特图长度
                            timeAxisLength : 30
                        }
                    ]
                }
            },
            // 关键级发生切换的时间点
            "criticalitySwitchTime" : {
                type : Number,
                default : -1
            },

            // 甘特图表格当前所在页数
            "cpuPageNow" : {
                type : Number,
                default : 0
            },

            // 一页最多显示处理器甘特图的数量
            "maxItemSize" : {
                type : Number,
                default : 7
            }
        },

        methods : {
            // 根据处理器数量动态改变 cpuGantt 组件的高度
            calculateHeight() {
                return {
                    'height' : (Math.min(this.cpuGanttInformations.length - (this.cpuPageNow * this.maxItemSize), this.maxItemSize) * 80 + 40 + 20) + 'px'
                }
            },

            range(start, end) {
                var arr = []
                for (let i = start; i < end; ++i) {
                    arr.push(i);
                }
                return arr;
            }
        },

        computed : {
            // 处理器甘特图的数量
            totalItemNum : function() {
                return this.cpuGanttInformations.length
            }
        }


    }
</script>

<style scoped>
    * {
        box-sizing: border-box;
    }

    /* overflow : auto 表示溢出的话有滚动条 */
    .cpuGanttChartStyle {
        width: 1380px;
        overflow: auto;
    }

</style>