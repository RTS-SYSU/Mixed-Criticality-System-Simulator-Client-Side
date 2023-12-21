<template>
    <!-- 
        一个任务的具体信息。
     -->
    <div>
        <div class="content-style" v-on="contentEvent">
            <span ref="content1" class="font-style border-style" >{{ taskInformation.staticId }}</span>
            <span ref="content2" class="font-style border-style" >{{ taskInformation.dynamicId }}</span>
            <span ref="content3" class="font-style border-style" >{{ taskInformation.priority }}</span>
            <span ref="content4" class="font-style border-style" >{{ taskInformation.criticality }}</span>
            <span ref="content5" class="font-style border-style" >{{ taskInformation.releaseTime }}</span>
        </div>

        <!-- 
            该任务在执行过程中访问的资源以及访问的时间点。
         -->
        <div ref="resource-access-details" class="resource-access-details" v-bind:style="resourceDetailsStyle">
            <div class="grid-style">
                <span class="font-style border-style" style="font-size: 18px;">Access Resource</span>
                <span class="font-style border-style" style="font-size: 18px;">Access Resource Time</span>
            </div>

            <div class="grid-style" v-for="(resourceId, index) in taskInformation.resourceAccessIndex" v-bind:key="index">
                <span class="font-style border-style" style="font-size: 18px;">{{ resourceId }}</span>
                <span class="font-style border-style" style="font-size: 18px;">{{ taskInformation.resourceAccessTime[index] }}</span>
            </div>
        </div>


    </div>
</template>

<script>
    export default {
        // 父组件传递给子组件的信息
        props : {
            // 任务的基本信息
            "taskInformation" : {
                type: Object,
                default : function() {
                    return {
                        "staticId" : 1,
                        "dynamicId": 2,
                        "priority": 998,
                        "criticality": 1,
                        "releaseTime": 15,
                        "resourceAccessIndex" : [0, 1],
                        "resourceAccessTime" : [10, 20]
                    }
                }
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
            resourceDetailsStyle : function() {
                return {
                    'height' : ((1 + this.taskInformation.resourceAccessTime.length) * 40) + 'px'
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
                this.$refs['content5'].style.backgroundColor = 'rgb(246, 246, 246)'
            },

            // 鼠标移出时颜色变浅
            mouseLeaveMethod : function()
            {
                // 修改背景颜色为原来的颜色
                this.$refs['content1'].style.backgroundColor = 'white'
                this.$refs['content2'].style.backgroundColor = 'white'
                this.$refs['content3'].style.backgroundColor = 'white'
                this.$refs['content4'].style.backgroundColor = 'white'
                this.$refs['content5'].style.backgroundColor = 'white'
            },

            // 鼠标点击 div （包括 span，利用冒泡）时，显示出任务的具体信息
            clickContentStyle : function(event) 
            {
                // 先隐藏所有的 resource-access-details
                let displayBlocks = document.querySelectorAll('.resource-access-details')
                for (let i = 0; i < displayBlocks.length; ++i) {
                    displayBlocks[i].style.display = 'none'
                }

                // 然后再修改 style 让其显示出来
                this.$refs['resource-access-details'].style.display = 'block';

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

        grid-template-columns: 35% 40% 25% 25% 32%;

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
    .resource-access-details {
        /* 之后会隐藏该 grid */
        display:none;

        /* 使用绝对定位定位到页面中间 */
        position: absolute;

        left: 2010px;
        top: 220px;

        background-color: white;
        width: 400px;

        /* 边框样式 */
        border-style: solid;
        
        /* border-width: top right bottom left, 按顺时针依次作用 */
        border-width: 2px 0px 0px 2px;

        border-color: rgb(208, 211, 212);
    }

    /* 资源访问的头部信息样式 */
    .grid-style {
        display: grid;

        grid-template-columns: 45% 55%;

        height: 40px;
    }
    
    /* 访问资源的详细信息对应的样式 */
    .body-style {
        /* 边框样式 */
        border-style: solid;
        
        /* border-width: top right bottom left, 按顺时针依次作用 */
        border-width: 0px 2px 2px 0px;

        /* 边框颜色 */
        border-color: rgb(208, 211, 212);

        font-size: 18px;

        /* 字体颜色 */
        color: rgb(208, 211, 212);

        /* 字体加粗效果 */
        font-weight: bold;

        /* 字体在水平方向上居中 */
        text-align: center;

        /* 字体在垂直方向上居中 */
        line-height: 40px;

        /* 设置高度 */
        height: 40px;
    }
</style>