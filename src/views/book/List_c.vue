<template>
    <div>

        <el-table :data="tableData" stripe row-key="id"  default-expand-all>
            <el-table-column prop="id" label="编号" width="80"></el-table-column>
            <el-table-column prop="bookid" label="图书编号"></el-table-column>
            <el-table-column prop="adress" label="书架位置"></el-table-column>
            <el-table-column prop="name" label="图书名称"></el-table-column>
            <el-table-column prop="bookNo" label="ISBN"></el-table-column>
            <el-table-column prop="description" width="200" label="描述"></el-table-column>
            <el-table-column prop="publishDate" label="出版日期"></el-table-column>
            <el-table-column prop="author" label="作者"></el-table-column>
            <el-table-column prop="publisher" label="出版社"></el-table-column>
            <el-table-column prop="category" label="分类号"></el-table-column>
            <el-table-column prop="cover" label="封面">
                <template v-slot="scope">
                    <el-image :src="scope.row.cover" :preview-src-list="[scope.row.cover]"></el-image>
                </template>
            </el-table-column>
            <el-table-column prop="createtime" label="创建时间"></el-table-column>
            <el-table-column prop="updatetime" label="更新时间"></el-table-column>
        </el-table>


        <!--    分页-->
        <div style="margin-top: 20px">
            <el-pagination
                    background
                    :current-page="params.pageNum"
                    :page-size="params.pageSize"
                    layout="prev, pager, next"
                    @current-change="handleCurrentChange"
                    :total="total">
            </el-pagination>
        </div>

    </div>
</template>

<script>
    import request from "@/utils/request";
    import Cookies from 'js-cookie'

    export default {
        name: "BookList_c",
        data() {
            return {
                role: " ",
                admin: Cookies.get('token') ? JSON.parse(Cookies.get('token')) : {},
                tableData: [],
                total: 0,
                params: {
                    pageNum: 1,
                    pageSize: 10,
                    name: '',
                    bookNo: ''
                }
            }
        },
        created() {
            const id = this.$route.query.id
            this.role = Cookies.get('role')
            this.load()
        },
        methods: {
            load() {
                const id = this.$route.query.id
                request.get('/book/page_c/' , {
                    params: {
                        ...this.params,
                        book_id: id
                    }
                }).then(res => {
                    if (res.code === '200') {
                        this.tableData = res.data.list
                        this.total = res.data.total
                    }
                })
            },
            reset() {
                this.params = {
                    pageNum: 1,
                    pageSize: 10,
                    name: '',
                    bookNo: ''
                }
                this.load()
            },
            handleCurrentChange(pageNum) {
                // 点击分页按钮触发分页
                this.params.pageNum = pageNum
                this.load()
            }
        }
    }
</script>

<style scoped>

</style>