<template>
    <!-- 为每一个处理器绘制对应的甘特图 -->
    <div class="cpuGanttChartLinesStyle" v-bind:style="columnNumbers">
        <!-- 先将时间刻度显示出来 -->
        <span class="chart-item" v-for="(item, index) in timeAxisLength" v-bind:key="index"></span>

        <!-- 显示当前正在执行的任务的 id  -->
        <template v-for="(eventInformation, index) in eventInformations">
            <span class="taskId-style"
                v-if="eventInformation.staticPid != -1"
                v-bind:key="'span: ' + (index)"
                v-bind:style="duration(eventInformation)">
                {{ "Task " + eventInformation.dynamicPid }}
            </span>
        </template>

        <!-- 显示任务的状态 -->
         <div  v-for="(eventInformation, index) in eventInformations" 
                v-bind:class="eventInformation.state + '-block'"
                v-bind:key="'block:' + (eventInformation.startTime) + index" 
                v-bind:style="duration(eventInformation)">
        </div>

        <!-- 在某些时间点上的操作（请求资源、释放资源、请求并等待资源）-->
         <template v-for="(eventTimePoint, index) in eventTimePoints">

            <!-- locked：获取到一个资源 -->
            <svg class="symbol-style" v-if="eventTimePoint.event == 'locked'" v-bind:style="symbolPosition(eventTimePoint)" v-bind:key="'locked' + index">
                    <!-- 绘制圆圈 -->
                <circle cx="20" cy="20" r="10" stroke="black" stroke-width="3" fill="black"/>
                <!-- 绘制直线 -->
                <line x1="20" y1="30" x2="20" y2="80" stroke="black" stroke-width="3" fill="none"/>
            </svg>

            <!-- unlocked：释放一个资源 -->
            <svg class="symbol-style" v-if="eventTimePoint.event == 'unlocked'" v-bind:style="symbolPosition(eventTimePoint)" v-bind:key="'unlocked' + index">
                <!-- 绘制圆圈 -->
                <circle cx="20" cy="20" r="10" stroke="black" stroke-width="3" fill="none"/>
                <!-- 绘制直线 -->
                <line x1="20" y1="30" x2="20" y2="80" stroke="black" stroke-width="3" fill="none"/>
            </svg>

            <!-- lock attempt: 尝试去请求资源，但是资源已经被其他进程获得 -->
            <svg class="symbol-style" v-if="eventTimePoint.event == 'lock-attempt'" v-bind:style="symbolPosition(eventTimePoint)" v-bind:key="'lock-attempt' + index">
                <!-- 左边黑色填充的半圆 -->
                <path d="M 20 30 A 10 10 0 1 1 20 10z" stroke="black" stroke-width="3" fill="black"/>
                <!-- 右边白色无填充的半圆 -->
                <path d="M 20 10 A 10 10 0 1 1 20 30" stroke="black" stroke-width="3" fill="none"/>
                <!-- 直线 -->
                <line x1="20" y1="30" x2="20" y2="80" stroke="black" stroke-width="3" fill="none"/>
            </svg>

            <!-- release: 发布一个任务 -->
            <svg class="symbol-style" v-if="eventTimePoint.event == 'release'" v-bind:style="symbolPosition(eventTimePoint)" v-bind:key="'lock-attempt' + index">
                <!-- 绘制直线 -->
                <line x1="20" y1="30" x2="20" y2="80" stroke="black" stroke-width="3" fill="none"/>
                <!-- 绘制三角形 -->
                <path d="M 20 10 L 10 30 L 30 30 z" stroke="black" stroke-width="3" fill="black"/>
            </svg>

            <!-- completion: 释放一个资源 -->
            <svg class="symbol-style" v-if="eventTimePoint.event == 'completion'" v-bind:style="symbolPosition(eventTimePoint)" v-bind:key="'lock-attempt' + index">
                <!-- 绘制直线(竖线) -->
                <line x1="20" y1="20" x2="20" y2="80" stroke="black" stroke-width="3" fill="none"/>
                <!-- 绘制直线（横线） -->
                <line  x1="10" y1="20" x2="30" y2="20" stroke="black" stroke-width="3" fill="none" />
            </svg>

            <!-- switch task: 切换任务 -->
            <svg class="symbol-style" v-if="eventTimePoint.event == 'switch-task'" v-bind:style="symbolPosition(eventTimePoint)" v-bind:key="'switch-task' + index">
                <line x1="20" y1="0" x2="20" y2="80" stroke="rgb(0, 185, 255)" stroke-width="3"></line>
            </svg>   
            
            <!-- shut-down: 关闭任务 -->
            <svg class="symbol-style" v-if="eventTimePoint.event == 'killed'" v-bind:style="symbolPosition(eventTimePoint)" v-bind:key="'killed' + index">
                <line x1="20" y1="20" x2="20" y2="80" stroke="rgb(255, 208, 0)" stroke-width="4"></line>
                <line x1="10" y1="10" x2="30" y2="30" stroke="rgb(255, 208, 0)" stroke-width="4"></line>
                <line x1="10" y1="30" x2="30" y2="10" stroke="rgb(255, 208, 0)" stroke-width="4"></line>
            </svg>
        </template>

        <!-- criticality-switch: 系统关键级发生变化 -->
        <svg class="symbol-style" v-if="criticalitySwitchTime != -1 " v-bind:style="{'left' : (criticalitySwitchTime * 80 - 21) + 'px'}">
            <circle cx="20" cy="40" r="10" stroke="red" stroke-width="4" fill="red"></circle>
        </svg>
    </div>
