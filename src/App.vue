<template>
  <div id="app">
    <!--头部-->
    <div style="height: 60px; line-height: 60px; background-color: white">
      <img src="@/assets/logo-head.png" alt="" style="width: 40px; position: relative; top: 10px; left: 10px">
      <span style="margin-left: 20px; font-size: 24px;">图书管理系统</span>
    </div>

    <!--侧边栏主体-->
    <div style="display: flex">
      <!--侧边栏导航-->
      <div style="width: 200px; overflow: hidden; margin-right: 2px; background-color: white">
        <el-menu :default-active="$route.path" class="el-menu-demo">
          <el-menu-item index="1">处理中心</el-menu-item>
          <el-submenu index="2">
            <template slot="title">我的工作台</template>
            <el-menu-item index="2-1">选项1</el-menu-item>
            <el-menu-item index="2-2">选项2</el-menu-item>
          </el-submenu>
        </el-menu>
      </div>

      <!--主体数据-->
      <div><el-button type="primary">这是一个按钮</el-button></div>
    </div>

    <router-view/>
  </div>
</template>

