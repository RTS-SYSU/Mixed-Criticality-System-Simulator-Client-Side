<template>
  <!-- <div id="app">
    <img alt="Vue logo" src="./assets/logo.png">
    <HelloWorld msg="Welcome to Your Vue.js App"/>
  </div> -->
  <div id="mainContainer" style="overflow: auto;">
    <GanttComponent/>
  </div>
</template>

<script>
import GanttComponent from './components/GanttComponent.vue'

export default {
  name: 'App',
  components: {
    GanttComponent : GanttComponent
  },

  // // // 根据页面大小进行等比例缩放
  // mounted() {
  //   var browerWidth = window.innerWidth; // 浏览器可视宽度
  //   var baseWidth = 2560; //设计稿宽度

  //   var zoomValueWidth = browerWidth / baseWidth; // 水平缩放比例计算

  //   // scale 函数：scale(水平方向上的缩放比例，竖直方向上的缩放比例)
  //   document.getElementById("mainContainer").style.transform = "scale(" + zoomValueWidth + "," + zoomValueWidth + ")"; // mainContainer 为主容器的id
    
  //   // window.onresize = function() { // 窗口尺寸变化时，重新计算和缩放
  //   //   browerWidth = window.innerWidth;
  //   //   browerHeight = window.innerHeight;

  //   //   zoomValueWidth = browerWidth / baseWidth;
  //   //   zoomValueHeight = browerHeight / baseHeight
  //   //   document.getElementById("mainContainer").style.transform = "scale(" + zoomValueWidth + "," + zoomValueHeight + ")"; // mainContainer 为主容器的id
  //   // }

  // }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
