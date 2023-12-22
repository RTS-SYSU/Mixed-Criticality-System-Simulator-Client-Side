# 混合关键系统模拟工具简介

在华为胡杨林项目的支持下，中山大学计算机学院RTS实验室研发了一款面向复杂混合关键实时系统资源共享协议的模拟工具。该模拟工具能够帮助用户全面评估不同条件下资源共享协议的性能表现、对比不同资源共享协议的优缺点、验证资源共享协议的正确性。这款工具提供以下三个功能：

- 以可视化形式展示任务全周期运行情况，即在甘特图上使用不同的颜色和符号来表示任务的状态（独立执行、等待资源、访问资源等）。这个功能有助于用户更好地理解指定资源共享协议运行规则，便捷对比不同资源共享协议的优缺点、深入了解系统任务之间的相互影响关系、及时发现系统潜在问题，如死锁和饥饿。
- 模拟指定任务最坏运行情况，即模拟任务的极端执行情况。通过模拟任务的极端执行情况，用户可以深入了解任务在最不利情况下的运行状况，有助于验证系统任务的响应时间，定位系统可调度性瓶颈并为资源共享协议调优提供关键信息。
- 调整系统运行参数。这项功能使用户能够模拟各种工作负载和资源配置，从而全面评估不同条件下资源共享协议的性能表现。



# 前端实现技术栈

混合关键系统模拟工具选用前后端分离的开发模式进行构建。本仓库存储前端代码，后端仓库地址为[RTS-SYSU/Mixed-Criticality-System-Simulator-Serve-Side (github.com)](https://github.com/RTS-SYSU/Mixed-Criticality-System-Simulator-Serve-Side)。混合关键系统模拟工具选用Vue.js和Electron来构建前端界面。在前端开发中，首先利用Vue.js开发出用户友好的交互性Web界面，接着通过Electron将Web界面打包成可在移动端平台上运行的应用程序。



# 前端代码文件夹说明

在`Mixed-Criticality-System-Simulator-Client-Side`文件夹中，`src/components`存放前端界面代码，其他内容为Vue项目自带文件。`src/components`文件夹中的内容具体如下：

- `GanttComponent.vue`合并所有组件，用于展示模拟工具的运行界面。
- `GanttStateLegend.vue`用于呈现运行界面中的图例。
- `IsSchedulable.vue`用于展示资源共享协议可调度性。
- `SystemSettingMenu.vue`用于调整模拟工具运行环境参数。
- `cpuGantt`文件夹用于绘制处理器甘特图。
- `resourceDetails`文件夹用于呈现资源信息。
- `taskDetails`文件夹用于呈现任务信息和查看指定任务的最坏运行情况。
- `taskGantt`文件夹用于绘制任务甘特图。



# 前端代码编写环境

混合关键系统模拟工具前端界面的开发中使用了Node.js、Vue.js、vue/cli、axios和electron库。在Node.js环境中，通过npm包管理器管理Vue项目开发所需的库，包括axios用于前后端通信、electron用于将Vue项目的Web页面打包成移动端程序界面，以及vue/cli用于快速创建和有效管理Vue项目。以下是所使用库的版本信息：

- Node.js : v16.20.2
- Vue.js : 2.6.14
- vue/cli : 5.0.0
- axios : 1.5.0
- electron : 12.2.3

# 前端代码运行指南

前端代码的运行指南如下：

- 从本仓库下载代码至本地。
- 进入`Mixed-Criticality-System-Simulator-Client-Side`文件夹。
- 在控制台中运行`npm install`命令安装依赖项。
- 在控制台中运行`npm run serve`命令运行代码。
- 点击控制台中显示的网址即可访问混合关键系统模拟工具的前端界面。
