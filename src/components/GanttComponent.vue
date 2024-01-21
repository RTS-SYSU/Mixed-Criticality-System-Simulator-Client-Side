<template>
    <!-- 
        先创建一个容器，容纳使用到的组件
    -->
    <div class="total-layout" v-bind:style="scaleSize">
        <!-- 
            功能区：
                1. IsSchedulable 组件：展示资源共享协议可调度性
                2. TaskTable 组件：展示任务信息
                3. ResourceTable 组件：展示资源信息
                4. SystemSettingMenu 组件：模拟工具运行环境配置菜单
                5. “模拟器开始运行” 按钮
         -->
        <div class="block-leftmost">
            <!-- 
                1. IsSchedulable 组件：展示资源共享协议可调度性

                IsSchedulable 组件需要三个数据：
                    1. msrpSchedulable ：MSRP资源共享协议的可调度性
                    2. mrspSchedulable ：MrsP资源共享协议的可调度性
                    3. pwlpSchedulable ：PWLP资源共享协议的可调度性
             -->
            <IsSchedulable v-bind:msrpSchedulable="msrpSchedulable" v-bind:mrspSchedulable="mrspSchedulable" v-bind:pwlpSchedulable="pwlpSchedulable"/>

            <!-- 
                2. TaskTable 组件：展示任务信息

                TaskTable 组件需要以下四个数据：
                    1. taskInformations: 任务信息
                    2. taskInformationLength: 任务个数
                    3. taskInformationPageNow: 当前页数（由于任务信息表格可能无法一次性显示所有任务，因此采用分页显示）
                    4. taskTableMaxItemSize: 任务信息表格一次最多显示的任务个数
            -->
            <TaskTable v-bind:task-informations="taskInformations"
                            v-bind:taskInformationPageNow="this.taskInformationPageNow"
                            v-bind:maxItemSize="this.taskInformationTableMaxItemSize"
                            v-bind:taskInformationTotalPageSize="this.taskInformationTotalPageSize"></TaskTable>

            <!-- 
                3. ResourceTable 组件：展示资源信息

                ResourceTable 组件需要以下四个数据：
                    1. resourceInformations: 资源信息
                    2. resourceInformationLength: 资源个数
                    3. resourceInformationPageNow: 当前页数（由于资源信息表格可能无法一次性显示所有资源，因此采用分页显示）
                    4. resourceITableMaxItemSize: 资源信息表格一次最多显示的资源个数
             -->
            <ResourceTable v-bind:resourceInformations="resourceInformations"
                            v-bind:taskInformationLength="this.taskInformations.length"
                            v-bind:taskInformationPageNow="this.taskInformationPageNow"
                            v-bind:taskTableMaxItemSize="this.taskInformationTableMaxItemSize"></ResourceTable>

            
            <!-- 
                4. SystemSettingMenu 组件：模拟工具运行环境配置菜单

                <button> 按钮用于控制 SystemSettingMenu 组件的显示：点击 button 按钮，SystemSettingMenu 出现；再点击一次，SystemSettingMenu 组件隐藏。
                TaskTable 组件需要以下数据：
                    1. totalCPUNum : 处理器核心个数
                    2. NumberOfTaskInAPartition : 处理器平均分配任务个数
                    3. minPeriod : 任务最短周期
                    4. maxPeriod : 任务最长周期
                    5. numberOfMaxAccessToOneResource : numberOfMaxAccessToOneResource
                    6. resourceSharingFacto : 访问资源的任务比例
                    7. resourceType : 访问资源类型
                    8. isStartUpSwitch : 是否开启关键级切换功能
                    9. isAutomaticallySwitch : 是否自动发生关键级切换
                    10. criticalitySwitchTime : 关键级切换发生的时间点
             -->
            <button class="button-style" v-on:click="ShowSystemSettingMenu()"> 配置工具运行参数 </button>
            <SystemSettingMenu v-bind:totalCPUNum="totalCPUNum" v-bind:NumberOfTaskInAPartition="NumberOfTaskInAPartition"
                            v-bind:minPeriod="minPeriod" v-bind:maxPeriod="maxPeriod" v-bind:numberOfMaxAccessToOneResource="numberOfMaxAccessToOneResource"
                            v-bind:resourceSharingFactor="resourceSharingFactor" v-bind:resourceType="resourceType"
                            v-bind:isStartUpSwitch="isStartUpSwitch"
                            v-bind:isAutomaticallySwitch="isAutomaticallySwitch"
                            v-bind:criticalitySwitchTime="criticalitySwitchTime"></SystemSettingMenu>


            <!-- 
                模拟器运行按钮：
                点击模拟一组新任务的执行
            -->
            <button class="button-style" v-on:click="StartSimulation()"> 开始运行 </button>
        </div>
        
        <!-- 
            甘特图区：
                1. cpuGantt 组件 : 绘制处理器甘特图
                2. taskGantt 组件 : 绘制任务甘特图
         -->
        <div class="gantt-chart">
            <!--
                1. cpuGantt 组件 : 绘制处理器甘特图

                cpuGantt 组件需要以下两个数据：
                        1. cpuGanttInformations : 处理器执行任务的状态以及事件
                        2. criticalitySwitchTime : 关键级切换发生的时间点
            -->
            <cpuGantt v-bind:cpu-gantt-informations="cpuGanttInformations" v-bind:criticality-switch-time="criticalitySwitchTime"/>

            <!--
                1. taskGantt 组件 : 绘制任务甘特图

                taskGantt 组件需要以下两个数据：
                        1. taskGanttInformations : 任务执行过程中的状态以及事件
                        2. criticalitySwitchTime : 关键级切换发生的时间点
            -->
            <taskGantt v-bind:task-gantt-informations="taskGanttInformations" v-bind:criticality-switch-time="criticalitySwitchTime"/>
        </div>

        <!--
            图例区: 
                1. GanttStateLegend : 显示甘特图中的颜色和符号的含义
        -->
        <div class="legend">
            <GanttStateLegend></GanttStateLegend>
        </div>
    </div>

