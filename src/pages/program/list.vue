<template>
   <div>
       <h3>栏目管理<br></h3>
        <el-button type="primary" @click="toAddHandler">添加</el-button>
        <el-button type="danger">批量删除</el-button>
        <el-table ref="multipleTable" :data="programs" tooltip-effect="dark" style="width: 100%" >
            <el-table-column type="selection" width="120"></el-table-column>
            <el-table-column prop="id" label="编号" width="240">
                <!-- <template slot-scope="scope">{{ scope.row.date }}</template> -->
            </el-table-column>
            <el-table-column prop="name" label="栏目名称" width="240"></el-table-column>
            <el-table-column prop="num" label="序号" width="240" show-overflow-tooltip></el-table-column>
            <el-table-column prop="parentId" label="父栏目" width="240" show-overflow-tooltip></el-table-column>
            <el-table-column prop="address" label="操作" width="240" show-overflow-tooltip>
                <template v-slot="slot">
                    <el-button icon="el-icon-edit-outline" @click="upDataHandler(slot.row)"></el-button>
                    <el-button icon="el-icon-delete" @click="deleteHandler(slot.row.id)"></el-button>
                    <el-button >详情</el-button>
                </template>
            </el-table-column>
        </el-table>
         <el-dialog
            title="添加项目"
            :visible.sync="visible"
            width="60%">
             <el-form :model="ruleform" :rules="rules" ref="ruleform" label-width="80px">
                <el-form-item prop="name" label="栏目名称">
                    <el-input v-model="ruleform.name"></el-input>
                </el-form-item>
                <el-form-item prop="num" label="序号">
                    <el-input v-model="ruleform.num"></el-input>
                </el-form-item>
            </el-form>
            <span slot="footer" class="dialog-footer">
                <el-button @click="closeModelHandler">取 消</el-button>
                <el-button type="primary" @click="submitHandler('ruleform')">确 定</el-button>
            </span>
         </el-dialog>
   </div>
</template>
<script>
import request from '@/utils/request'
import querystring from 'querystring'
export default {
    data(){
        return{
            programs:[],
            ruleform:{},
            visible:false,
            delivery: false,
            rules:{
                name: [
                { required: true, message: '请输入活动名称', trigger: 'blur' },
                { min: 3, max: 5, message: '长度在 3 到 5 个字符', trigger: 'blur' }
                ],
                num: [
                { required: true, message: '请输入序号', trigger: 'blur' },
                { min: 3, max: 5, message: '长度在 3 到 5 个字符', trigger: 'blur' }
                ]
            }

        }
    },
    methods:{
        loadData(){
            let url = "http://localhost:6677/category/findAll"
            request.get(url).then((response)=>{
                this.programs = response.data;
            })
        },
        upDataHandler(row){

        },
        deleteHandler(){

        },
        toAddHandler(){
            this.visible=true
            this.form={
                type:""
            }
        },
        closeModelHandler(){
            this.visible=false
        },
        submitHandler(formName){
            this.$refs[formName].validate((valid) => {
                if (valid) {
                    let url="http://localhost:6677/category/saveOrUpdate"
                    request({
                        url,
                        method:"post",
                        Headers:{
                        "Content-Type":"application/x-www-form-urlencoded"
                        },
                        data:querystring.stringify(this.ruleform)
                    }).then((response)=>{
                        this.loadData()
                        this.closeModelHandler()
                        this.$message({
                            type:"success",
                            message:response.message
                        })
                     })
                } else {
                    console.log('error submit!!');
                    return false;
                }
            });           
        }
    },
    created(){
        this.loadData()
    },
    
    
    
}
</script>
<style scoped>

</style>