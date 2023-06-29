<template>
  <div style="height: 100vh; overflow: hidden; position: relative">
    <el-card class="cover" v-if="loginAdmin.id">
      <slide-verify :l="42"
                    :r="10"
                    :w="310"
                    :h="155"
                    :accuracy="5"
                    slider-text="向右滑动"
                    @success="onSuccess"
                    @fail="onFail"
                    @refresh="onRefresh"
      ></slide-verify>
    </el-card>

      <div style="width: 500px; height: 400px; background-color: white; border-radius: 10px;
        margin: 150px auto; padding:50px">
        <div style="margin: 30px; text-align: center; font-size: 30px; font-weight: bold; color: dodgerblue">登 录</div>
        <el-form :model="admin" :rules="rules" ref="loginForm">
          <el-form-item prop="username">
            <el-input placeholder="请输入账号" prefix-icon="el-icon-user" size="medium" v-model="admin.username"></el-input>
          </el-form-item>
          <el-form-item prop="password">
            <el-input placeholder="请输入密码" show-password prefix-icon="el-icon-lock" size="medium" v-model="admin.password"></el-input>
          </el-form-item>
          <el-form-item>
            <el-button style="width: 100%" size="medium" type="primary" @click="login">登录</el-button>
          </el-form-item>
          <el-form-item label="角色">
            <el-select v-model="value" clearable placeholder="请选择角色">
              <el-option
                      v-for="item in options"
                      :key="item.value"
                      :label="item.label"
                      :value="item.value">
              </el-option>
            </el-select>
          </el-form-item>
        </el-form>
      </div>
  </div>
</template>

<script>
import request from "@/utils/request";
import Cookies from 'js-cookie'
import jwt_decode from 'jwt-decode'

export default {
    name: 'LOGIN',
    data() {
        return {
            value: '',
            options: [{
                label: '管理员',
                value: '1'
            },{
                label: '用户',
                value: '2'
            }],
            loginAdmin: {},
            admin: {},
            rules: {
                username: [
                    { required: true, message: '请输入用户名', trigger: 'blur'},
                    { min: 3, max: 10, message: '长度在3-10个字符', trigger: 'blur'}
                ],
                password: [
                    { required: true, message: '请输入密码', trigger: 'blur'},
                    { min: 3, max: 10, message: '长度在3-10个字符', trigger: 'blur'}
                ]
            }
        }
    },
  methods: {
    login() {
      this.$refs['loginForm'].validate((valid) => {
        if (valid) {
            if(this.value ==1){
                request.post('/admin/login',this.admin).then(res => {
                    if (res.code === '200') {
                        this.loginAdmin = res.data  // 滑块组件就出现了
                    } else {
                        this.$notify.error(res.msg)
                    }
                })
            }else if (this.value == 2) {
                request.post('/user/login',this.admin).then(res => {
                    if (res.code === '200') {
                        this.loginAdmin = res.data  // 滑块组件就出现了
                    } else {
                        this.$notify.error(res.msg)
                    }
                })
            }
        }
      })
    },
    onSuccess() { // 滑块验证通过之后触发的
      const decoded = jwt_decode(this.loginAdmin.token)
      Cookies.set("role",decoded.role)
      Cookies.set("roles",decoded.roles)
      Cookies.set('token', JSON.stringify(this.loginAdmin))
      this.$notify.success("登录成功")
      this.$router.push('/')
    },
    onFail() {
      console.log('onFail')
    },
    onRefresh() {
      console.log('refresh')
    }
  }
}
</script>

<style>
.cover {
  width: fit-content;
  background-color: white;
  position: absolute;
  top:50%;
  left:50%;
  transform: translate(-50%, -50%);
  z-index: 1000;
}
</style>