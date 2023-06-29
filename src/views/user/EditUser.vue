<template>
  <div style="width: 80%">
    <div style="margin-bottom: 30px">编辑用户</div>
    <el-form :inline="true" :model="form" label-width="100px">
      <el-form-item label="用户码">
        <el-input v-model="form.username" disabled></el-input>
      </el-form-item>
      <el-form-item label="姓名">
        <el-input v-model="form.name" placeholder="请输入姓名"></el-input>
      </el-form-item>
      <el-form-item label="年龄">
        <el-input v-model="form.age" placeholder="请输入年龄"></el-input>
      </el-form-item>
      <el-form-item label="性别">
        <el-input v-model="form.sex" placeholder="请输入性别"></el-input>
      </el-form-item>
      <el-form-item label="联系方式">
        <el-input v-model="form.phone" placeholder="请输入联系方式"></el-input>
      </el-form-item>
      <el-form-item label="学院">
        <el-input v-model="form.academy" placeholder="请输入学院"></el-input>
      </el-form-item>
      <el-form-item label="专业">
        <el-input v-model="form.speciality" placeholder="请输入专业"></el-input>
      </el-form-item>
      <el-form-item label="班级">
        <el-input v-model="form.classes" placeholder="请输入班级"></el-input>
      </el-form-item>
      <el-form-item label="地址" v-if="false">
        <el-input v-model="form.address" placeholder="请输入地址"></el-input>
      </el-form-item>
      <el-form-item label="角色">
        <el-select v-model="form.role" clearable placeholder="请选择角色">
          <el-option
                  v-for="item in options"
                  :key="item.value"
                  :label="item.label"
                  :value="item.value">
          </el-option>
        </el-select>
      </el-form-item>
      <el-form-item label="密码">
        <el-input v-model="form.password" placeholder="请输入密码"></el-input>
      </el-form-item>
    </el-form>

    <div style="text-align: center; margin-top: 30px">
      <el-button type="primary" @click="save" size="medium">提交</el-button>
    </div>
  </div>
</template>

<script>
import request from "@/utils/request";

export default {
  name: 'EditUser',
  data() {
    return {
      options: [],
      form: {}
    }
  },
  created() {
    const id = this.$route.query.id
      request.get('/role/list').then(res =>{
          this.options = res.data.map(item => {
              return {
                  label: item.rolename,
                  value: item.id
              }
          })
      })
    request.get("/user/" + id).then(res => {
      this.form = res.data
      this.form.password = null
    })
  },
  methods: {
    save() {
      request.put('/user/update', this.form).then(res => {
        if (res.code === '200') {
          this.$notify.success('更新成功')
          this.$router.push("/userList")
        } else {
          this.$notify.error(res.msg)
        }
      })
    },
    // selectrole(){
    //     request.get('/role/list').then(res =>{
    //         this.option = res.data
    //     })
    //     console.log(this.option)
    // }
  }
}

</script>

