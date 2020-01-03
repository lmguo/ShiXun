<template>
    <div>
        <h3>员工管理<br></h3>
        <el-button @click="toAddHandler" type="primary">添加</el-button>
        <el-button type="danger">批量删除</el-button>
        <el-table :data="employee" stripe>
            <el-table-column prop="id" label="编号" ></el-table-column>
            <el-table-column prop="realname" label="姓名" ></el-table-column>
            <el-table-column prop="gender" label="性别"></el-table-column>
            <el-table-column prop="id_card" label="身份证号" ></el-table-column>
            <el-table-column prop="bank_card" label="银行卡号" ></el-table-column>
            <el-table-column prop="telephone" label="联系方式"></el-table-column>
            <el-table-column label="操作" width="240">
                <template v-slot="slot">
                    <el-button icon="el-icon-edit-outline" @click="toUpdateHandler(slot.row)"></el-button>
                    <el-button icon="el-icon-delete" @click="toDeleteHandler(slot.row.id)"></el-button>
                    <el-button >详情</el-button>
                </template>
            </el-table-column>
        </el-table>
        <!-- <el-pagination 
            layout="prev, pager, next"
            :total="50">
        </el-pagination> -->
        <el-dialog
            :title="title"
            :visible.sync="visible"
            width="60%"
            >
            <el-form :model="form">
                <el-form-item label="用户名">
                    <el-input v-model="form.username"></el-input>
                </el-form-item>
                <el-form-item label="密码">
                    <el-input type="password" v-model="form.password"></el-input>
                </el-form-item>
                <el-form-item label="真实姓名">
                    <el-input v-model="form.realname"></el-input>
                </el-form-item>
                <el-form-item label="性别">
                    <el-radio-group v-model="form.gender">
                        <el-radio label="男">男</el-radio>
                        <el-radio label="女">女</el-radio>
                    </el-radio-group>
                </el-form-item>
                <el-form-item label="联系方式">
                    <el-input v-model="form.telephone"></el-input>
                </el-form-item>
            </el-form>
            <span slot="footer" class="dialog-footer">
                <el-button @click="closeModelHandler">取 消</el-button>
                <el-button type="primary" @click="submitHandler">确 定</el-button>
            </span>
        </el-dialog>
    </div>
</template>

<script>
import request from '@/utils/request'
import querstring from 'querystring'
export default {
    methods:{
        loadData(){
            let url = "http://localhost:6677/waiter/findAll"
            request.get(url).then((response)=>
            {
                this.employee = response.data
            })
        },
        submitHandler(){
            let url = "http://localhost:6677/waiter/saveOrUpdate"
            request({
                url,
                method:"POST",
                //请求体中放的是查询字符串
                headers:{
                    "Content-Type":"application/x-www-form-urlencoded"
                },
                //请求体中的数据 
                data:querstring.stringify(this.form)
            }).then((response)=>{
                this.closeModelHandler()
                this.loadData()
                $message({
                    type:"success",
                    message:response.message
                })
            })
        },
        toAddHandler(){
            this.title="录入员工信息"
            this.visible=true;
            this.form={
                type:"waiter"
            }
        },
        closeModelHandler(){
            this.visible=false;
        },
        toUpdateHandler(row){
            this.form=row
            this.title="修改员工信息"
            this.visible=true;
        },
        toDeleteHandler(id){
            this.$confirm('此操作将永久删除该文件, 是否继续?', '提示', {
                    confirmButtonText: '确定',
                    cancelButtonText: '取消',
                    type: 'warning'
            }).then(() => {
                    this.$message({
                        type: 'success',
                        message: '删除成功!'
                    });
                })
        }

    },
    created(){
        this.loadData()
    },
    data(){
        return{
            title:"录入员工信息",
            visible:false,
            employee:[],
            form:{}
        }
    }  
}
</script>

<style scoped>

</style>