</template>

<script>
    import GanttStateLegend from './GanttStateLegend.vue'
    import TaskTable from './taskDetails/TaskTable.vue'
    import cpuGantt from './cpuGantt/cpuGantt.vue'
    import taskGantt from './taskGantt/taskGantt.vue'
    import IsSchedulable from './IsSchedulable.vue'
    import SystemSettingMenu from './SystemSettingMenu.vue'
    import ResourceTable from './resourceDetails/ResourceTable.vue'

    // 在组件当中，data 必须为函数
    export default {
        name : 'GanttComponent',

        // 注册组件
        components: {
            cpuGantt : cpuGantt,
            taskGantt : taskGantt,
            GanttStateLegend: GanttStateLegend,
            TaskTable: TaskTable,
            IsSchedulable : IsSchedulable,
            SystemSettingMenu : SystemSettingMenu,
            ResourceTable : ResourceTable
        },


        data: function() {
            return {
                // 处理器甘特图:处理器执行任务的状态以及事件
                cpuGanttInformations : [
                    {
                        eventInformations : [
                        ],
                        eventTimePoints : [
                        ],
                        timeAxisLength : 30
                    }
                ],

                // 任务基本信息
                taskInformations : 
                    [
                    ],
                
                // 任务在执行过程中的状态以及触发的时间
                taskGanttInformations : 
                [
                    {
                        staticPid : "",
                        dynamicPid : "",
                        runningCPUCore : "",
                        taskGanttInformation : 
                        {
                            eventInformations : 
                            [   
                            ],
                            timeAxisLength : 30,
                            eventTimePoints : 
                            [
                            ]
                        }
                    }
                ],

                // 关键级切换发生的时间点
                criticalitySwitchTime : -1,

                // 用于窗口自适应
                setWindowSizeTimer : null,
                scaleSize : {
                    transform : "scale(1, 1)"
                },

                // MSRP 资源共享协议的可调度性
                msrpSchedulable : true,
                // MrsP 资源共享协议的可调度性
                mrspSchedulable : false,
                // PWLP 资源共享协议的可调度性
                pwlpSchedulable : true,

                // 是否开启关键级切换功能
                isStartUpSwitch : false, 
                // 关键级自动发生切换
                isAutomaticallySwitch : true,
                

                // 模拟器运行配置信息
                totalCPUNum : 2, 
                NumberOfTaskInAPartition : 2,
                minPeriod : 10,
                maxPeriod : 1000, 
                numberOfMaxAccessToOneResource : 2,
                resourceSharingFactor : 0.50,
                resourceType : "SHORT LENGTH",
                resourceNum : "PARTITIONS",

                // 显示资源信息
                resourceInformations : [
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
                ],

                // GanttTaskTable 所需信息
                // taskInformationPageNow: 当前页数（由于任务信息表格可能无法一次性显示所有任务，因此采用分页显示）
                taskInformationPageNow : 0,
                // taskTableMaxItemSize: 任务信息表格一次最多显示的任务个数
                taskInformationTableMaxItemSize : 7
            }
        },

        computed : {
            // 任务信息表格所需要的页数
            taskInformationTotalPageSize : function() {
                var ret = Math.ceil(this.taskInformations.length / this.taskInformationTableMaxItemSize)
                return ret;
            }
        },

        methods : {

            // 当浏览器的窗口大小发生变化时，调用该函数对 scale 进行修改
            ChangeScaling(browerWidth) {
                const baseWidth = 2465
                const zoomValue = browerWidth / baseWidth
                // 使用 this.$set 来更新属性以确保响应式更新
                this.scaleSize = {transform : 'scale(' + zoomValue + ',' + zoomValue + ')' }

            },

            // 浏览器窗口大小发生变化时调用该函数修改scale
            setWindowSize() {
                // 关闭上一个定时器以防止开太多定时器使得浏览器性能下降
                clearTimeout(this.setWindowSizeTimer)
                // 开启定时器
                this.style = this.setWindowSizeTimer = setTimeout(() => {
                    this.ChangeScaling(window.innerWidth, window.innerHeight)
                }, 200)
            },

            // 点击按钮显示模拟器配置菜单
            ShowSystemSettingMenu() {
                this.$bus.$emit('ShowSystemSettingMenu')
            },

            // 模拟器开始模拟一组新任务的执行
            StartSimulation() {
                if (this.isAutomaticallySwitch)
                    this.criticalitySwitchTime = -1
                // 系统运行一次
                this.$axios.post('http://localhost:8080/api/isSchedulable', {
                    totalCPUNum : this.totalCPUNum,
                    numberOfTaskInAPartition : this.NumberOfTaskInAPartition,
                    minPeriod : this.minPeriod,
                    maxPeriod : this.maxPeriod,
                    numberOfMaxAccessToOneResource : this.numberOfMaxAccessToOneResource,
                    resourceSharingFactor : this.resourceSharingFactor,
                    resourceType : this.resourceType,
                    resourceNum : this.resourceNum,
                    isStartUpSwitch : this.isStartUpSwitch,
                    criticalitySwitchTime : this.criticalitySwitchTime 

                }).then(
                    response => {
                        this.msrpSchedulable = response.data.msrpSchedulable
                        this.mrspSchedulable = response.data.mrspSchedulable
                        this.pwlpSchedulable = response.data.pwlpSchedulable
                        this.taskInformations = response.data.taskInformations
                        this.taskGanttInformations = response.data.taskGanttInformations
                        this.cpuGanttInformations = response.data.cpuGanttInformations
                        this.resourceInformations = response.data.resourceInformations
                    },
                    error => {
                        console.log('error', error.message)
                    }
                )
            }
        },

        mounted() {

            // 关闭 1.任务信息表格显示的访问资源以及访问资源的时间点 2. 资源信息表格显示的访问资源的所有任务
            window.onclick = function(event){
                // taskInformationTable 关闭相对应的信息
                // 获取目标 DOM 元素
                let displayBlocks = document.querySelectorAll('.resource-access-details')

                // 判断 event.target(触发的 DOM 元素) 是否是 access-resource-table 里面的内容
                let targetDOM = false;

                for (let i = 0; i < displayBlocks.length; ++i) {
                    if (displayBlocks[i].contains(event.target)) {
                        targetDOM = true;
                        break
                    }
                }

                if (!targetDOM){
                    for (let i = 0; i < displayBlocks.length; ++i) {
                        displayBlocks[i].style.display = "none"
                    }
                }

                // resourceInformationTable 关闭相对应的信息
                // 获取目标 DOM 元素
                displayBlocks = document.querySelectorAll('.task-access-details')

                // 判断 event.target(触发的 DOM 元素) 是否是 access-resource-table 里面的内容
                targetDOM = false;

                for (let i = 0; i < displayBlocks.length; ++i) {
                    if (displayBlocks[i].contains(event.target)) {
                        targetDOM = true;
                        break
                    }
                }

                if (!targetDOM){
                    for (let i = 0; i < displayBlocks.length; ++i) {
                        displayBlocks[i].style.display = "none"
                    }
                }
            }

            // 绑定事件-->获取对应协议下的任务执行情况
            this.$bus.$on('getGanttChart', (protocalName)=>{
                var address = `http://localhost:8080/api/${protocalName}?isStartUpSwitch=${this.isStartUpSwitch}&criticalitySwitchTime=${this.criticalitySwitchTime}` 
                this.$axios.post(address).then(
                    response => {
                        this.cpuGanttInformations = response.data.cpuGanttInformations
                        this.taskGanttInformations = response.data.taskGanttInformations
                        this.criticalitySwitchTime = response.data.criticalitySwitchTime
                    },
                    error => {
                        console.log('error', error.message)
                    }
                )
            })

            // 绑定自定义事件 --> 查看某一个任务的较差运行情况
            this.$bus.$on('WorstCaseSetting', (_sufferTaskId, _protocalName, _isStartUpSwitch, _isAutomaticallySwitch, _criticalitySwitchTime) => {
                if (_sufferTaskId < 0)
                    return 
                
                if (_isAutomaticallySwitch)
                    _criticalitySwitchTime = -1

                _protocalName = _protocalName.toLowerCase()

                var address = `http://localhost:8080/api/${_protocalName}WorstCase?staticPid=${_sufferTaskId}&isStartUpSwitch=${_isStartUpSwitch}&criticalitySwitchTime=${_criticalitySwitchTime}`
                
                if (_protocalName == 'mrsp') {
                    // MrsP 资源共享协议
                    this.$axios.post(address).then(
                        response => {
                            this.mrspSchedulable = response.data.schedulable
                            this.cpuGanttInformations = response.data.totalInformation.cpuGanttInformations
                            this.taskGanttInformations = response.data.totalInformation.taskGanttInformations
                            this.criticalitySwitchTime = response.data.totalInformation.criticalitySwitchTime
                        },
                        error => {
                            console.log('error', error.message)
                        }
                    )
                }else if (_protocalName == 'msrp')  {
                    // MSRP 资源共享协议
                    this.$axios.post(address).then(
                        response => {
                            this.msrpSchedulable = response.data.schedulable
                            this.cpuGanttInformations = response.data.totalInformation.cpuGanttInformations
                            this.taskGanttInformations = response.data.totalInformation.taskGanttInformations
                            this.criticalitySwitchTime = response.data.totalInformation.criticalitySwitchTime
                        },
                        error => {
                            console.log('error', error.message)
                        }
                    )
                }else if (_protocalName == 'pwlp')  {
                    // PWLP 资源共享协议
                    this.$axios.post(address).then(
                        response => {
                            this.pwlpSchedulable = response.data.schedulable
                            this.cpuGanttInformations = response.data.totalInformation.cpuGanttInformations
                            this.taskGanttInformations = response.data.totalInformation.taskGanttInformations
                            this.criticalitySwitchTime = response.data.totalInformation.criticalitySwitchTime
                        },
                        error => {
                            console.log('error', error.message)
                        }
                    )
                }
            })

            // 绑定自定义事件：--> 修改模拟器的运行配置
            this.$bus.$on('AdjustSystemSetting', (tmpTotalCPUNum, tmpNumberOfTaskInAPartition, tmpMinPeriod, tmpMaxiPeriod, 
                        tmpNumberOfMaxAccessToOneResource, tmpResourceSharingFactor, tmpResourceType, tmpResourceNum,
                        tmpIsStartUpSwitch, tmpIsAutomaticallySwitch, tmpCriticalitySwitchTime)=>{
                
                this.totalCPUNum = tmpTotalCPUNum
                this.NumberOfTaskInAPartition = tmpNumberOfTaskInAPartition
                this.minPeriod = tmpMinPeriod
                this.maxPeriod = tmpMaxiPeriod
                this.numberOfMaxAccessToOneResource = tmpNumberOfMaxAccessToOneResource
                this.numberOfTaskInAPartition = tmpNumberOfTaskInAPartition
                this.resourceSharingFactor = tmpResourceSharingFactor  
                this.resourceType = tmpResourceType
                this.resourceNum = tmpResourceNum
                this.isStartUpSwitch = tmpIsStartUpSwitch
                this.isAutomaticallySwitch = tmpIsAutomaticallySwitch
                this.criticalitySwitchTime = tmpCriticalitySwitchTime

                if (this.isAutomaticallySwitch)
                    this.criticalitySwitchTime = -1
            })

            // TaskTable 翻页
            this.$bus.$on('TaskTablePageDown', ()=>{
                this.taskInformationPageNow = Math.max(this.taskInformationPageNow - 1, 0)
            })
            // TaskTable 翻页
            this.$bus.$on('TaskTablePageUp', ()=>{
                this.taskInformationPageNow = Math.min(this.taskInformationPageNow + 1, this.taskInformationTotalPageSize - 1)
            })

            // 先让系统运行一次
            this.StartSimulation();
        },

        // 窗口自适应
        created() {
            window.addEventListener('resize', this.setWindowSize)
            this.ChangeScaling(window.innerWidth)
        },

        destroyed() {
            window.removeEventListener('resize', this.ChangeScaling)
        }

    }