</template>

<script>
    export default {
        name: "cpuGanttChartLines",

        props: {

            "criticalitySwitchTime" : {
                type : Number,
                default : -1
            },
            // 时间轴长度
            "timeAxisLength" : {
                type : Number,
                default: 30
            },
            // 传递过来的任务信息
            "eventInformations" : {
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
            },
            // 事件发生时间点
            "eventTimePoints" : {
                type : Array,
                default: function(){
                    return [
                        {
                            staticPid: 0,
                            dynamicPid: 1,
                            event: "release",
                            eventTime: 5
                        },
                        {
                            staticPid: 0,
                            dynamicPid: 1,
                            event: "switch-task",
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
                        },
                        {
                            staticPid: 0,
                            dynamicPid: 1,
                            event: "killed",
                            eventTime: 27
                        },
                        {
                            staticPid: 0,
                            dynamicPid: 1,
                            event: "criticality-switch",
                            eventTime: 29
                        }
                    ]
                }
            }
        },

        computed: {
            columnNumbers: function() {
                // 1. 列的个数以及宽度
                // 2. 控制溢出
                return {
                    'grid-template-columns' : 'repeat(' + this.timeAxisLength + ', 80px)'
                }
            }
        },

        methods: {
            // 任务在这一状态下的持续时间
            duration: function(eventInformation) {
                return { 
                        'width': (eventInformation.endTime - eventInformation.startTime) * 80 + 'px',
                        'left' : 80 * eventInformation.startTime + 'px'
                        }
            },

            // 符号应该所处的位置
            symbolPosition: function(eventTimePoint) {
                // 符号需要显示在时间点的左边
                return {
                        'left' : (eventTimePoint.eventTime * 80 - 21) + 'px'
                    }
            }
        }
    }
</script>

