<template>
    <div>
<!--按钮-->
        <el-button type="success" size="small" @click="toAddHandler">添加</el-button>
        <el-button type="danger" size="small" @click="todeleteHandler">批量删除</el-button>
        <!--/按钮-->
        <!--表格-->
        <el-table :data="system">
            <!-- <el-table-column label="编号" prop="id"> </el-table-column> -->
            <el-table-column label="栏目名称" prop="name"> </el-table-column>
            <el-table-column label="序号" prop="num"> </el-table-column>
            <el-table-column label="父栏目" prop="icon"> </el-table-column>
            <el-table-column fixed="right" label="操作" > 
                <template v-slot="slot">
                    <a href="" @click.prevent="todeleteHandler(slot.row.id)">删除</a>
                    <a href="" @click.prevent="toupdateHandler">修改</a>
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
       <!-- 模式框--> 
       <el-dialog
         title="录入系统信息"
         :visible.sync="visible"
         width="50%">
          <el-form
          :model="form" label-width="80px"> 
          <!-- <el-form-item label="编号">
              <el-input v-model="form.id"></el-input>
          </el-form-item> --> 
          <el-form-item label="栏目名称">
              <el-input type="name" v-model="form.name"></el-input>
          </el-form-item>
          <el-form-item label="序号">
              <el-input type="num" v-model="form.num"></el-input>
          </el-form-item>
          <!-- <el-form-item label="父栏目">
              <el-input type="icon" v-model="form.icon"></el-input>
          </el-form-item>-->
          <el-form-item label="所属栏目">
                    <el-select v-model="form.status" placeholder="请选择">
                        <el-option v-for="a in system"
                            :key="a.value" :label="a.name"
                            :value="a.parentId">
                        </el-option>
                    </el-select>
                </el-form-item> 
          </el-form>
          ----{{form}}
         <span>这是一段信息</span>
         <span slot="footer" class="dialog-footer">
         <el-button @click="closeModalHander" size="small">取 消</el-button>
         <el-button type="primary" @click="submitHandler" size="small">确 定</el-button>
        </span>
        </el-dialog> 
        <!--/模式框 -->
    </div>
    

</template>
<script>
import request from '@/utils/request'
import querystring from'querystring'
export default {
    //用于存放网页中用于调用的方法
    methods:{
        loadData(){
            let url = "http://localhost:6677/category/findAll"
      request.get(url).then((response)=>{
          this.system = response.data
      })
        },
        submitHandler(){

            let url = "http://localhost:6677/category/saveOrUpdate"
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
             let url = "http://localhost:6677/category/findAll"
            request.get(url).then((response)=>{
                this.system = response.data;
            })
            this.title="添加产品信息",
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
            a:[],
            system:[],
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
