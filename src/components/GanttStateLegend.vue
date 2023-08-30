<!-- 
    甘特图图例信息：显示哪种颜色对应正在执行任务的状态
-->
<template>
    <div class='legend-style' v-bind:style="{'height' :  (stateRepresentations.length * 30 + 20) + 'px'}">
        <div class="childBlock" v-for="(stateRepresentation, index) in stateRepresentations" v-bind:key="index">
            <div v-bind:class="stateRepresentation.stateStyle"></div>
            <span class="informationStyle">{{ stateRepresentation.state }}</span>
        </div>
    </div>
</template>

<script>
    export default {
        name : 'GanttStateLegend',

        // stateRepresentation: 显示甘特图上面的颜色信息
        data: function() {
            return {
                stateRepresentations : [
                    {
                        state : 'Executing without Locks',
                        stateStyle : 'normal-execution'
                    },
                    {
                        state : 'Spinning',
                        stateStyle : 'spinning'
                    },
                    {
                        state : 'Executing in Critical Section',
                        stateStyle : 'access-resource'
                    }
                ]
            }
        }
    }
</script>


<style scoped>
    .legend-style {
        /* 边框大小以及对应的格式 */
        border-width: 2px;
        border-style: solid;
        border-color: rgb(208, 211, 212);
    }

    /* normal-execution: 正常执行时的样式 */
    .normal-execution {

        position: absolute;
        /* 背景颜色 */
        background-color: rgb(133, 193, 233);

        /* 设置宽度和高度 */
        width: 30px;
        height:20px;

        /* 加上边框大小、颜色、样式 */
        border-width: 2px;
        border-style: solid;
        border-color: rgb(133, 193, 233);

        /* 设置边框四周有角度 */
        border-radius: 40px;

        /* 设置成块内元素 */
        display: inline-block
    }

    /* access-resource: 访问资源（critical section）*/
    .access-resource {

        position: absolute;
        /* 背景颜色 */
        background-color: rgb(130, 224, 170);

        /* 设置宽度和高度 */
        width: 30px;
        height:20px;

        /* 加上边框大小、颜色、样式 */
        border-width: 2px;
        border-style: solid;
        border-color: rgb(130, 224, 170);

        /* 设置边框四周有角度 */
        border-radius: 40px;

        /* 设置成块内元素 */
        display: inline-block
        }

    /* spinning: 自旋等待资源 */
    .spinning {

        position: absolute;
        /* 背景颜色 */
        background-color: rgb(236, 112, 99);

        /* 设置宽度和高度 */
        width: 30px;
        height:20px;

        /* 加上边框大小、颜色、样式 */
        border-width: 2px;
        border-style: solid;
        border-color: rgb(236, 112, 99);

        /* 设置边框四周有角度 */
        border-radius: 40px;

        /* 设置成块内元素 */
        display: inline-block
    }

    /* 元素之间有间隔 */
    /* childBlock */
    .childBlock {
        position: relative;

        height: 20px;

        display: block;

        left: 20px;

        /* 元素之间的间隔 */
        margin-top: 10px;
        margin-left: 10px;
        margin-bottom: 10px;
        margin-right: 10px;
    }

    /* 显示甘特图信息样式的特点 */
    .informationStyle {
        position: absolute;
        /* 字体大小 */
        font-size: 18px;

        /* 需要修改成 inline-block才可以设置宽度和高度 */
        display:inline-block;
            /* 设置高度 */
        height: 20px;
        
        /* 设置距离顶端1px, 底部也1px */
        top:2px;
        bottom:2px;

        left: 40px;
    }
</style>