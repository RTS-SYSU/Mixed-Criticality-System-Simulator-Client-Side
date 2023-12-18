<template>
    <div class="taskGanttChart" v-bind:style="calculateHeight()">
        <taskGanttTimeAxis v-bind:timeAxisLength="taskGanttInformations[0].taskGanttInformation.timeAxisLength"/>
        
        <template v-for="(item, index) in this.range(taskPageNow * maxItemSize, Math.min((taskPageNow + 1 ) * maxItemSize, this.totalItemNum))">
            <taskGanttChartLines v-bind:key="index"
                v-bind:criticality-switch-time="criticalitySwitchTime"
                v-bind:time-axis-length="taskGanttInformations[item].taskGanttInformation.timeAxisLength"
                v-bind:event-informations="taskGanttInformations[item].taskGanttInformation.eventInformations"
                v-bind:event-time-points="taskGanttInformations[item].taskGanttInformation.eventTimePoints"/>
        </template>
    </div>
</template>

<script>
    import taskGanttTimeAxis from './taskGanttTimeAxis'
    import taskGanttChartLines from './taskGanttChartLines'

    export default {
        name : 'taskGanttChart',

        components : {
            taskGanttTimeAxis : taskGanttTimeAxis,
            taskGanttChartLines : taskGanttChartLines
        },

        props : {
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
                                        state : "normal-execution",
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
            },
            "criticalitySwitchTime" : {
                type : Number,
                default : -1
            },
            "taskPageNow" : {
                type : Number,
                default : 0
            },
            "maxItemSize" : {
                type : Number,
                default : 7
            }
        },

        methods : {
            calculateHeight() {
                return {
                    'height' : (Math.min(this.taskGanttInformations.length - (this.taskPageNow * this.maxItemSize), this.maxItemSize) * 80 + 40 + 20) + 'px'
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
            totalItemNum : function() {
                return this.taskGanttInformations.length
            }
        }
    }
</script>

<style scoped>
    * {
        box-sizing:border-box;
    }

    /* overflow : auto 表示溢出的话有滚动条 */
    .taskGanttChart {
        width: 1380px;
        overflow: auto;
    }
</style>