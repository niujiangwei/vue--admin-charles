<template>
    <div>
     <!--按钮-->
        <el-button type="success" size="small" @click="toAddHandler">添加</el-button>
        <el-button type="danger" size="small" @click="todeleteHandler">批量删除</el-button>
        <!--/按钮-->
        <!--表格-->
        <el-table :data="product">
            <el-table-column label="编号" prop="id"> </el-table-column>
            <el-table-column label="产品名称" prop="name"> </el-table-column>
            <el-table-column label="价格" prop="price"> </el-table-column>
            <el-table-column label="描述" prop="description"> </el-table-column>
            <el-table-column fixed="right" label="操作" > 
                <template v-slot="slot">
                    <a href="" @click.prevent="todeleteHandler(slot.row.id)">删除</a>
                    <a href="" @click.prevent="toupdateHandler" >修改</a>
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
         title="录入产品信息"
         :visible.sync="visible"
         width="60%">
         <el-form
          :model="form" label-width="80px">
          <!-- <el-form-item label="编号">
              <el-input v-model="form.id"></el-input>
          </el-form-item> -->
          <el-form-item label="产品名称">
              <el-input type="name" v-model="form.name"></el-input>
          </el-form-item>
          <el-form-item label="价格">
              <el-input type="price" v-model="form.price"></el-input>
          </el-form-item>
          <el-form-item label="描述">
              <el-input type="description" v-model="form.description"></el-input>
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
            let url = "http://localhost:6677/product/findAll"
      request.get(url).then((response)=>{
          this.product = response.data
      })
        },
        submitHandler(){

            let url = "http://localhost:6677/product/saveOrUpdate"
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
                
                })
            })
        
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
        toupdateHandler(){
            this.visible=true;
        },
        toAddHandler(){
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
            product:[],
            form:{}
        }
    },
    created(){
        // 文档加载完毕/vue实例创建完毕
        // this为当前Vue实例
        
            this.loadData();
        
    }
}
</script>

<style scoped>

</style>