<style scoped>
    * {
        box-sizing : border-box;
    }
    .cpuGanttChartLinesStyle {
        position: relative;
        /* 指定时间轴的背景颜色 */
        background-color: #fff;

        /* 设置长度和宽度 */
        width : 1380px;
        height: 80px;

        display: grid;

    }

    .chart-item {
        /* 设置字体颜色 */
        color:rgb(208, 211, 212);;

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

        width: 80px;
        height: 80px;
    }

    /* normal-execution: 正常执行时的样式 */
    .normal-execution-block {
        box-sizing: border-box;

        /*  */
        position: absolute;
        display: inline-block;

        /* 设置宽度和高度 */
        width: 80px;
        height:40px;

        /* 边框样式 */
        border-style: solid;
        
        border-left-width: 1px;
        border-right-width: 1px;
        border-top-width: 2px;
        border-bottom-width: 2px;

        border-color: black;

        /* 设置距离顶端 40px */
        top: 40px;

        /* 设置字体大小 */
        font-size: 20px;

        /* 字体居中 */
            /* 字体在水平方向上居中 */
        text-align: center;
            /* 设置成和 div 一样的高度即可使得字体在垂直方向上居中 */
        line-height: 40px;
    }

    /* access-resource: 访问资源（critical section）*/
    .access-resource-block {
        box-sizing: border-box;

        /*  */
        position: absolute;

        display: inline-block;

        /* 背景图片 */
        background-image: url("data:image/svg+xml;utf8,<svg xmlns='http://www.w3.org/2000/svg' width='80' height='40'><line x1=\"0\" y1=\"0\" x2=\"80\" y2=\"40\" stroke=\"black\" fill=\"none\"/></svg>");

        /* 设置宽度和高度 */
        width: 80px;
        height:40px;

        border-style: solid;

        border-left-width: 1px;
        border-right-width: 1px;
        border-top-width: 2px;
        border-bottom-width: 2px;

        border-color: black;

        /* 设置距离顶端 40px */
        top: 40px;
    }

    /* help-access-resource: 访问资源（critical section）*/
    .help-access-resource-block {
        box-sizing: border-box;

        /*  */
        position: absolute;

        display: inline-block;

        /* 背景图片 */
        background-image: url("data:image/svg+xml;utf8,<svg xmlns='http://www.w3.org/2000/svg' width='80' height='40' style=\"background-color:rgb(202, 249, 160)\"><line x1=\"0\" y1=\"0\" x2=\"80\" y2=\"40\" stroke=\"black\" fill=\"none\"/></svg>");

        /* 设置宽度和高度 */
        width: 80px;
        height:40px;

        border-style: solid;

        border-left-width: 1px;
        border-right-width: 1px;
        border-top-width: 2px;
        border-bottom-width: 2px;

        border-color: black;

        /* 设置距离顶端 40px */
        top: 40px;
    }

    /* directspinning: 自旋等待资源 */
    .direct-spinning-block {
        box-sizing: border-box;

        position: absolute;

        display: inline-block;

        /* 设置宽度和高度 */
        width: 80px;
        height:40px;

        border-style: solid;

        border-left-width: 1px;
        border-right-width: 1px;
        border-top-width: 2px;
        border-bottom-width: 2px;

        border-color: black;

        /* 设置背景图片为折线 */
        background-image: url("data:image/svg+xml;utf8,<svg xmlns='http://www.w3.org/2000/svg' width='80' height='40'><polyline points='0,0 10,40 20,0 30,40 40,0 50,40 60,0 70,40 80,0' stroke='black' fill='none'/></svg>");

        /* 设置距离顶端 40px */
        top: 40px;
    }

        /* help-direct-spinning: 自旋等待资源 */
    .help-direct-spinning-block {
        box-sizing: border-box;

        position: absolute;

        display: inline-block;

        /* 设置宽度和高度 */
        width: 80px;
        height:40px;

        border-style: solid;

        border-left-width: 1px;
        border-right-width: 1px;
        border-top-width: 2px;
        border-bottom-width: 2px;

        border-color: black;

        /* 设置背景图片为折线 */
        background-image: url("data:image/svg+xml;utf8,<svg xmlns='http://www.w3.org/2000/svg' width='80' height='40' style=\"background-color:rgb(202, 249, 160)\"><polyline points='0,0 10,40 20,0 30,40 40,0 50,40 60,0 70,40 80,0' stroke='black' fill='none'/></svg>");

        /* 设置距离顶端 40px */
        top: 40px;
    }


    /* arrival-blocking: 到达阻塞 */
    .arrival-blocking-block {
        box-sizing: border-box;

        position: absolute;

        display: inline-block;

        /* 设置宽度和高度 */
        width: 80px;
        height:40px;

        border-style: solid;

        border-left-width: 1px;
        border-right-width: 1px;
        border-top-width: 2px;
        border-bottom-width: 2px;

        border-color: black;

        /* 设置背景图片为折线 */
        background-image: url("data:image/svg+xml;utf8,<svg xmlns='http://www.w3.org/2000/svg' width='80' height='40'><line x1=\"0\" y1=\"10\" x2=\"80\" y2=\"10\" stroke=\"black\" fill=\"none\"/> <line x1=\"0\" y1=\"20\" x2=\"80\" y2=\"20\" stroke=\"black\" fill=\"none\"/> <line x1=\"0\" y1=\"30\" x2=\"80\" y2=\"30\" stroke=\"black\" fill=\"none\"/> </svg>");

        /* 设置距离顶端 40px */
        top: 40px;
    }

    /* 字体大小以及位置 */
    .taskId-style {
        position: absolute;
        font-size: 16px;
        font-weight: bold;

        /* 设置字体在水平方向上居中 */
        text-align: center;

        height: 40px;
        line-height: 40px;
    }

    /* 某一时间点事件的对应的样式 */
    .symbol-style {
        position:absolute;

        /* 背景颜色设置为透明以便显示下面的元素 */
        background-color: transparent;

        height: 80px;
        width: 40px;
    }

</style>