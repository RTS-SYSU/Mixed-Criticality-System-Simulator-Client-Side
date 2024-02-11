<template>
    <!-- 
        最坏运行情况配置菜单
     -->
    <div class="WorstCaseSettingMenu-style" v-bind:style="WorCaseSettingMenuStyle">
        <!-- 标题 -->
        <div class="header-style">
            <span> 最坏运行情况配置菜单 </span>
            <!-- ❌ -->
            <svg class="symbol-style" v-on:click="CloseWorstCaseSettingMenu" v-on:mouseover="hoverSymbol" v-on:mouseout="resetSymbol">
                    <!-- 绘制圆圈 -->
                    <circle cx="15" cy="15" r="15" v-bind:stroke="circleAndLineColor" stroke-width="2" fill="none"/>
                    <!-- 绘制直线 -->
                    <line x1="4" y1="4" x2="26" y2="26" v-bind:stroke="circleAndLineColor" fill="none"/>
                    <!-- 绘制直线 -->
                    <line x1="4" y1="26" x2="26" y2="4" v-bind:stroke="circleAndLineColor" fill="none"/>
            </svg>
        </div>
    
        <!-- 分割线 -->
        <hr style="border : 1px solid rgb(208, 211, 212); height : 1px;"/>

        <!-- 显示任务的id -->
        <div class="option-style">
            <span style="margin-left:10px;"> 任务 Task id </span>
            <span style="margin-left: 150px; width : 30px">{{ this.sufferTaskId }}</span>
        </div>

        <!-- 分割线 -->
        <hr style="border : 1px solid rgb(208, 211, 212); height : 1px;"/>

         <!-- 协议选择 -->
         <div class="option-style">
            <span style="margin-left:10px;"> 资源共享协议 </span>
            <select class="select" v-model="resourceAccessProtocol">
                <option value="MSRP">MSRP</option>
                <option value="MRSP">MRSP</option>
                <option value="PWLP">PWLP</option>
            </select>
        </div>

        <!-- 分割线 -->
        <hr style="border : 1px solid rgb(208, 211, 212); height : 1px;"/>

        <!-- 关键级切换选项 -->
        <div class="option-style">
            <span style="margin-left:10px;"> 关键级切换 </span>
            <SwitchButton style="display:inline-block; vertical-align:middle; margin-left:130px" :defaultChecked="true" v-model="isStartUpSwitch" :checkedInfo="'on'" :uncheckedInfo="'off'" :disabled="false"/>
        </div>

        <!-- 分割线 -->
        <hr style="border : 1px solid rgb(208, 211, 212); height : 1px;"/>


        <!-- 关键级自动切换选项 -->
        <div class="option-style" v-if="isStartUpSwitch">
            <span style="margin-left:10px;"> 关键级自动切换 </span>
            <SwitchButton style="display:inline-block; vertical-align:middle; margin-left:130px" :defaultChecked="true" v-model="isAutomaticallySwitch" :checkedInfo="'on'" :uncheckedInfo="'off'" :disabled="false"/>
        </div>

        <!-- 分割线 -->
        <hr style="border : 1px solid rgb(208, 211, 212); height : 1px;" v-if="isStartUpSwitch"/>

        <!-- 手动设置切换时间 -->
        <div class="option-style" v-if="isStartUpSwitch && !isAutomaticallySwitch">
            <span style="margin-left:10px;"> 设置关键级切换时间 </span>
            <input type="number" v-model="criticalitySwitchTime" class="input-style" v-bind:style="AdjustInputWidth"/>
        </div>

        <!-- 分割线 -->
        <hr style="border : 1px solid rgb(208, 211, 212); height : 1px;" v-if="isStartUpSwitch && !isAutomaticallySwitch"/>

        <!-- 查看某一个任务的较差运行情况 -->
        <button class="button-style" v-on:click="ClickTheWorstCaseButton">查看最坏运行情况</button>
    </div>
</template>

