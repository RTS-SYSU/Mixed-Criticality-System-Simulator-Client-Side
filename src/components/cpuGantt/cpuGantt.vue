<template>
    <div class="cpuGanttStyle" v-bind:style="calculateHeight()">
        <!-- 左边的信息框 -->
        <cpuGanttBasicInformation 
                v-bind:totalItemNum="cpuGanttInformations.length" 
                v-bind:cpuPageNow="this.cpuPageNow" 
                v-bind:maxItemSize="this.maxItemSize"
                v-bind:cpuTotalPageSize="this.totalPageSize"/>

        <!-- 右边的甘特图信息 -->
        <cpuGanttChart 
                v-bind:criticalitySwitchTime="criticalitySwitchTime"
                v-bind:cpuGanttInformations="cpuGanttInformations"  
                v-bind:cpuPageNow="this.cpuPageNow"
                v-bind:maxItemSize="this.maxItemSize"/>
    </div>
</template>

<script>
    import cpuGanttBasicInformation from './cpuGanttBasicInformation.vue'
    import cpuGanttChart from './cpuGanttChart.vue'

    export default {
        name : 'cpuGantt',

        // 注册组件
        components : {
            cpuGanttBasicInformation : cpuGanttBasicInformation,
            cpuGanttChart : cpuGanttChart
        },

        props : {
            // 处理器执行任务的状态以及事件
            "cpuGanttInformations" : 
            {
                type : Array,
                default : function() {
                    return [
                        {
                            // 执行状态信息
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
            }
        },

        methods : {
            // 根据处理器数量动态改变 cpuGantt 组件的高度
            calculateHeight() {
                return {
                    'height' : (40 + Math.min(this.cpuGanttInformations.length - this.cpuPageNow * this.maxItemSize, this.maxItemSize) * 80 + 30) + 'px'
                }
            }
        },

        // 用于翻页的数据
        // pageNow : 目前处在第几页
        // maxItemSize : 一页可以存放多少数据
        data() {
            return {
                cpuPageNow : 0,
                maxItemSize : 6
            }
        },

        // cpu 甘特图总页数
        computed : {
            totalPageSize : function() {
                var ret = Math.ceil(this.cpuGanttInformations.length / this.maxItemSize)
                if (this.cpuGanttInformations.length % this.maxItemSize == 0) {
                    ret = ret - 1;
                }
                return ret;
            }
        },

        // 绑定事件：
        mounted() {
            // 绑定 cpu 翻页事件：pageNow - 1
            this.$bus.$on('cpuPageDown', ()=>{
                this.cpuPageNow = Math.max(this.cpuPageNow - 1, 0)
            })

            // 绑定 cpu 翻页事件：pageNow + 1
            this.$bus.$on('cpuPageUp', ()=>{
                this.cpuPageNow = Math.min(this.cpuPageNow + 1, this.totalPageSize - 1)
            })


        }
    }
</script>

<style scoped>
    /* flex 布局 ：cpuGanttBasicInformation 组件和 cpuGanttChart 组件放在同一行  */
    /* 高度大小需要根据传进来的数据进行确定 */
    .cpuGanttStyle{
        display: flex;

        width: 1400px;

        /* 设置外围边框样式 */
        /* 边框大小：border-width: top right bottom left, 按顺时针依次作用 */
        border-width: 2px 2px 2px 2px;

        /* 边框样式 */
        border-style: solid;
        border-color: rgb(208, 211, 212);
    }
</style>