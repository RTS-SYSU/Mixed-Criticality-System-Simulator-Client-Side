<!-- 
    甘特图图例信息：显示哪种颜色对应正在执行任务的状态
-->
<template>
    <div class='legend-style' v-bind:style="{'height' :  ((stateRepresentations.length - 7) * 80 +  (7 * 60 + 20)) + 'px'}">

        <template v-for="(stateRepresentation, index) in stateRepresentations">
            <!-- 
                任务状态表示
            -->
            <div v-bind:key="index" class="childBlock" v-if="index < 7">
                <div v-bind:class="stateRepresentation.stateStyle"></div>
                <div class="informationStyle">{{ stateRepresentation.state }}</div>
            </div>

            <!-- 
                事件点表示:
            -->
            <!-- 
                locked: 成功获取到一个资源
            -->
            <div v-bind:key="index" class="childBlock" style="height: 60px;" v-if="stateRepresentation.stateStyle == 'locked'">
                <svg class="symbol-style">
                    <!-- 绘制圆圈 -->
                    <circle cx="40" cy="15" r="10" stroke="black" stroke-width="3" fill="black"/>
                    <!-- 绘制直线 -->
                    <line x1="40" y1="25" x2="40" y2="60" stroke="black" stroke-width="3" fill="none"/>
                </svg>
                <span class="informationStyle" style="height: 60px; line-height: 60px;">{{ stateRepresentation.state }}</span>
            </div>

            <!-- 
                locked-attempt: 尝试去获取一个资源，但是该资源正在被使用
            -->
            <div v-bind:key="index" class="childBlock" style="height: 60px;" v-if="stateRepresentation.stateStyle == 'locked-attempt'">
                <svg class="symbol-style">
                    <!-- 创建一个 SVG 元素（lock attempt）-->
                        <!-- 
                            M: 路径的起始点
                            A：rx ry x-axios-rotation large-arc-flag sweep-flag x y
                                rx: 半短轴长度
                                ry：半长轴长度
                                x-axios-rotation：椭圆相对于 x 轴的旋转角度
                                large-arc-flag: 是否使用大弧
                                    1 表示使用大弧
                                    0 则相反
                                sweep-flag：绘制弧的方向
                                    1 表示顺时针绘制
                                    0 表示逆时针绘制
                                x：圆弧终点的横坐标
                                y：圆弧终点的纵坐标
                                z：将终点和起点连接起来，形成一个闭合的形状
                        -->
                        <!-- 左边黑色填充的半圆 -->
                        <path d="M 40 25 A 10 10 0 1 1 40 5z" stroke="black" stroke-width="3" fill="black"/>
                        <!-- 右边白色无填充的半圆 -->
                        <path d="M 40 5 A 10 10 0 1 1 40 25" stroke="black" stroke-width="3" fill="none"/>
                        <!-- 直线 -->
                        <line x1="40" y1="25" x2="40" y2="60" stroke="black" stroke-width="3" fill="none"/>
                </svg>
                <span class="informationStyle" style="height: 60px; line-height: 60px;">{{ stateRepresentation.state }}</span>
            </div>

            <!-- 
                withdraw：任务撤销资源申请
            -->
            <div v-bind:key="index" class="childBlock" style="height: 60px;" v-if="stateRepresentation.stateStyle == 'withdraw'">
                <svg class="symbol-style">
                    <!-- 绘制圆圈 -->
                    <circle cx="40" cy="15" r="10" stroke="#00a3ff" stroke-width="3" fill="#00a3ff"/>
                    <!-- 绘制直线 -->
                    <line x1="40" y1="25" x2="40" y2="60" stroke="black" stroke-width="3" fill="none"/>
                </svg>
                <span class="informationStyle" style="height: 60px; line-height: 60px;">{{ stateRepresentation.state }}</span>
            </div>

            <!-- 
                locked-attempt: 尝试去获取一个资源，但是该资源正在被使用
            -->
            <div v-bind:key="index" class="childBlock" style="height: 60px;" v-if="stateRepresentation.stateStyle == 'unlocked'">
                 <svg class="symbol-style">
                    <circle cx="40" cy="15" r="10" stroke="black" stroke-width="3" fill="none"/>
                    <!-- 绘制直线 -->
                    <line x1="40" y1="25" x2="40" y2="60" stroke="black" stroke-width="3" fill="none"/>
                </svg>
                <span class="informationStyle" style="height: 60px; line-height: 60px;">{{ stateRepresentation.state }}</span>
            </div>

            <!-- 
                release: 一个任务的发布
             -->
            <div v-bind:key="index" class="childBlock" style="height: 60px;" v-if="stateRepresentation.stateStyle == 'release'">
                 <svg class="symbol-style">
                    <!-- 绘制直线 -->
                    <line x1="40" y1="25" x2="40" y2="60" stroke="black" stroke-width="3" fill="none"/>
                    <!-- 绘制三角形 -->
                    <path d="M 40 10 L 30 25 L 50 25 z" stroke="black" stroke-width="3" fill="black"/>
                </svg>

                <span class="informationStyle" style="height: 60px; line-height: 60px;">{{ stateRepresentation.state }}</span>
            </div>

            <!-- 
                completion: 任务的完成
             -->
             <div v-bind:key="index" class="childBlock" style="height: 60px;" v-if="stateRepresentation.stateStyle == 'completion'">
                 <svg class="symbol-style">
                    <!-- 绘制直线（竖线） -->
                    <line x1="40" y1="15" x2="40" y2="60" stroke="black" stroke-width="3" fill="none"/>
                    <!-- 绘制直线 （横线）-->
                    <line  x1="25" y1="15" x2="55" y2="15" stroke="black" stroke-width="3" fill="none" />
                </svg>
                <span class="informationStyle" style="height: 60px; line-height: 60px;">{{ stateRepresentation.state }}</span>
            </div>

            <!-- 
                switch task: 切换任务
             -->
             <div v-bind:key="index" class="childBlock" style="height: 60px;" v-if="stateRepresentation.stateStyle == 'switch-task'">
                <svg class="symbol-style">
                    <line x1="40" y1="0" x2="40" y2="60" stroke="rgb(0, 185, 255)" stroke-width="3"></line>
                </svg>   
                <span class="informationStyle" style="height: 60px; line-height: 60px;">{{ stateRepresentation.state }}</span>
            </div>

            <!-- 
                shut-down: 关闭一个任务
             -->
            <div v-bind:key="index" class="childBlock" style="height : 60px;" v-if="stateRepresentation.stateStyle == 'killed'">
                 <svg class="symbol-style">
                    <line x1="40" y1="20" x2="40" y2="60" stroke="rgb(255, 208, 0)" stroke-width="4"></line>
                    <line x1="30" y1="10" x2="50" y2="30" stroke="rgb(255, 208, 0)" stroke-width="4"></line>
                    <line x1="30" y1="30" x2="50" y2="10" stroke="rgb(255, 208, 0)" stroke-width="4"></line>
                 </svg>
                 <span class="informationStyle" style="height: 60px; line-height: 60px;">{{ stateRepresentation.state }}</span>
            </div>

            <!-- 
                criticality-switch: 系统关键级发生变化
             -->
             <div v-bind:key="index" class="childBlock" style="height : 60px;" v-if="stateRepresentation.stateStyle == 'criticality-switch'">
                 <svg class="symbol-style">
                    <circle cx="40" cy="30" r="10" stroke="red" stroke-width="4" fill="red"></circle>
                 </svg>
                 <span class="informationStyle" style="height: 60px; line-height: 60px;">{{ stateRepresentation.state }}</span>
            </div>
            
        </template>

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
                        state : 'Direct Spinning',
                        stateStyle : 'direct-spinning'
                    },
                    {
                        state : 'Indirect Spinning',
                        stateStyle : 'indirect-spinning'
                    },
                    {
                        state : 'Arrival Blocking',
                        stateStyle : 'arrival-blocking'
                    },
                    {
                        state : 'Access Resource',
                        stateStyle : 'access-resource'
                    },
                    {
                        state : 'Help task to access resource',
                        stateStyle : 'help-direct-spinning'
                    },
                    {
                        state : 'Access Resource(Immigrate)',
                        stateStyle : 'help-access-resource'
                    },
                    {
                        state : 'Get a resource',
                        stateStyle : 'locked'
                    },
                    {
                        state : 'Wait for a resource',
                        stateStyle : 'locked-attempt'
                    },
                    {
                        state : 'Withdraw of application',
                        stateStyle : 'withdraw'
                    },
                    {
                        state : 'Release a resource',
                        stateStyle : 'unlocked'
                    },
                    {
                        state : 'Release a task',
                        stateStyle : 'release'
                    },
                    {
                        state : 'Complete a task',
                        stateStyle : 'completion'
                    },
                    {
                        state : 'Switch task',
                        stateStyle : 'switch-task'
                    },
                    {
                        state : 'Shut Down a task',
                        stateStyle : 'killed'
                    },
                    {
                        state : 'System Criticality Switch',
                        stateStyle : 'criticality-switch'
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
        display: inline-block;

        vertical-align: top;
    }

    /* access-resource: 访问资源（critical section）*/
    .access-resource {

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
        display: inline-block;

        vertical-align: top;
    }

    /* help-access-resource: 帮助机制访问资源（critical section）*/
    .help-access-resource {

        /* 背景图片 */
        background-image:url("data:image/svg+xml;utf8,<svg xmlns='http://www.w3.org/2000/svg' width='80' height='40' style=\"background-color:rgb(202, 249, 160)\"><line x1=\"0\" y1=\"0\" x2=\"80\" y2=\"40\" stroke=\"black\" fill=\"none\"/></svg>");

        /* 设置宽度和高度 */
        width: 80px;
        height:40px;

        /* 加上边框大小、颜色、样式 */
        border-width: 2px;
        border-style: solid;
        border-color: black;

        /* 设置成块内元素 */
        display: inline-block;

        vertical-align: top;
    }

    /* direct-spinning: 直接自旋等待资源 */
    .direct-spinning {

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
        display: inline-block;

        vertical-align: top;
    }

    /* help-direct-spinning: helpTask 帮助 preemptedRunningTask 执行 */
    .help-direct-spinning {

        /* 背景颜色 */
        background-image: url("data:image/svg+xml;utf8,<svg xmlns='http://www.w3.org/2000/svg' width='80' height='40' style=\"background-color:rgb(202, 249, 160)\"><polyline points='0,0 10,40 20,0 30,40 40,0 50,40 60,0 70,40 80,0' stroke='black' fill='none'/></svg>");

        /* 设置宽度和高度 */
        width: 80px;
        height:40px;

        /* 加上边框大小、颜色、样式 */
        border-width: 2px;
        border-style: solid;
        border-color: black;

        /* 设置成块内元素 */
        display: inline-block;
        
        vertical-align: top;
    }

    /* indirect-spinning: 间接自旋 */
    .indirect-spinning {

        /* 背景颜色 */
        background-image: url("data:image/svg+xml;utf8,<svg xmlns='http://www.w3.org/2000/svg' width='80' height='40'><line x1=\"10\" y1=\"0\" x2=\"10\" y2=\"40\" stroke=\"black\" fill=\"none\"/> <line x1=\"20\" y1=\"0\" x2=\"20\" y2=\"40\" stroke=\"black\" fill=\"none\"/> <line x1=\"30\" y1=\"0\" x2=\"30\" y2=\"40\" stroke=\"black\" fill=\"none\"/> <line x1=\"40\" y1=\"0\" x2=\"40\" y2=\"40\" stroke=\"black\" fill=\"none\"/> <line x1=\"50\" y1=\"0\" x2=\"50\" y2=\"40\" stroke=\"black\" fill=\"none\"/> <line x1=\"60\" y1=\"0\" x2=\"60\" y2=\"40\" stroke=\"black\" fill=\"none\"/> <line x1=\"70\" y1=\"0\" x2=\"70\" y2=\"40\" stroke=\"black\" fill=\"none\"/> </svg>");

        /* 设置宽度和高度 */
        width: 80px;
        height:40px;

        /* 加上边框大小、颜色、样式 */
        border-width: 2px;
        border-style: solid;
        border-color: black;

        /* 设置成块内元素 */
        display: inline-block;

        vertical-align: top;
    }

    /* arrival-blocking: 到达阻塞 */
    .arrival-blocking {

        /* 背景颜色 */
        background-image: url("data:image/svg+xml;utf8,<svg xmlns='http://www.w3.org/2000/svg' width='80' height='40'><line x1=\"0\" y1=\"10\" x2=\"80\" y2=\"10\" stroke=\"black\" fill=\"none\"/> <line x1=\"0\" y1=\"20\" x2=\"80\" y2=\"20\" stroke=\"black\" fill=\"none\"/> <line x1=\"0\" y1=\"30\" x2=\"80\" y2=\"30\" stroke=\"black\" fill=\"none\"/> </svg>");

        /* 设置宽度和高度 */
        width: 80px;
        height:40px;

        /* 加上边框大小、颜色、样式 */
        border-width: 2px;
        border-style: solid;
        border-color: black;

        /* 设置成块内元素 */
        display: inline-block;

        vertical-align: top;
    }

    /* SYMBOL-STYLE: 符号对应的样式 */
    .symbol-style {

        /* 设置宽度和高度 */
        width: 80px;
        height:80px;

        /* 加上边框大小、颜色、样式
        border-width: 2px;
        border-style: solid;
        border-color: black; */

        /* 设置成块内元素 */
        display: inline-block;

        vertical-align: top;
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
        display: inline-block;
        /* 字体大小 */
        font-size: 18px;

            /* 设置高度 */
        height: 40px;
        
        /* 设置距离顶端1px, 底部也1px */
        margin-left: 20px;

        /* 字体垂直方向上居中 */
        line-height: 40px;

        vertical-align: top;
    }
</style>