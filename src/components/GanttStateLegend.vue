<!-- 
    甘特图图例信息：显示哪种颜色对应正在执行任务的状态
-->
<template>
    <div class='legend-style' v-bind:style="{'height' :  (stateRepresentations.length * 60 + 20) + 'px'}">
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
        background-color: white;

        /* 设置宽度和高度 */
        width: 80px;
        height:40px;

        /* 加上边框大小、颜色、样式 */
        border-width: 2px;
        border-style: solid;
        border-color: black;


        /* 设置成块内元素 */
        display: inline-block
    }

    /* access-resource: 访问资源（critical section）*/
    .access-resource {

        position: absolute;
        /* 背景图片 */
        background-image:url("data:image/svg+xml;utf8,<svg xmlns='http://www.w3.org/2000/svg' width='80' height='40'><line x1=\"0\" y1=\"0\" x2=\"80\" y2=\"40\" stroke=\"black\" fill=\"none\"/></svg>");

        /* 设置宽度和高度 */
        width: 80px;
        height:40px;

        /* 加上边框大小、颜色、样式 */
        border-width: 2px;
        border-style: solid;
        border-color: black;

        /* 设置成块内元素 */
        display: inline-block
        }

    /* spinning: 自旋等待资源 */
    .spinning {

        position: absolute;
        /* 背景颜色 */
        background-image: url("data:image/svg+xml;utf8,<svg xmlns='http://www.w3.org/2000/svg' width='80' height='40'><polyline points='0,0 10,40 20,0 30,40 40,0 50,40 60,0 70,40 80,0' stroke='black' fill='none'/></svg>");

        /* 设置宽度和高度 */
        width: 80px;
        height:40px;

        /* 加上边框大小、颜色、样式 */
        border-width: 2px;
        border-style: solid;
        border-color: black;

        /* 设置成块内元素 */
        display: inline-block
    }

    /* 元素之间有间隔 */
    /* childBlock */
    .childBlock {
        position: relative;

        height: 40px;

        display: block;

        left: 20px;

        margin-top: 20px;
    }

    /* 显示甘特图信息样式的特点 */
    .informationStyle {
        position: absolute;

        display: inline-block;

        /* 字体大小 */
        font-size: 18px;

        /* 需要修改成 inline-block才可以设置宽度和高度 */
        display:inline-block;
            /* 设置高度 */
        height: 40px;
        
        /* 设置距离顶端1px, 底部也1px */
        top:2px;
        bottom:2px;

        left: 100px;

        /* 字体垂直方向上居中 */
        line-height: 40px;
    }
</style>