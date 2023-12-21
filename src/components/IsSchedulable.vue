<template>
    <!-- 
        显示资源共享协议的可调度性。
     -->
    <div class="isSchedulableStyle">
        <!-- MSRP 协议 -->
        <div class="item" ref="protocolMSRP">
            <!-- 协议名称 -->
            <span class="informationStyle"> MSRP 协议调度情况</span>
            
            <!-- 打勾：在该协议下能够进行调度 -->
            <svg class="symbol-style" v-if="msrpSchedulable">
                <rect x="0" y="0" width="50" height="50" fill="rgb(202, 249, 160)"></rect>
                <line x1="12" y1="25" x2="25" y2="45" stroke="black" stroke-width="3"></line>
                <line x1="25" y1="45" x2="45" y2="5" stroke="black" stroke-width="3"></line>
            </svg>

            <!-- 打叉：在协议下不能够进行调度 -->
            <svg class="symbol-style" v-if="!msrpSchedulable">
                <rect x="0" y="0" width="50" height="50" fill="red"></rect>
                <line x1="5" y1="5" x2="45" y2="45" stroke="black" stroke-width="3"></line>
                <line x1="5" y1="45" x2="45" y2="5" stroke="black" stroke-width="3"></line>
            </svg>

            <!-- 按钮：点击查看调度信息 -->
            <button class="button-style" v-bind:style="visibility(msrpSchedulable)" @click="sendProtocol('msrp')">查看调度信息</button>

        </div>

        <!-- MRSP 协议 -->
        <div class="item" ref="protocolMrsP">
            <span class="informationStyle"> MRSP 协议调度情况</span>
            <!-- 打勾：在该协议下能够进行调度 -->
            <svg class="symbol-style" v-if="mrspSchedulable">
                <rect x="0" y="0" width="50" height="50" fill="rgb(202, 249, 160)"></rect>
                <line x1="12" y1="25" x2="25" y2="45" stroke="black" stroke-width="3"></line>
                <line x1="25" y1="45" x2="45" y2="5" stroke="black" stroke-width="3"></line>
            </svg>

            <!-- 打叉：在协议下不能够进行调度 -->
            <svg class="symbol-style" v-if="!mrspSchedulable">
                <rect x="0" y="0" width="50" height="50" fill="red"></rect>
                <line x1="5" y1="5" x2="45" y2="45" stroke="black" stroke-width="3"></line>
                <line x1="5" y1="45" x2="45" y2="5" stroke="black" stroke-width="3"></line>
            </svg>

            <!-- 按钮：点击查看调度信息 -->
            <button class="button-style" v-bind:style="visibility(mrspSchedulable)" @click="sendProtocol('mrsp')">查看调度信息</button>

        </div>

        <!-- PWLP 协议 -->
        <div class="item" ref="protocolPWLP">
            <span class="informationStyle"> PWLP 协议调度情况</span>
            <!-- 打勾：在该协议下能够进行调度 -->
            <svg class="symbol-style">
                <rect x="0" y="0" width="50" height="50" fill="rgb(202, 249, 160)"></rect>
                <line x1="12" y1="25" x2="25" y2="45" stroke="black" stroke-width="3"></line>
                <line x1="25" y1="45" x2="45" y2="5" stroke="black" stroke-width="3"></line>
            </svg>

            <!-- 按钮：点击查看调度信息 -->
            <button class="button-style" v-bind:style="visibility(pwlpSchedulable)" @click="sendProtocol('pwlp')">查看调度信息</button>

        </div>
    </div>
</template>

<script>
    export default {
        name : 'IsSchedulableInformation',

        props : {
            "msrpSchedulable" : {
                type : Boolean,
                default : true
            },
            "mrspSchedulable" : {
                type : Boolean,
                default : false
            },
            "pwlpSchedulable" : {
                type : Boolean,
                default : true
            }
        },

        methods : {
            visibility : function(isSchedulable) {
                if (isSchedulable == false) {
                    return {
                        'visibility' : "hidden"
                    }
                }
                else {
                    return {

                    }
                }
            },
            sendProtocol : function(protocalName) {
                // 第一个参数是事件名称
                // 第二个参数是传递的参数，协议名称
                this.$bus.$emit('getGanttChart', protocalName)
            }
        }
    }
</script>

<style scoped>
    * {
        box-sizing:border-box;
    }

    .isSchedulableStyle {
        width: 487px;
        
        border-width: 2px;
        border-style: solid;
        border-color: rgb(208, 211, 212);

        text-align: center;
    }

    /* 
        1. 居中显示
    */
    .item {
        text-align: center;
        width : 487px;
        height: 50px;

        /* 块元素水平居中 */
        margin : 0 auto;

        margin-top: 10px;
        margin-bottom : 10px;
    }

    /* SYMBOL-STYLE: 符号对应的样式 */
    .symbol-style {

        /* 设置宽度和高度 */
        width: 50px;
        height:50px;

        /* 设置成块内元素 */
        display: inline-block;

        vertical-align: top;

        /* 与左边元素之间的宽度 */
        margin-left: 20px;
    }

    /* 字体样式 */
    .informationStyle {
        display: inline-block;
        /* 字体大小 */
        font-size: 19px;

        /* 设置高度 */
        height: 50px;

        /* 字体垂直方向上居中 */
        line-height: 50px;

        vertical-align: top;

        /* 字体粗细: 加粗 */
        font-weight: bold;
    }

    /* 按钮的样式设置 */
    /* .button-style {
        background-color : dodgerblue;
        color : white;
        width: 160px;
        height: 50px;
        border: 0;
        font-size: 19px;
        border-radius : 30px;

        margin-left: 20px;
    } */

    /* 按钮样式 */
    .button-style {
        display: inline-block;
        padding: 10px 20px;
        font-size: 19px;
        font-weight: bold;
        text-align: center;
        text-decoration: none;
        cursor: pointer;
        border: 2px solid rgb(95, 175, 255);
        border-radius: 15px;
        color: #ffffff;
        background-color: rgb(95, 175, 255);
        transition: background-color 0.3s, color 0.3s, border-color 0.3s;

        height: 50px;
        width: 160px;
        /* 水平方向上居中 */
        margin: 0 auto; 
        line-height: 0px;
        margin-left: 20px;
    }

    .button-style:hover {
        background-color: #ffffff;
        color: rgb(95, 175, 255);
        border-color: rgb(95, 175, 255);
    }
    
</style>