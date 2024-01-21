<template>
    <!-- 
        任务信息表格
     -->
    <div>
        <div class="task-information" v-bind:style="calculateHeight()">
            <!-- 
                Header: 头信息/标题信息
            -->
            <div class="header-style">
                <span class="font-style border-style">Task id</span>
                <span class="font-style border-style">Allocation</span>
                <span class="font-style border-style">Priority</span>
                <span class="font-style border-style">Criticality</span>
                <span class="font-style border-style">WCCT_low</span>
                <span class="font-style border-style">WCCT_high</span>
                <span class="font-style border-style">Utilization</span>
                <span class="font-style border-style">Period</span>
                <span class="font-style border-style">Total time</span>
            </div>

            <!-- 
                任务信息
            -->
            <div>
                <template v-for="(item, index) in this.range(taskInformationPageNow * maxItemSize, Math.min((taskInformationPageNow + 1 ) * maxItemSize, taskInformations.length))">
                    <TaskInformation
                        v-bind:key="index"
                        v-bind:task-information="taskInformations[item]"
                        v-bind:task-index="item"
                        v-bind:taskInformationPageNow = "taskInformationPageNow"
                        v-bind:maxItemSize = "maxItemSize"
                        ref="task-information"></TaskInformation>
                </template>
            </div>
        </div>

        <!-- 翻页 -->
        <div class="taskInformationPageFlipStyle" v-if="taskInformationTotalPageSize > 1">
            <button style="margin-left:10px;" @click="clickPageDown()" v-on="buttonEvent">{{ '&lt;' }}</button>
            <span class="flip-span">{{ (taskInformationPageNow + 1) + " / " + (this.taskInformationTotalPageSize)}}</span>
            <button @click="clickPageUp()" v-on="buttonEvent">{{ '&gt;' }}</button>
        </div>

    </div>
</template>

<script>
    import TaskInformation from './TaskInformation.vue';

    export default {
        name : "TaskTable",

        props : {
            "taskInformations" : {
                type : Array,
                default : function() {
                    return [
                        {
                            "staticPid" : 1,
                            "dynamicPid": 2,
                            "priority": 998,
                            "criticality": 1,
                            "releaseTime": 15,
                            "resourceAccessTime" : [10, 20],
                            "resourceAccessIndex" : [0, 1]
                        },
                        {
                            "staticPid" : 3,
                            "dynamicPid": 4,
                            "priority": 998,
                            "criticality": 0,
                            "releaseTime": 15,
                            "resourceAccessTime" : [2, 15],
                            "resourceAccessIndex" : [0, 1]
                        }
                    ]
                }
            },

            "taskInformationPageNow" : {
                type : Number,
                default : 0
            },

            "maxItemSize" : {
                type : Number,
                defualt : 7
            },

            "taskInformationTotalPageSize" : {
                type : Number,
                default : 0
            }
        },


        methods :{
            // TaskTable 翻页
            clickPageDown() {
                this.$bus.$emit('TaskTablePageDown')
            },

            clickPageUp() {
                this.$bus.$emit('TaskTablePageUp')
            },

            mouseEnterMethod : function(event) {
                event.target.style.backgroundColor = 'rgb(200, 200, 200)'
            },

            mouseLeaveMethod : function(event) {
                event.target.style.backgroundColor = 'white'
            },

            calculateHeight() {
                return {
                    'height' : (Math.min(this.taskInformations.length - this.taskInformationPageNow * this.maxItemSize, this.maxItemSize) * 42 + 42 + 30) + 'px'
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
            // 在 button 上的事件以及解决办法
            buttonEvent : function() {
                return {
                    mouseenter : this.mouseEnterMethod,
                    mouseleave : this.mouseLeaveMethod
                }
            }
        },

        // 注册组件
        components : {
            TaskInformation : TaskInformation
        }
    }
</script>

<style scoped>
    * {
        box-sizing: border-box;
    }

    .header-style {
        /* 网格化布局 */
        display: grid;

        grid-template-columns: 122.5px 122.5px 120px 120px 155px 155px 155px 122.5px 122.5px;
    }

    .font-style {
        font-size: 20px;

        /* 字体颜色 */
        color: rgb(208, 211, 212);

        /* 字体加粗效果 */
        font-weight: bold;

        /* 字体在水平方向上居中 */
        text-align: center;

        /* 字体在垂直方向上居中 */
        line-height: 40px;
    }

    /* 加上边框 */
    .border-style {
        border-style: solid;
        
        /* border-width: top right bottom left, 按顺时针依次作用 */
        border-width: 0px 2px 2px 0px;

        border-color: rgb(208, 211, 212);

        /* 设置一下高度 */
        height: 40px;
    }

    .task-information {
        display: block;

        /* 设置其为父组件宽度的 20 % */
        width: 487px;
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

        margin-top: 20px;
    }

    button {
        /* 字体的颜色 */
        color : rgb(119, 119, 119);

        /* 字体大小 */
        font-size: 20px;

        /* 背景颜色 */
        background-color : rgb(255, 255, 255);

        /* 按钮宽度和高度设置 */
        width : 30px;
        height: 30px;
        display:inline-block;

        /* 取消边框 */
        border-width : 0px;
    }

    .flip-span {
        /* 字体的颜色 */
        color : rgb(99, 99, 99);

        /* 字体颜色 */
        font-size : 18px;

        /* 背景颜色 */
        background-color : rgb(255, 255, 255);

        /* 字体的宽度和高度设置 */
        width : 40px;
        height: 30px;
        display:inline-block;
        border-width : 0px;

        /* 居中显示 */
        text-align: center;
    }

    .taskInformationPageFlipStyle {
        width : 120px;
        height : 30px;
        margin: 0 auto;
    }
</style>