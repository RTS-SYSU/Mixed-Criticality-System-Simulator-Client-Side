<template>
    <div>
        <!-- 
            @contextmenu.prevent 阻止浏览器默认的上下文菜单弹出
            并在用户右键点击时调用 Vue.js 中的 rightmouseClick 函数
         -->
        <div class="content-style" v-on="contentEvent" @contextmenu.prevent="rightmouseClick($event)">
            <span ref="content1" class="font-style border-style" >{{ taskInformation.staticPid }}</span>
            <span ref="content2" class="font-style border-style" >{{ taskInformation.allocation }}</span>
            <span ref="content3" class="font-style border-style" >{{ taskInformation.priority }}</span>
            <span ref="content4" class="font-style border-style" >{{ taskInformation.criticality }}</span>
            <span ref="content5" class="font-style border-style" >{{ taskInformation.wcctlow }}</span>
            <span ref="content6" class="font-style border-style" >{{ taskInformation.wccthigh }}</span>
            <span ref="content7" class="font-style border-style" >{{ taskInformation.utilization.toFixed(2) }}</span>
            <span ref="content8" class="font-style border-style" >{{ taskInformation.period }}</span>
            <span ref="content9" class="font-style border-style" >{{ taskInformation.totalTime }}</span>
        </div>
    
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
        
        <!-- 
            WorstCase 设置菜单
         -->
         <WorstCaseSettingMenu ref="worst-case-setting-menu" v-bind:sufferTaskId="taskInformation.staticPid"/>
    </div>
</template>

<script>
    import WorstCaseSettingMenu from './WorstCaseSettingMenu.vue';
    export default {
        components : {
            WorstCaseSettingMenu : WorstCaseSettingMenu
        },

        // 父组件传递给子组件的信息
        props : {
            // 任务的基本信息
            "taskInformation" : {
                type: Object,
                default : function() {
                    return {
                        "staticPid" : 1,
                        "dynamicPid": 2,
                        "priority": 998,
                        "criticality": 1,
                        "releaseTime": 15,
                        "resourceAccessIndex" : [0, 1],
                        "resourceAccessTime" : [10, 20],
                    }
                }
            },

            // 现在是第几个任务
            "taskIndex" : {
                type: Number,
                default : function() {
                    return 1;
                }
            },

            // 现在是遍历到资源的第几页
            "taskInformationPageNow" : {
                type : Number,
                default : 0
            },

            // 每一页可以容纳的数量
            "maxItemSize" : {
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
            resourceDetailsStyle : function() {
                return {
                    'top' : 20 + 194 + 20 + 42 + (this.taskIndex - this.taskInformationPageNow * this.maxItemSize) * 42 +'px'
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
                this.$refs['content6'].style.backgroundColor = 'rgb(246, 246, 246)'
                this.$refs['content7'].style.backgroundColor = 'rgb(246, 246, 246)'
                this.$refs['content8'].style.backgroundColor = 'rgb(246, 246, 246)'
                this.$refs['content9'].style.backgroundColor = 'rgb(246, 246, 246)'
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
                this.$refs['content6'].style.backgroundColor = 'white'
                this.$refs['content7'].style.backgroundColor = 'white'
                this.$refs['content8'].style.backgroundColor = 'white'
                this.$refs['content9'].style.backgroundColor = 'white'
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
                
            },

            // 鼠标右击 div, 显示出较差运行情况配置菜单
            rightmouseClick : function(event) {
                // 先隐藏所有的 worst-case-setting-menu
                let displayBlocks = document.querySelectorAll('.WorstCaseSettingMenu-style')
                
                for (let i = 0; i < displayBlocks.length; ++i) {
                    displayBlocks[i].style.display = 'none'
                }

                // 然后再修改 style 让其显示出来
                this.$refs['worst-case-setting-menu'].$el.style.display = 'block'
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

        grid-template-columns: 122.5px 122.5px 120px 120px 155px 155px 155px 122.5px 122.5px;

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

        left: 505px;

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

    }
</style>