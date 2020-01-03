<template>
    <div>
        <el-button size="small" type="primary" @click="toAddHand">添加</el-button>
        <el-button size="small" type="danger" @click="todeleteHandler">批量删除</el-button>
        <el-table :data="employees">
            <el-table-column label="编号" prop="id"></el-table-column>
            <el-table-column label="姓名" prop="realname"></el-table-column>
            <el-table-column label="联系方式" prop="telephone"></el-table-column>
            <el-table-column label="身份证号" prop="idCard"></el-table-column>
            <el-table-column label="银行卡号" prop="bankCard"></el-table-column>
            <el-table-column fixed="right" label="操作"> 
                <template v-slot="slot">
                 <a href="" @click.prevent="todeleteHandler(slot.row.id)">删除</a>
                    <a href="" @click.prevent="toupdateHandler(slot.row)">修改</a>
                </template>
                </el-table-column>
        </el-table>
         <!--分页-->
         <el-pagination
         layout="prev, pager, next"
        :total="50">
       </el-pagination>
       <!--/分页-->
       <!--模式框-->
       <el-dialog
         :title="title"
         :visible.sync="visible"
         width="50%">
         ---{{form}}
         <el-form lable-width="80px">
             <el-form-item label="用户名">
                 <el-input v-model="form.realname"/>
             </el-form-item>
             <el-form-item label="联系方式">
                 <el-input v-model="form.telephone"/>
             </el-form-item>
             
             </el-form>
         <span slot="footer" class="dialog-footer">
         <el-button @click="closeModalHander" size="small">取 消</el-button>
         <el-button type="primary" @click="submitHandler" size="small">确 定</el-button>
        </span>
        </el-dialog>
        <!--/模式框-->
        </div>
</template>

<script>
import request from '@/utils/request'
import querystring from'querystring'
export default {
      data(){
        return {
            title:"录入员工信息",
            visible:false,
            employees:[],
            form:{
                type:"success"
            }

            
        
        }
    },
       created(){
           this.lodaDate();
       },
    methods:{
        
        submitHandler(){

            let url = "http://localhost:6677/waiter/saveOrUpdate"
            request({
                url,
                method:"POST",
                headers:{
                "Content-Type": "application/x-www-form-urlencoded"
                
                },
                data:querystring.stringify(this.form)
            }).then((response) => {
                 this.closeModalHander();
                this.$message({
                    type:"success",
                    message:response.message
                
                })
                this.lodaDate();
            })
            
        },
        lodaDate(){
            let url = "http://localhost:6677/waiter/findAll";
            request.get(url).then((response)=>{
                this.employees = response.data
                //箭头函数中的this指向外部函数中的this 
            })
        },
toAddHand(){
     this.form = {
                type:"customer"
            }
    this.visible = true;
    this.title = "录入员工信息"
},
todeleteHandler(id){
     //确认
             this.$confirm('此操作将永久删除该文件, 是否继续?', '提示', {
          confirmButtonText: '确定',
          cancelButtonText: '取消',
          type: 'warning'
        }).then(() => {
          this.$message({
            type: 'success',
            message: '删除成功!'+id
          });
        })
        },
toupdateHandler(row){
    this.form = row
    this.visible = true
    this.title = "修改员工信息"
}
    },
    
    
    
}
</script>
<style scoped>

</style>
