<template>
    <div>
        <el-upload
                :action="'http://localhost:9090/api/excel/upload?token=' + this.admin.token"
                :file-list="fileList"
        >
            <el-button type="primary">上传文件</el-button>
            <el-button
                    type="success" v-if="false">
                    <!--:disabled="fileList.length === 0"-->

                下载示例
            </el-button>
        </el-upload>
    </div>
</template>


<script>
    import request from "@/utils/request";
    import Cookies from 'js-cookie'
    export default {
        name: "ListAdd",
        data() {
            return {
                fileList: [],
                admin: Cookies.get('token') ? JSON.parse(Cookies.get('token')) : {},
            }
        },
        methods: {
            handleBeforeUpload(file) {
                this.fileList.push(file); // 限制同时上传一个文件
                return false; // 禁止自动上传
                console.log(fileList)
            },
            handleUpload() {
                console.log(fileList)
                const formData = new FormData();
                formData.append('file', this.fileList[0].raw);

                    request.post('/excel/upload', formData)
                    .then(response => {
                        console.log(response.data);
                        if (response.code === '200') {
                            this.$notify.success('新增成功')
                    }})
                    .catch(error => {
                        console.error(error);
                        this.$notify.error(res.msg)
                    });
            },
            handleUploadSuccess(response) {
                console.log(response);
                // 处理上传成功的逻辑
            },
            handleUploadError(error) {
                console.error(error);
                // 处理上传失败的逻辑
            }
        }
    }
</script>

<style scoped>

</style>