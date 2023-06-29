<template>
  <div style="width: 80%">
    <div style="margin-bottom: 30px">编辑管理员</div>
    <el-form :inline="true" :model="form" label-width="100px">
      <el-form-item label="用户名" prop="username">
        <el-input v-model="form.username" placeholder="请输入姓名"></el-input>
      </el-form-item>
      <el-form-item label="联系方式" prop="phone">
        <el-input v-model="form.phone" placeholder="请输入联系方式"></el-input>
      </el-form-item>
      <el-form-item label="邮箱">
        <el-input v-model="form.email" placeholder="请输入地址"></el-input>
      </el-form-item>
      <el-form-item label="角色" v-if="role == 1">
        <el-select v-model="form.role" clearable placeholder="请选择角色">
          <el-option
                  v-for="item in options"
                  :key="item.value"
                  :label="item.label"
                  :value="item.value">
          </el-option>
        </el-select>
      </el-form-item>
    </el-form>

    <div style="text-align: center; margin-top: 30px">
      <el-button type="primary" @click="save" size="medium">提交</el-button>
    </div>
  </div>
</template>

<script>
import request from "@/utils/request";
import Cookies from 'js-cookie'

export default {
  name: 'EditAdmin',
  data() {
    return {
      role: " ",
      options: [],
      form: {}
    }
  },
  created() {
    const id = this.$route.query.id
    this.role = Cookies.get('role')
      request.get('/role/list').then(res =>{
          this.options = res.data.map(item => {
              return {
                  label: item.rolename,
                  value: item.id
              }
          })
      })
    request.get("/admin/" + id).then(res => {
      this.form = res.data
    })
  },
  methods: {
    save() {
      request.put('/admin/update', this.form).then(res => {
        if (res.code === '200') {
          this.$notify.success('更新成功')
          this.$router.push("/adminList")
        } else {
          this.$notify.error(res.msg)
        }
      })
    }
  }
}

</script>

