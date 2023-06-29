<template>
  <div>
    <!-- 头部区域 -->
    <div style="height: 70px; line-height: 70px; background-color: white; margin-bottom: 2px; display: flex">
      <div style="width: 500px">
        <img src="@/assets/logo.png" alt="" style="width: 50px; position: relative; top: 10px; left: 20px;">
        <span style="margin-left: 30px; font-size: 24px;" >攀枝花学院图书管理系统</span>
      </div>
      <div style="flex: 1; text-align: right; padding-right: 20px">
        <el-dropdown size="medium">
          <span class="el-dropdown-link" style="cursor: pointer">
            {{ admin.username }}<i class="el-icon-arrow-down el-icon--right"></i>
          </span>
          <el-dropdown-menu slot="dropdown" style="margin-top: -5px">
            <el-dropdown-item><div style="width: 50px; text-align: center;" @click="logout">退出</div></el-dropdown-item>
          </el-dropdown-menu>
        </el-dropdown>
      </div>
    </div>

    <!-- 侧边栏和主体 -->
    <div style="display: flex">
      <!-- 侧边栏导航 -->
      <div style="width: 200px; min-height: calc(100vh - 62px); overflow: hidden; margin-right: 2px; background-color: white">
        <el-menu :default-active="$route.path" router class="el-menu-demo" style="margin-bottom: 10px">
          <el-menu-item index="/">
            <i class="el-icon-eleme"></i>
            <span>首页</span>
          </el-menu-item>
          <el-submenu index="user" v-if="hs.includes('user')" >
            <template slot="title">
              <i class="el-icon-question"></i>
              <span>用户管理</span>
            </template>
            <el-menu-item index="/addUser" v-if="hs.includes('/addUser')">用户添加</el-menu-item>
            <el-menu-item index="/userList" v-if="hs.includes('/userList')">用户列表</el-menu-item>
          </el-submenu>
          <el-submenu index="admin" v-if="hs.includes('admin')">
            <template slot="title">
              <i class="el-icon-user"></i>
              <span>管理员管理</span>
            </template>
            <el-menu-item index="/addAdmin" v-if="hs.includes('/addAdmin')">管理员添加</el-menu-item>
            <el-menu-item index="/adminList" v-if="hs.includes('/adminList')">管理员列表</el-menu-item>
          </el-submenu>
          <el-submenu index="authority"  v-if="hs.includes('authority')">
            <template slot="title">
              <i class="el-icon-warning"></i>
              <span>权限管理</span>
            </template>
            <el-menu-item index="/roleList"  v-if="hs.includes('/roleList')">角色权限管理</el-menu-item>
          </el-submenu>
          <el-submenu index="category"  v-if="hs.includes('category')">
            <template slot="title">
              <i class="el-icon-s-operation"></i>
              <span>图书分类管理</span>
            </template>
            <el-menu-item index="/addCategory"  v-if="hs.includes('/addCategory')">图书分类添加</el-menu-item>
            <el-menu-item index="/categoryList"  v-if="hs.includes('/categoryList')">图书分类列表</el-menu-item>
          </el-submenu>
          <el-submenu index="book"  v-if="hs.includes('book')">
            <template slot="title">
              <i class="el-icon-notebook-1"></i>
              <span>图书管理</span>
            </template>
            <el-menu-item index="/addBook"  v-if="hs.includes('/addBook')">图书添加</el-menu-item>
            <el-menu-item index="/bookList"  v-if="hs.includes('/bookList')">图书列表</el-menu-item>
            <el-menu-item index="/listAdd" v-if="hs.includes('/listAdd')" >批量添加</el-menu-item>
          </el-submenu>
          <el-submenu index="borrow"  v-if="hs.includes('borrow')">
            <template slot="title">
              <i class="el-icon-document-copy"></i>
              <span>借书管理</span>
            </template>
            <el-menu-item index="/addBorrow"  v-if="hs.includes('/addBorrow')">借书添加</el-menu-item>
            <el-menu-item index="/borrowList"  v-if="hs.includes('/borrowList')">借书列表</el-menu-item>
          </el-submenu>
          <el-submenu index="retur" v-if="hs.includes('retur')">
            <template slot="title">
              <i class="el-icon-document"></i>
              <span>还书管理</span>
            </template>
            <el-menu-item index="/returList"  v-if="hs.includes('/returList')">还书列表</el-menu-item>
          </el-submenu>
        </el-menu>
      </div>

      <!-- 主体数据 -->
      <!-- width: 0; 可以限制容器的宽度，不被子元素无限撑开-->
      <div style="flex: 1; width: 0; background-color: white; padding: 10px">
        <router-view />
      </div>
    </div>
  </div>
</template>

<script>
import Cookies from 'js-cookie'

export default {
  name: "Layout.vue",
  data() {
    return {
      hs: [],
      admin: Cookies.get('token') ? JSON.parse(Cookies.get('token')) : {}
    }
  },
    created(to, from, next){
        const hsting = Cookies.get('roles')
        console.log(hsting)
         this.hs = hsting.split(',')
         console.log(this.hs)
    },
  methods: {
    logout() {
      // 清除浏览器用户数据
      Cookies.remove('token')
      Cookies.remove('role')
      Cookies.remove('roles')
      this.$router.push('/login')
    },
     // load(){
     //     const hsting = Cookies.get('roles')
     //     console.log(hsting)
     //     this.hs = hsting.split('%2c')
     //     console.log(this.hs)
     // }
}
}
</script>

<style scoped>

</style>