<script>
import SwitchButton from './SwitchButton.vue'
    export default {
        name : "WorstCaseSettingMenu",
        
        components: { 
            SwitchButton : SwitchButton 
        },

        props : {
            // 要查看的任务的静态 id
            sufferTaskId : {
                type : Number,
                default : -1
            }
        },

        data : function() {
            return {
                // 是否开启关键级切换
                isStartUpSwitch : false,

                // 关键级自动切换
                isAutomaticallySwitch : true,

                // 手动设置关键级切换时间
                criticalitySwitchTime : -1,

                // 选择的资源访问协议
                resourceAccessProtocol : "MRSP",

                // svg 元素的颜色
                circleAndLineColor : 'rgb(208, 211, 212)'
            }
        },

        // 计算属性
        computed : {
            // 阻塞界面的设置
            WorCaseSettingMenuStyle : function() {
                var baseHeight = 10 + 58 + 58 + 58 + 58 + 54; 

                if (this.isStartUpSwitch) {
                    baseHeight += 58;
                }

                if (this.isStartUpSwitch && !this.isAutomaticallySwitch) {
                    baseHeight += 58;
                }

                return {
                    'height' : baseHeight + 'px',
                    'display' : 'none'
                }
            },

            // 根据阻塞时间的大小调整输入框大小
            AdjustInputWidth : function() {
                let value = Math.abs(this.criticalitySwitchTime)
                let i = 0;
                while (value > 10) {
                    value = value / 10;
                    ++i;
                }
                return {
                    'width' : Math.min(180, 60 + i * 5) + 'px',
                    'margin-left' : 180 - Math.min(180, 60 + i * 5) + 'px'
                }
            }
        },

        methods : {
            // 关闭 worst-case-setting-menu
            CloseWorstCaseSettingMenu : function() {
                this.$el.style.display = 'none'
            },

            hoverSymbol() {
                // 鼠标移入时改变颜色
                this.circleAndLineColor = 'rgb(66, 133, 244)';
            },

            resetSymbol() {
                // 鼠标移出时重置颜色
                this.circleAndLineColor = 'rgb(208, 211, 212)';
            },

            // 点击 “查看该任务的较差运行情况”
            ClickTheWorstCaseButton : function() {
                if (this.criticalitySwitchTime >= 0) {
                    // 发布 WorstCaseSetting 事件
                    this.$bus.$emit('WorstCaseSetting', this.sufferTaskId, this.resourceAccessProtocol, this.isStartUpSwitch, this.isAutomaticallySwitch, this.criticalitySwitchTime)
                    // 关闭窗口
                    this.$el.style.display = 'none'
                }else {
                    alert('关键级切换时间必须大于或等于0！')
                    this.criticalitySwitchTime = 0
                }
            }
        }
    }
</script>

<style scoped>
    * {
        box-sizing: border-box;
    }

    .WorstCaseSettingMenu-style {
        background-color: white;
        position:absolute;
        width:380px;
        height:500px;
        border:3px rgb(208, 211, 212) solid;
        border-radius: 0px;
        left:1006px;
        top: 300px;
    }

    .header-style {
        text-align: center;
        line-height: 40px;
        width: 380px;
        height: 40px;

        margin-top: 10px;
    }

    .header-style span {
        /* 字体加粗 */
        font-weight: bold;

        /* 调整字号 */
        font-size : 22px;

        /* 字体颜色 */
        color : rgb(110, 110, 110);

        width: 380px;
        height: 40px;
    }

    span {

        display: inline-block;

        vertical-align: middle;

        /* 字体加粗 */
        font-weight: bold;

        /* 调整字号 */
        font-size : 19px;

        /* 字体颜色 */
        color : rgb(110, 110, 110);

        width: 180px;
        height: 40px;

        line-height: 40px;
    }

    .option-style {
        width: 400px;
        height: 40px;
        display: flex;
        align-items: center;
    }

    .input-style {
        padding: 8px;
        border: 2px solid #ccc;
        border-radius: 8px;
        transition: border-color 0.3s ease-in-out;

        height: 35px;
        width: 60px;
        font-size: 16px;

        vertical-align: middle;
        line-height: 40px;
        
        margin-left : 120px;
    }

    .input-style:focus {
        outline: none;
        border-color: #66afe9;
        box-shadow: 0 0 5px rgba(102, 175, 233, 0.6);
    }

    /* 绘制❌所需要的 CSS 样式 */
    .symbol-style {
        /* 设置宽度和高度 */
        width: 30px;
        height:30px;

        /* 设置成块内元素 */
        display: inline-block;

        position : absolute;

        left : 340px;
        top : 5px;

        cursor: pointer;
        transition: stroke 0.3s;
    }

    .symbol-style:hover {
        stroke: rgb(95, 175, 255);
    }

    .select {
        padding: 4px;
        font-size: 16px;
        border: 2px solid #ccc;
        border-radius: 10px;
        background-color: #fff;
        transition: border-color 0.3s ease-in-out, box-shadow 0.3s ease-in-out;
        
        height: 30px;
        width: 77px;
        margin-left : 103px;
    }

    .select:focus {
        outline: none;
        border-color: rgb(95, 175, 255);
        box-shadow: 0 0 10px rgba(102, 175, 233, 0.6);
    }

    .selected-option {
        margin-top: 10px;
        font-weight: bold;
        color: #333;
        transition: color 0.3s ease-in-out;
    }

    .selected-option:hover {
        color: #007bff;
    }

    /* 按钮样式 */
    .button-style {
        display: block;
        padding: 10px 20px;
        font-size: 16px;
        font-weight: bold;
        text-align: center;
        text-decoration: none;
        cursor: pointer;
        border: 2px solid rgb(95, 175, 255);
        border-radius: 15px;
        color: #ffffff;
        background-color: rgb(95, 175, 255);
        transition: background-color 0.3s, color 0.3s, border-color 0.3s;

        height: 40px;
        /* 水平方向上居中 */
        margin: 0 auto; 
        line-height: 0px;
    }

    .button-style:hover {
        background-color: #ffffff;
        color: rgb(95, 175, 255);
        border-color: rgb(95, 175, 255);
    }
</style>