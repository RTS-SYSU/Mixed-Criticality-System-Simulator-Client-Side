<template>
    <div>
        <!-- 
            Header: 头信息/表格头部
        -->
        <div class="header-style">
            <span class="font-style border-style">Static Task Id</span>
            <span class="font-style border-style">Dynamic Task Id</span>
            <span class="font-style border-style">Priority</span>
            <span class="font-style border-style">Criticality</span>
            <span class="font-style border-style">Release Time</span>
        </div>

        <!-- 
            任务信息
        -->
        <div>
            <GanttTaskInformation v-for="(taskInformation, index) in taskInformations" 
                    v-bind:key="index"
                    v-bind:task-information="taskInformation"
                    ref="task-information"></GanttTaskInformation>
        </div>
    </div>
</template>

<script>
import GanttTaskInformation from './GanttTaskInformation.vue';

    export default {
        name : "GanttTaskTable",

        props : {
            "taskInformations" : {
                type : Array,
                default : function() {
                    return [
                        {
                            "staticId" : 1,
                            "dynamicId": 2,
                            "priority": 998,
                            "criticality": 1,
                            "releaseTime": 15,
                            "resourceAccessTime" : [10, 20],
                            "resourceAccessIndex" : [0, 1]
                        },
                        {
                            "staticId" : 3,
                            "dynamicId": 4,
                            "priority": 998,
                            "criticality": 0,
                            "releaseTime": 15,
                            "resourceAccessTime" : [2, 15],
                            "resourceAccessIndex" : [0, 1]
                        }
                    ]
                }
            }
        },

        // 注册组件
        components : {
            GanttTaskInformation : GanttTaskInformation
        },

        // 挂载函数
        mounted() {
            window.onclick = function(event){
                // 获取目标 DOM 元素
                let displayBlocks = document.querySelectorAll('.resource-access-details')

                // 判断 event.target(触发的 DOM 元素) 是否是 access-resource-table 里面的内容
                let targetDOM = false;

                for (let i = 0; i < displayBlocks.length; ++i) {
                    if (displayBlocks[i].contains(event.target)) {
                        targetDOM = true;
                        break
                    }
                }

                if (!targetDOM){
                    for (let i = 0; i < displayBlocks.length; ++i) {
                        displayBlocks[i].style.display = "none"
                    }
                }
            }
        }
    }
</script>

<style scoped>
    .header-style {
        /* 网格化布局 */
        display: grid;

        grid-template-columns: 35% 40% 25% 25% 32%;

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


</style>