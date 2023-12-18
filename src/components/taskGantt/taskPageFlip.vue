<template>
    <div class="taskPageFlipStyle" v-if="taskTotalPageSize > 0">
        <button style="margin-left:170px;" @click="clickPageDown()" v-on="buttonEvent">{{ '&lt;' }}</button>
        <span>{{ (taskPageNow + 1) + " / " + (this.taskTotalPageSize) }}</span>
        <button @click="clickPageUp()" v-on="buttonEvent">{{ '&gt;' }}</button>
    </div>    
</template>

<script>
    export default {
        name : 'taskPageFlip',

        props : {
            "taskPageNow" : {
                type : Number,
                default : 0
            },

            "taskTotalPageSize" : {
                type : Number,
                default : 2
            }
        },

        methods : {
            clickPageDown : function() {
                this.$bus.$emit('taskPageDown')
            },

            clickPageUp : function() {
                this.$bus.$emit('taskPageUp')
            },

            mouseEnterMethod : function(event) {
                event.target.style.backgroundColor = 'rgb(200, 200, 200)'
            },

            mouseLeaveMethod : function(event) {
                event.target.style.backgroundColor = 'white'
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
        }
    }
</script>

<style scoped>
    * {
        box-sizing : border-box;
    }

    .taskPageFlipStyle {
        width : 440px;
        height : 20px;
        margin: 0 auto;
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

    span {
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
</style>