<template>
    <div>
        <!--按钮-->
        <el-button type="success" size="small" @click="toAddHandler">添加</el-button>
        <el-button type="danger" size="small" @click="todeleteHandler">批量删除</el-button>
        <!--/按钮-->
        <!--表格-->
        <el-table :data="order">
            <el-table-column label="订单编号" prop="id"> </el-table-column>
            <el-table-column label="下单时间" prop="orderTime"> </el-table-column>
            <el-table-column label="总价" prop="total"> </el-table-column>
            <el-table-column label="状态" prop="status"> </el-table-column>
            <el-table-column label="顾客id" prop="customerId"> </el-table-column>
            <el-table-column fixed="right" label="操作" > 
                <template v-slot="slot">
                    <a href="" @click.prevent="todeleteHandler(slot.row.id)">删除</a>
                    <a href="" @click.prevent="toupdateHandler (slot.row)" >修改</a>
                </template>
            </el-table-column>
        </el-table>
        <!--/表格-->
        <!--分页-->
         <el-pagination
         layout="prev, pager, next"
        :total="50">
       </el-pagination>
       <!--/分页-->
        <!--模式框-->
       <el-dialog
         title="录入订单信息"
         :visible.sync="visible"
         width="60%">
         <el-form
          :model="form" label-width="80px">
          <el-form-item label="订单编号">
              <el-input v-model="form.id"></el-input>
          </el-form-item>
          <el-form-item label="下单时间">
              <el-input type="orderTime" v-model="form.orderTime"></el-input>
          </el-form-item>
          <el-form-item label="总价">
              <el-input type="total" v-model="form.total"></el-input>
          </el-form-item>
          <el-form-item label="状态">
              <el-input type="status" v-model="form.status"></el-input>
          </el-form-item>
          <el-form-item label="顾客id">
              <el-input type="customerId" v-model="form.customerId"></el-input>
          </el-form-item>
          </el-form>
          ----{{form}}
         <span>这是一段信息</span>
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
    //用于存放网页中用于调用的方法
    methods:{
        loadData(){
            let url = "http://localhost:6677/order/findAll"
      request.get(url).then((response)=>{
          this.order = response.data
      })
        },
        submitHandler(){

            let url = "http://localhost:6677/order/save"
            request({
                url,
                method:"POST",
                headers:{
                "Content-Type": "application/x-www-form-urlencoded"
                
                },
                data:querystring.stringify(this.form)
            }).then((response)=>{
                this.closeModalHander();
                this.$message({
                    type:"success",
                    message:response.message
                
                });
                this.visible=false;
                 this.loadData();
            })
        
                    },
        todeleteHandler(id){
            //确认
             this.$confirm('此操作将永久删除该文件, 是否继续?', '提示', {
          confirmButtonText: '确定',
          cancelButtonText: '取消',
          type: 'warning'
        }).then(() => {
            
                //   刷新
               
                // 提示
                this.$message({           
            type: 'success',
            message: '删除成功！'+id
              });
          this.loadData();
          });
        
        },
        toupdateHandler(row){
            this.form = row;
            this.visible=true;
        },
        toAddHandler(){
            this.form = {
                type:"order"
            }
            this.visible=true;
        },
        closeModalHander(){
            this.visible=false;
        }
    },
    //用于存放要向网页中显示的数据
    data(){
        return{
            visible:false,
            order:[],
            form:{}
        }
    },
    created(){
        // 文档加载完毕/vue实例创建完毕
        // this为当前Vue实例
        
            this.loadData()
        
    }
}
</script>

<style scoped>

</style>