</script>


<!-- 
    甘特图样式表的样式
    添加 scoped 表示
-->
<style scoped>
    * {
        box-sizing: border-box;
    }

    .total-layout {
        display:flex;

        /* 创建一个层叠上下文 */
        z-index: 0; 

        position: fixed;
        top: 0;
        left: 0;

        /* 设置对元素进行变换时，围绕哪个点进行变化 */
        transform-origin: 0 0;
    }

    .gantt-chart {
        box-sizing: content-box;
        width : 1500px;
        height: 100%;
        display: block;

        margin-left: 20px;
        margin-top : 20px;

        /* 创建层叠上下文：比task-information小 */
        z-index: 2;
    }

    /* 图例样式 */
    .legend {
        width: 400px;

        /* 设置其于左边元素之间的间隔 */
        margin-left: 25px;

        margin-top : 20px;

        height: 100%;

        display: block;
    }

    .block-leftmost {
        display : block;
        width : 487px;
        height: 100%;
        /* 创建层叠上下文 */
        z-index: 10;

        margin-top: 20px;
        margin-left: 20px;
    }

    /* 按钮样式 */
    .button-style {
        display: block;
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
        width: 250px;
        /* 水平方向上居中 */
        margin: 0 auto; 
        line-height: 0px;
        margin-top: 10px;
    }

    .button-style:hover {
        background-color: #ffffff;
        color: rgb(95, 175, 255);
        border-color: rgb(95, 175, 255);
    }
</style>