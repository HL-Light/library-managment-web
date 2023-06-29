<template>
    <el-table :data="tableData" stripe>
        <el-table-column prop="id" label="编号" width="340"></el-table-column>
        <el-table-column prop="rolename" label="角色名" width="340"></el-table-column>
        <el-table-column prop="rolepath" label="角色权限" width="400" ></el-table-column>
        <el-table-column label="操作" width="230">
            <template v-slot="scope">
                <!--          scope.row 就是当前行数据-->
                <el-button type="primary" @click="$router.push('/roleEdit?id=' + scope.row.id)">编辑</el-button>
                <!--<el-popconfirm-->
                        <!--style="margin-left: 5px"-->
                        <!--title="您确定删除这行数据吗？"-->
                        <!--@confirm="del(scope.row.id)"-->
                <!--&gt;-->
                    <!--<el-button type="danger" slot="reference">删除</el-button>-->
                <!--</el-popconfirm>-->
            </template>
        </el-table-column>
    </el-table>
</template>

<script>
    import request from "@/utils/request";
    import Cookies from 'js-cookie'

export default {
        name: "Rolelist",
        data(){
            return{
                admin: Cookies.get('token') ? JSON.parse(Cookies.get('token')) : {},
                tableData: [],
                total: 0,
                form: {},
            }
        },
        created() {
            this.load()
        },
        methods: {
            load() {
                request.get('/role/list').then(res => {
                    if (res.code === '200') {
                        this.tableData = res.data
                        this.total = res.data.total
                        console.log(res.data)
                    }
                })
                console.log(this.tableData)

            },
            reset() {
                this.params = {
                    pageNum: 1,
                    pageSize: 10,
                    username: '',
                    phone: '',
                    email: ''
                }
                this.load()
            },
            del(id) {
                request.delete("/role/delete/" + id).then(res => {
                    if (res.code === '200') {
                        this.$notify.success('删除成功')
                        this.load()
                    } else {
                        this.$notify.error(res.msg)
                    }
                })
            }
        }
    }
</script>

<style scoped>

</style>