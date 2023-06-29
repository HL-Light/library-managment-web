<template>
 <div>
  <el-checkbox :indeterminate="isIndeterminate" v-model="checkAll" @change="handleCheckAllChange">全选</el-checkbox>
  <div style="margin: 15px 0;"></div>
  <el-checkbox-group v-model="selectedValues" @change="handleCheckedCitiesChange">
   <el-checkbox v-for="option in options" :label="option.value" :key="option.value">{{ option.label}}</el-checkbox>
  </el-checkbox-group>

  <div style="text-align: center; margin-top: 30px">
   <el-button type="primary" @click="save" size="medium">提交</el-button>
  </div>
 </div>
</template>

<script>
import request from "@/utils/request";

export default {
  name: 'RoleEdit',
  data() {
    return {
      id: '',
      checkAll: false,
      isIndeterminate: true,
      options: [
            { label: '学生管理权限', value: 'user' },
            { label: '学生添加', value: '/addUser' },
            { label: '学生列表', value: '/userList' },
            { label: '管理员管理', value: 'admin' },
            { label: '管理员添加', value: '/addAdmin' },
            { label: '管理员列表', value: '/adminList' },
            { label: '权限管理', value: 'authority' },
            { label: '角色权限管理', value: '/roleList' },
            { label: '图书分类管理', value: 'category' },
            { label: '图书分类添加', value: '/addCategory' },
            { label: '图书分类列表', value: '/categoryList' },
            { label: '图书管理', value: 'book' },
            { label: '图书添加', value: '/addBook' },
            { label: '批量添加', value: '/listAdd' },
            { label: '图书列表', value: '/bookList' },
            { label: '借书管理', value: 'borrow' },
            { label: '借书添加', value: '/addBorrow' },
            { label: '借书列表', value: '/borrowList' },
            { label: '还书管理', value: 'retur' },
            { label: '还书列表', value: '/returList' }
        ],
      selectedValues: [],  // 定义一个数组来存储多选框选中的值
      selectedValuesStr: '',
      form: {
          id:'',
          rolepath: ''
      }
    }
  },
  created() {
      const id = this.$route.query.id
      request.get('/role/getPath/'+id).then(res =>{
          if (res.code === '200') {
              this.selectedValuesStr = String(res.data.rolepath);
              this.selectedValues = this.selectedValuesStr.split(',')
          } else {
              this.$notify.error(res.msg)
          }
      })

      console.log(this.selectedValues)
  },
  methods: {
   save() {
       const id = this.$route.query.id
       this.selectedValuesStr = this.selectedValues.join(',')
       console.log(this.selectedValuesStr)
      this.form = {
           id: id,
           rolepath: this.selectedValuesStr
      }
       console.log(this.form.path)
      request.put('/role/update',this.form).then(res => {
        if (res.code === '200') {
          this.$notify.success('更新成功')
          this.$router.push("/roleList")
        } else {
          this.$notify.error(res.msg)
        }
      })
    },
   handleCheckAllChange(val) {
       this.selectedValues = val ? this.options.map(option => option.value) : [];
       this.isIndeterminate = false;
   },
   handleCheckedCitiesChange(value) {
          let checkedCount = value.length;
          this.checkAll = checkedCount === this.options.length;
          this.isIndeterminate = checkedCount > 0 && checkedCount < this.options.length;
      }

  }
}

</script>

