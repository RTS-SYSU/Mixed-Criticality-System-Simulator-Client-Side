<template>
    <!-- 
        大规模测试结果。
     -->
     <div class="TestSchedulabilityResult" v-bind:style="calculateHeight">
        <!-- 标题 -->
        <div class="header-style">
            <span> 可调度性结果 </span>
            <!-- ❌ -->
            <svg class="symbol-style" v-on:click="CloseTestSchedulabilityResultMenu" v-on:mouseover="hoverSymbol" v-on:mouseout="resetSymbol">
                    <!-- 绘制圆圈 -->
                    <circle cx="15" cy="15" r="15" v-bind:stroke="circleAndLineColor" stroke-width="2" fill="none"/>
                    <!-- 绘制直线 -->
                    <line x1="4" y1="4" x2="26" y2="26" v-bind:stroke="circleAndLineColor" fill="none"/>
                    <!-- 绘制直线 -->
                    <line x1="4" y1="26" x2="26" y2="4" v-bind:stroke="circleAndLineColor" fill="none"/>
            </svg>
        </div>

        <div class="table-style">
            <div class="row-style">
                <span class="font-style border-style"></span>
                <span class="font-style border-style">可调度数量</span>
                <span class="font-style border-style">测试数量</span>
                <span class="font-style border-style">可调度率</span>
            </div>
            <div class="row-style">
                <span class="font-style border-style">MSRP</span>
                <span class="font-style border-style">{{ msrpSchedulabilityNum }}</span>
                <span class="font-style border-style">{{ totalTestNum }}</span>
                <span class="font-style border-style">{{ (msrpSchedulabilityNum / totalTestNum * 100).toFixed(2) + "%" }}</span>
            </div>
            <div class="row-style">
                <span class="font-style border-style">MrsP</span>
                <span class="font-style border-style">{{ mrspSchedulabilityNum }}</span>
                <span class="font-style border-style">{{ totalTestNum }}</span>
                <span class="font-style border-style">{{ (mrspSchedulabilityNum / totalTestNum * 100).toFixed(2) + "%" }}</span>
            </div>
            <div class="row-style">
                <span class="font-style border-style">PWLP</span>
                <span class="font-style border-style">{{ pwlpSchedulabilityNum }}</span>
                <span class="font-style border-style">{{ totalTestNum }}</span>
                <span class="font-style border-style">{{ (pwlpSchedulabilityNum / totalTestNum * 100).toFixed(2) + "%" }}</span>
            </div>
            <div class="row-style">
                <span class="font-style border-style">动态资源共享协议</span>
                <span class="font-style border-style">{{ dynamicSchedulabilityNum }}</span>
                <span class="font-style border-style">{{ totalTestNum }}</span>
                <span class="font-style border-style">{{ (dynamicSchedulabilityNum / totalTestNum * 100).toFixed(2) + "%" }}</span>
            </div>
        </div>
    </div>
</template>

<script>
    export default {
        name : "TestSchedulabilityResult",

        data : function() {
            return {
                // svg 元素的颜色
                circleAndLineColor : 'rgb(208, 211, 212)'
            }
        },

        props : {
            msrpSchedulabilityNum : {
                type : Number,
                default : 1
            },
            mrspSchedulabilityNum : {
                type : Number,
                default : 1
            },
            pwlpSchedulabilityNum : {
                type : Number,
                default : 1
            },
            dynamicSchedulabilityNum : {
                type : Number,
                default : 1
            },
            totalTestNum : {
                type : Number,
                default : 1
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

            // 关闭 Test-Schedulability
            CloseTestSchedulabilityResultMenu : function() {
                this.$el.style.display = 'none'
                this.$bus.$emit('ShowTaskGantt')
            }
        },

        mounted() {
            // 绑定自定义事件 --> 显示出模拟器配置菜单
            this.$bus.$on('ShowTestSchedulabilityResultMenu', ()=>{
                this.$el.style.display = 'block'
            })

            // 绑定自定义事件 --> 显示出模拟器配置菜单
            this.$bus.$on('CloseTestSchedulabilityResultMenu', ()=>{
                this.$el.style.display = 'none'
            })
        },

        computed : {
            calculateHeight : function() {
                return {
                    'display' : 'none'
                }
            }
        }
    }
</script>

<style scoped>
    .TestSchedulabilityResult {
        background-color: white;
        position:absolute;
        width:500px;
        height:250px;
        border:2px rgb(208, 211, 212) solid;
        border-radius: 0px;
        left:1006px;
        top: 300px;
        display: block;
        box-sizing: content-box;
    }

    .table-style {
        width: 502px;
        height: 210px;
        border-width: 2px 0px 0px 0px;
        border-color: rgb(208, 211, 212);
        border-style: solid;
    }

    .row-style {
        display: grid;
        grid-template-columns: 190px 110px 100px 102px;
    }

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

    .header-style {
        text-align: center;
        width: 500px;
        height: 40px;
    }

    .header-style span {
        /* 字体加粗 */
        font-weight: bold;

        /* 调整字号 */
        font-size : 22px;

        /* 字体颜色 */
        color : rgb(110, 110, 110);

        width: 500px;
        height: 40px;

        line-height: 40px;
    }

    .header-style span {

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

    /* 绘制❌所需要的 CSS 样式 */
    .symbol-style {
        /* 设置宽度和高度 */
        width: 30px;
        height:30px;

        /* 设置成块内元素 */
        display: inline-block;

        position : absolute;

        left : 460px;
        top : 5px;

        cursor: pointer;
        transition: stroke 0.3s;
    }
</style>