<template>
    <!-- 
        模拟工具运行环境配置菜单。
     -->
    <div class="SystemSettingMenu-style" v-bind:style="calculateHeight">
        <!-- 标题 -->
        <div class="header-style">
            <span> 工具运行环境配置菜单 </span>
            <!-- ❌ -->
            <svg class="symbol-style" v-on:click="CloseSystemSettingMenu" v-on:mouseover="hoverSymbol" v-on:mouseout="resetSymbol">
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

        <!-- 设置 CPU 个数 -->
        <div class="option-style">
            <span style="margin-left:10px;"> CPU 个数 </span>
            <input type="number" v-model="tmpTotalCPUNum" class="input-style" v-bind:style="AdjustInputWidth(tmpTotalCPUNum, 140)" min="1"/>
        </div>

        <!-- 分割线 -->
        <hr style="border : 1px solid rgb(208, 211, 212); height : 1px;"/>

        <!-- CPU 平均分配个数 -->
        <div class="option-style">
            <span style="margin-left:10px;"> CPU 上平均分配任务个数 </span>
            <input type="number" v-model="tmpNumberOfTaskInAPartition" class="input-style" v-bind:style="AdjustInputWidth(tmpNumberOfTaskInAPartition, 140)" min="1"/>
        </div>

        <!-- 分割线 -->
        <hr style="border : 1px solid rgb(208, 211, 212); height : 1px;"/>

        <!-- 任务最短周期 -->
        <div class="option-style">
            <span style="margin-left:10px;"> 任务最短周期 </span>
            <input type="number" v-model="tmpMinPeriod" class="input-style" v-bind:style="AdjustInputWidth(tmpMinPeriod, 140)" min="10"/>
        </div>

        <!-- 分割线 -->
        <hr style="border : 1px solid rgb(208, 211, 212); height : 1px;"/>

        <!-- 任务最长周期 -->
        <div class="option-style">
            <span style="margin-left:10px;"> 任务最长周期 </span>
            <input type="number" v-model="tmpMaxPeriod" class="input-style" v-bind:style="AdjustInputWidth(tmpMaxPeriod, 140)" min="50"/>
        </div>

        <!-- 分割线 -->
        <hr style="border : 1px solid rgb(208, 211, 212); height : 1px;"/>

        <!-- 任务最大访问资源次数 -->
        <div class="option-style">
            <span style="margin-left:10px; width : 250px;"> 任务访问同一资源的最大次数 </span>
            <input type="number" v-model="tmpNumberOfMaxAccessToOneResource" class="input-style" v-bind:style="AdjustInputWidth(tmpNumberOfMaxAccessToOneResource, 110)" min="0"/>
        </div>

        <!-- 分割线 -->
        <hr style="border : 1px solid rgb(208, 211, 212); height : 1px;"/>

        <!-- 访问资源的任务比例 -->
        <div class="option-style">
            <span style="margin-left:10px;"> 访问资源的任务比例 </span>
            <input type="number" v-model="tmpResourceSharingFactor" class="input-style" style="width : 60px; margin-left : 80px;" min="0" max="1" step="0.1"/>
        </div>

        <!-- 分割线 -->
        <hr style="border : 1px solid rgb(208, 211, 212); height : 1px;"/>

        <!-- 任务最长周期 -->
        <div class="option-style">
            <span style="margin-left:10px;"> 访问资源类型 </span>
            <select class="select" v-model="tmpResourceType">
                <option value="VERY LONG LENGTH">Very Long Length</option>
                <option value="LONG LENGTH">Long Length</option>
                <option value="MEDIUM LENGTH">Medium Length</option>
                <option value="SHORT LENGTH">Short Length</option>
                <option value="VERY SHORT LENGTH">Very Short Length</option>
                <option value="RANDOM">Random</option>
            </select>
        </div>

        <!-- 分割线 -->
        <hr style="border : 1px solid rgb(208, 211, 212); height : 1px;"/>

        <!-- 资源的个数 -->
        <div class="option-style">
            <span style="margin-left:10px;"> 资源个数 </span>
            <select class="select" v-model="tmpResourceNum">
                <option value="HALF PARTITIONS">Half Partitions</option>
                <option value="PARTITIONS">Partitions</option>
                <option value="DOUBLE PARTITIONS">Double Partitions</option>
            </select>
        </div>

        <!-- 分割线 -->
        <hr style="border : 1px solid rgb(208, 211, 212); height : 1px;"/>

        <!-- 关键级切换选项 -->
        <div class="option-style">
            <span style="margin-left:10px;"> 关键级切换 </span>
            <SwitchButton style="display:inline-block; vertical-align:middle; margin-left:80px" :defaultChecked="true" v-model="tmpIsStartUpSwitch" :checkedInfo="'on'" :uncheckedInfo="'off'" :disabled="false"/>
        </div>

        <!-- 分割线 -->
        <hr style="border : 1px solid rgb(208, 211, 212); height : 1px;"/>


        <!-- 关键级自动切换选项 -->
        <div class="option-style" v-if="tmpIsStartUpSwitch">
            <span style="margin-left:10px;"> 关键级自动切换 </span>
            <SwitchButton style="display:inline-block; vertical-align:middle; margin-left:80px" :defaultChecked="true" v-model="tmpIsAutomaticallySwitch" :checkedInfo="'on'" :uncheckedInfo="'off'" :disabled="false"/>
        </div>

        <!-- 分割线 -->
        <hr style="border : 1px solid rgb(208, 211, 212); height : 1px;" v-if="tmpIsStartUpSwitch"/>

        <!-- 手动设置切换时间 -->
        <div class="option-style" v-if="tmpIsStartUpSwitch && !tmpIsAutomaticallySwitch">
            <span style="margin-left:10px;"> 设置关键级切换时间 </span>
            <input type="number" v-model="tmpCriticalitySwitchTime" class="input-style" v-bind:style="AdjustInputWidth(tmpCriticalitySwitchTime, 140)"/>
        </div>

        <!-- 分割线 -->
        <hr style="border : 1px solid rgb(208, 211, 212); height : 1px;" v-if="tmpIsStartUpSwitch && !tmpIsAutomaticallySwitch"/>

        <!-- 确认更改配置按钮 -->
        <button class="button-style" v-on:click="ClickTheAdjustSettingButton">确认更改配置</button>
    </div>
