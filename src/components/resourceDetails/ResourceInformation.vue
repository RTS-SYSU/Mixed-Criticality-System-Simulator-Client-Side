<template>
    <!-- 
        展示每一个资源的详细信息
    -->
    <div>
        <!-- 
            显示资源的基本信息
         -->
        <div class="content-style" v-on="contentEvent">
            <span ref="content1" class="font-style border-style" >{{ resourceInformation.resourceId }}</span>
            <span ref="content2" class="font-style border-style" >{{ resourceInformation.c_low }}</span>
            <span ref="content3" class="font-style border-style" >{{ resourceInformation.c_high }}</span>
            <span ref="content4" class="font-style border-style" >{{ resourceInformation.isGlobalResource }}</span>
        </div>

        <!-- 
            显示访问资源的所有任务
         -->
        <div ref="task-access-details" class="task-access-details" v-bind:style="taskDetailsStyle">
            <div class="grid-style">
                <span class="font-style border-style" style="font-size: 18px;">Access Resource Task</span>
            </div>

            <div class="grid-style" style="height : 42px;" v-for="(taskId, index) in resourceInformation.accessTasks" v-bind:key="index">
                <span class="font-style border-style" style="font-size: 18px;">{{ 'Task id ' + taskId }}</span>
            </div>
        </div>
    
    </div>
</template>

<script>
    export default {
        name : "ResourceInformation",

        // 父组件传递给子组件的信息
        props : {
            // 任务的基本信息
            "resourceInformation" : {
                type: Object,
                default : function() {
                    return {
                        "resourceId" : 0,
                        "c_low" : 10,
                        "c_high" : 15,
                        "isGlobalResource" : false,
                        "accessTasks" : [0, 1]
                    }
                }
            },

            // 现在是第几个任务
            "resourceIndex" : {
                type: Number,
                default : function() {
                    return 1;
                }
            },

            // 现在是遍历到资源的第几页
            "resourceInformationPageNow" : {
                type : Number,
                default : 0
            },

            // 每一页可以容纳的数量
            "maxItemSize" : {
                type : Number,
                default : 5
            },

            "taskInformationLength" : {
                type : Number,
                default : 2
            },

            "taskInformationPageNow" : {
                type : Number, 
                default : 0
            },

            "taskTableMaxItemSize" : {
                type : Number,
                default : 7
            }
        },

        // 计算属性
        computed : {
            // 在 content 上的事件以及解决办法
            contentEvent : function()
            { 
                return {
                    mouseenter : this.mouseEnterMethod,
                    mouseleave : this.mouseLeaveMethod,
                    click : this.clickContentStyle
                }
            },

            // 设计访问资源的详细信息
            taskDetailsStyle : function() {
                let taskInformationTableHeight = Math.min(this.taskInformationLength - this.taskInformationPageNow * this.taskTableMaxItemSize, this.taskTableMaxItemSize) * 42 + 42 + 30 + 30
                return {
                    'height' : ((1 + this.resourceInformation.accessTasks.length) * 42) + 'px',
                    'top' : 20 + 194 + 20 + taskInformationTableHeight + 20 + 40 + (this.resourceIndex - this.resourceInformationPageNow * this.maxItemSize) * 42 + 'px'
                }
            }
        },

         // 定义方法
         methods : {
            // 把以下这两个方法设置在 div 的原因是：利用冒泡，减少代码量
            // 鼠标移入时颜色变深
            mouseEnterMethod : function()
            {
                // 修改背景颜色更深，
                this.$refs['content1'].style.backgroundColor = 'rgb(246, 246, 246)'
                this.$refs['content2'].style.backgroundColor = 'rgb(246, 246, 246)'
                this.$refs['content3'].style.backgroundColor = 'rgb(246, 246, 246)'
                this.$refs['content4'].style.backgroundColor = 'rgb(246, 246, 246)'
            },

            // 鼠标移出时颜色变浅
            mouseLeaveMethod : function()
            {
                // 修改背景颜色为原来的颜色
                this.$refs['content1'].style.backgroundColor = 'white'
                this.$refs['content2'].style.backgroundColor = 'white'
                this.$refs['content3'].style.backgroundColor = 'white'
                this.$refs['content4'].style.backgroundColor = 'white'
            },

            // 鼠标点击 div （包括 span，利用冒泡）时，显示出任务的具体信息
            clickContentStyle : function(event) 
            {
                // 先隐藏所有的 resource-access-details
                let displayBlocks = document.querySelectorAll('.task-access-details')
                for (let i = 0; i < displayBlocks.length; ++i) {
                    displayBlocks[i].style.display = 'none'
                }

                // 然后再修改 style 让其显示出来
                this.$refs['task-access-details'].style.display = 'block';

                // 停止冒泡事件的发生
                event.stopPropagation();
                
            }
        }
    }
</script>

<style scoped>
    .content-style {
        /* 网格化布局 */
        display: grid;

        grid-template-columns: 200px 142.5px 142.5px 200px;
    }

    /* .content-style:hover {
        background-color: rgb(246, 246, 246);
    } */

    .font-style {
        font-size: 18px;

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

    /* 隐藏任务的详细信息 */
    .task-access-details {
        /* 之后会隐藏该 grid */
        display:none;

        /* 使用绝对定位定位到页面中间 */
        position: absolute;

        left: 507px;

        background-color: white;
        width: 250px;

        /* 边框样式 */
        border-style: solid;
        
        /* border-width: top right bottom left, 按顺时针依次作用 */
        border-width: 2px 0px 0px 2px;

        border-color: rgb(208, 211, 212);
    }

    /* 资源访问的头部信息样式 */
    .grid-style {
        display: grid;

        height: 42px;

        grid-template-columns: 100%;
    }
</style>