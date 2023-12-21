<template>
    <!-- 
        资源信息表格
     -->
    <div>
        <div class="resource-information" v-bind:style="calculateHeight()">
            <!-- 
                Header: 头信息/标题信息
            -->
            <div class="header-style">
                <span class="font-style border-style">Resource id</span>
                <span class="font-style border-style">c_low</span>
                <span class="font-style border-style">c_high</span>
                <span class="font-style border-style">Global Resource</span>
            </div>

            <!-- 
                任务信息
            -->
            <div>
                <template v-for="(item, index) in this.range(resourceInformationPageNow * maxItemSize, Math.min((resourceInformationPageNow + 1 ) * maxItemSize, resourceInformations.length))">
                    <ResourceInformation
                        v-bind:key="index"
                        v-bind:resource-information="resourceInformations[item]"
                        v-bind:resource-index="item"
                        v-bind:resourceInformationPageNow = "resourceInformationPageNow"
                        v-bind:maxItemSize = "maxItemSize"
                        v-bind:taskInformationLength="taskInformationLength"
                        v-bind:taskInformationPageNow="taskInformationPageNow"
                        v-bind:taskTableMaxItemSize="taskTableMaxItemSize"
                        ref="resource-information"></ResourceInformation>
                </template>
            </div>
        </div>

        <div class="resourceInformationPageFlipStyle" v-if="resourceInformationTotalPageSize > 0">
            <button style="margin-left:10px;" @click="clickPageDown()" v-on="buttonEvent">{{ '&lt;' }}</button>
            <span class="flip-span">{{ (resourceInformationPageNow + 1) + " / " + (this.resourceInformationTotalPageSize)}}</span>
            <button @click="clickPageUp()" v-on="buttonEvent">{{ '&gt;' }}</button>
        </div>
    </div>
</template>

<script>
    import ResourceInformation from './ResourceInformation.vue'
    export default {
        name : "ResourceTable",

        components : {
            ResourceInformation : ResourceInformation
        },

        // 注册数据
        data(){
            return {
                resourceInformationPageNow : 0,
                maxItemSize : 5
            }
        },

        props : {
            "resourceInformations" : {
                type : Array,
                default : function() {
                    return [
                        {
                            "resourceId" : 0,
                            "c_low" : 10,
                            "c_high" : 15,
                            "isGlobalResource" : false,
                            "accessTasks" : [0, 1]
                        },
                        {
                            "resourceId" : 1,
                            "c_low" : 15,
                            "c_high" : 20, 
                            "isGlobalResource" : false,
                            "accessTasks" : [0, 1]
                        }
                    ]
                }
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
            resourceInformationTotalPageSize : function() {
                var ret = Math.ceil(this.resourceInformations.length / this.maxItemSize)
                if (this.resourceInformations.length % this.maxItemSize == 0) {
                    ret = ret - 1;
                }
                return ret;
            },

            // 在 button 上的事件以及解决办法
            buttonEvent : function() {
                return {
                    mouseenter : this.mouseEnterMethod,
                    mouseleave : this.mouseLeaveMethod
                }
            }
        },

        methods :{
            clickPageDown() {
                this.resourceInformationPageNow = Math.max(this.resourceInformationPageNow - 1, 0)
            },

            clickPageUp() {
                this.resourceInformationPageNow = Math.min(this.resourceInformationPageNow + 1, this.resourceInformationTotalPageSize - 1)
            },

            mouseEnterMethod : function(event) {
                event.target.style.backgroundColor = 'rgb(200, 200, 200)'
            },

            mouseLeaveMethod : function(event) {
                event.target.style.backgroundColor = 'white'
            },

            calculateHeight() {
                return {
                    'height' : (Math.min(this.resourceInformations.length - this.resourceInformationPageNow * this.maxItemSize, this.maxItemSize) * 42 + 42 + 30) + 'px'
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
    }
</script>

<style scoped>
    * {
        box-sizing: border-box;
    }

    .header-style {
        /* 网格化布局 */
        display: grid;

        grid-template-columns: 200px 142.5px 142.5px 200px;
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

    .resource-information {
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

    .resourceInformationPageFlipStyle {
        width : 120px;
        height : 30px;
        margin: 0 auto;
    }
</style>