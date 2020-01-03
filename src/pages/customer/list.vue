<template>
    <div>
        <h3>顾客管理<br></h3>
        <el-button @click="toAddHandler" type="primary">添加</el-button>
        <el-button @click="toAddHandler" type="danger">批量删除</el-button>
        <el-table :data="customers" stripe>
            <el-table-column prop="id" label="编号" ></el-table-column>
            <el-table-column prop="username" label="用户名" ></el-table-column>
            <el-table-column prop="realname" label="真实姓名" ></el-table-column>
            <el-table-column prop="gender" label="性别" ></el-table-column>
            <el-table-column prop="bankcard" label="银行卡号"></el-table-column>
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
            <el-form :model="form" label-width="80px">
                <el-form-item label="用户名">
                    <el-input v-model="form.username"></el-input>
                </el-form-item>
                <el-form-item label="密码">
                    <el-input type="password" v-model="form.password"></el-input>
                </el-form-item>
                <el-form-item label="真实姓名">
                    <el-input  v-model="form.realname"></el-input>
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
            <!-- <span>这是一段信息</span> -->
            <span slot="footer" class="dialog-footer">
                <el-button @click="closeModelHandler">取 消</el-button>
                <el-button type="primary" @click="submitHandler">确 定</el-button>
            </span>
        </el-dialog>
    </div>
</template>

<script>
import request from '@/utils/request'
import querystring from 'querystring'
export default {
    //用于存放网页需调用的方法
    methods:{
            loadData(){
                let url = "http://localhost:6677/customer/findAll"
                request.get(url).then((response)=>
                {
                    this.customers = response.data;
                })
            },
            toAddHandler(){
                this.title="录入顾客信息";
                this.visible=true;
                this.form={
                    type:"customer"
                }
            },
            closeModelHandler(){
                this.visible=false;
            },
            toUpdateHandler(row){
                this.form=row;
                this.visible=true;
                this.title="修改顾客信息"
            },
            submitHandler(){
                let url = "http://localhost:6677/customer/saveOrUpdate";
                request({
                    url,
                    method:"POST",
                    headers:{
                       "Content-Type":"application/x-www-form-urlencoded"
                    },
                    data:querystring.stringify(this.form)
                }).then((response)=>{
                        this.closeModelHandler()
                        this.loadData()
                        this.$message({
                            type:"success",
                            message:response.message
                        })
                    })
            },
            toDeleteHandler(id){
                this.$confirm('此操作将永久删除该文件, 是否继续?', '提示', {
                    confirmButtonText: '确定',
                    cancelButtonText: '取消',
                    type: 'warning'
                }).then(() => {
                    let url = "http://localhost:6677/customer/deleteById"
                    request.get(url+"?id="+id).then((response)=>{
                        this.loadData()
                        this.$message({
                        type: 'success',
                        message:"删除成功"
                    });
                    })
                    
                })
            }
    },
//data用于存放网页要显示的数据 数据绑定
    data(){
        return{
            visible:false,
            title:"录入顾客信息",
            customers:[],
            form:{}
        }
    },

    // 生命周期，VUE实例加载完毕所要执行的操作
    created(){
        this.loadData();
    }
}
</script>

<style scoped>

</style>