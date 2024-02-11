<template>
    <!-- 
        查看历史纪录菜单
     -->
    <div class="HistoryMenu-style" v-bind:style="HistoryMenuStyle">
        <!-- 标题 -->
        <div class="header-style">
            <span> 查看历史纪录菜单 </span>
            <!-- ❌ -->
            <svg class="symbol-style" v-on:click="CloseHistoryMenu" v-on:mouseover="hoverSymbol" v-on:mouseout="resetSymbol">
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

         <!-- 历史记录选择 -->
         <div class="option-style">
            <span style="margin-left:10px;"> 历史记录： </span>
            <select class="select" v-model="selectedHistoryRecord">
                <option v-for="(item ,index) in HistoryRecords" v-bind:key="index" v-bind:value="item">{{ item }}</option>
            </select>
        </div>

        <!-- 分割线 -->
        <hr style="border : 1px solid rgb(208, 211, 212); height : 1px;"/>

        <!-- 查看某一个任务的较差运行情况 -->
        <button class="button-style" v-on:click="ClickImportHistoryButton">导入历史记录</button>
    </div>
</template>

<script>
    export default {
        name : "HistoryMenu",
    

        props : {
            // 要查看的任务的静态 id
            "HistoryRecords" : {
                type : Array,
                default : function() {
                    return ["Empty History Records!"]
                }
            }
        },

        data : function() {
            return {
                // svg 元素的颜色
                circleAndLineColor : 'rgb(208, 211, 212)',

                // 选择的历史记录
                selectedHistoryRecord : null
            }
        },

        // 计算属性
        computed : {
            // 查看历史记录界面的设置
            HistoryMenuStyle : function() {
                return {
                    'display' : 'none'
                }
            }
        },

        methods : {
            // 关闭 worst-case-setting-menu
            CloseHistoryMenu : function() {
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

            // 点击 “导入历史记录”
            ClickImportHistoryButton : function() {
                if (this.selectedHistoryRecord != null) {
                    // 关闭窗口
                    this.$el.style.display = 'none'
                    // 更新当前系统的配置
                    this.$bus.$emit('ImportHistory', this.selectedHistoryRecord)
                }else if (this.selectedHistoryRecord == null && this.HistoryRecords.length > 0){
                    alert("错误：空历史记录!")
                }
            }
        },

        mounted() {
            // 绑定自定义事件 --> 显示出历史记录菜单
            this.$bus.$on('ShowHistoryMenu', ()=>{
                this.$el.style.display = 'block'
            })

        }
    }
</script>

<style scoped>
    * {
        box-sizing: border-box;
    }

    .HistoryMenu-style {
        background-color: white;
        position:absolute;
        width:380px;
        height:180px;
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

        width: 100px;
        height: 40px;

        line-height: 40px;
    }

    .option-style {
        width: 400px;
        height: 40px;

        display: flex;
        align-items: center;
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
        width: 230px;
        margin-left : 20px;
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