</template>

<script>
    import SwitchButton from './taskDetails/SwitchButton.vue';
    export default {
        name : "SystemSettingMenu",

        components  : {
            SwitchButton : SwitchButton
        },

        data : function() {
            return {
                // CPU 个数
                tmpTotalCPUNum : 2,

                // 平均一个 cpu 上分配几个任务
                tmpNumberOfTaskInAPartition : 2,

                // 任务最短周期
                tmpMinPeriod : 10,

                // 任务最长周期
                tmpMaxPeriod : 1000,

                // 任务最大访问资源次数
                tmpNumberOfMaxAccessToOneResource : 2,

                // 访问资源的任务比例
                tmpResourceSharingFactor : 0.50,

                // 访问的资源类型
                tmpResourceType : "SHORT LENGTH",

                // 资源个数
                tmpResourceNum : "PARTITIONS",

                // svg 元素的颜色
                circleAndLineColor : 'rgb(208, 211, 212)',

                tmpIsStartUpSwitch : false,
                tmpIsAutomaticallySwitch : true,
                tmpCriticalitySwitchTime : -1
            }
        },

        props : {
            // 
            totalCPUNum : {
                type : Number,
                default : 2
            }, 

            NumberOfTaskInAPartition : {
                type : Number,
                default : 2
            },

            minPeriod : {
                type : Number,
                default : 10
            },

            maxPeriod : {
                type : Number, 
                default : 1000
            }, 

            numberOfMaxAccessToOneResource : {
                type : Number,
                default : 2
            },

            resourceSharingFactor : {
                type : Number,
                default : 0.50
            },

            resourceType : {
                type : String,
                default : "SHORT LENGTH"
            },

            resourceNum : {
                type : String,
                default : "PARTITIONS"
            },

            isStartUpSwitch : {
                type : Boolean,
                default : false
            },

            isAutomaticallySwitch : {
                type : Boolean,
                default : true
            },

            criticalitySwitchTime : {
                type : Number,
                default : -1
            }
        },

         methods : {
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
                // 发布 WorstCaseSetting 事件
                this.$bus.$emit('WorstCaseSetting', this.sufferTaskId, this.resourceAccessProtocol, 
                    this.isStartUpSwitch, this.isAutomaticallySwitch, this.criticalitySwitchTime)

                // 关闭窗口
                this.$el.style.display = 'none'
            },

            // 关闭 SystemSettingMenu
            CloseSystemSettingMenu : function(){
                this.$el.style.display = 'none'

                // 恢复一下值
                this.tmpTotalCPUNum = this.totalCPUNum,
                this.tmpNumberOfTaskInAPartition = this.NumberOfTaskInAPartition
                this.tmpMinPeriod = this.minPeriod
                this.tmpMaxPeriod = this.maxPeriod
                this.tmpNumberOfMaxAccessToOneResource = this.numberOfMaxAccessToOneResource
                this.tmpResourceSharingFactor = this.resourceSharingFactor
                this.tmpResourceType = this.resourceType
                this.tmpResourceNum = this.resourceNum
                this.tmpIsStartUpSwitch = this.isStartUpSwitch
                this.tmpIsAutomaticallySwitch = this.isAutomaticallySwitch
                this.tmpCriticalitySwitchTime = this.criticalitySwitchTime
            },

            // 根据阻塞时间的大小调整输入框大小
            AdjustInputWidth : function(num, leftWidth) {
                let value = Math.abs(num)
                let i = 0;
                while (value >= 10) {
                    value = value / 10;
                    ++i;
                }

                return {
                    'width' : Math.min(100, 50 + i * 8) + 'px',
                    'margin-left' : leftWidth - Math.min(100, 50 + i * 8) + 'px'
                }
            },

            // 确认更改配置
            ClickTheAdjustSettingButton : function() {

                // 发布事件
                this.$bus.$emit('AdjustSystemSetting', Number(this.tmpTotalCPUNum), Number(this.tmpNumberOfTaskInAPartition), 
                        Number(this.tmpMinPeriod), Number(this.tmpMaxPeriod), Number(this.tmpNumberOfMaxAccessToOneResource), 
                        Number(this.tmpResourceSharingFactor), this.tmpResourceType, this.tmpResourceNum,
                        this.tmpIsStartUpSwitch, this.tmpIsAutomaticallySwitch, Number(this.tmpCriticalitySwitchTime))
                
                // 关闭窗口
                this.$el.style.display = 'none'
            }
         },

         mounted() {
            // 绑定自定义事件 --> 显示出模拟器配置菜单
            this.$bus.$on('ShowSystemSettingMenu', ()=>{
                this.$el.style.display = 'block'
            })
         },

         computed : {
            calculateHeight : function() {
                let baseHeight = 586 + 58
                if (this.tmpIsStartUpSwitch) {
                    baseHeight += 58;
                }

                if (this.tmpIsStartUpSwitch && !this.tmpIsAutomaticallySwitch) {
                    baseHeight += 58;
                }

                return {
                    'height' : baseHeight + 'px',
                    'display' : 'none'
                }
            }
         }
    }
</script>

<style scoped>
    * {
        box-sizing: border-box;
    }

    .SystemSettingMenu-style {
        background-color: white;
        position:absolute;
        width:380px;
        height:586px;
        border:3px rgb(208, 211, 212) solid;
        border-radius: 0px;
        left:1006px;
        top: 300px;

        display: none;
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

        width: 220px;
        height: 40px;

        line-height: 40px;
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
        stroke: rgb(66, 133, 244);
    }

    .option-style {
        width: 400px;
        height: 40px;
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

    .select {
        padding: 4px;
        font-size: 16px;
        border: 2px solid #ccc;
        border-radius: 10px;
        background-color: #fff;
        transition: border-color 0.3s ease-in-out, box-shadow 0.3s ease-in-out;
        
        height: 30px;
        width: 160px;
        margin-left : -20px;
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
</style>