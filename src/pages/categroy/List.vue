<template>
    <div>
        <el-button type="sign"  icon="el-icon-search" @click="toAddHandler">添加</el-button>
        <el-button type="danger">批量删除</el-button>

        <el-table :data="customers">
             <el-table-column type="selection"></el-table-column>
            <el-table-column prop="id" label="编号"></el-table-column>
            <el-table-column prop="name" label="栏目名称"></el-table-column>
            <el-table-column prop="num" label="序号"></el-table-column>
            <el-table-column prop="parentId" label="父栏目"></el-table-column>
            <el-table-column label="操作">
                <template v-slot="slot">
                    <a href="" @click.prevent="toDeleteHandler(slot.row.id)">删除</a>
                    <a href="" @click.prevent="toUpdateHandler">修改</a>
                    <a href="" @click= "">详情</a>
                </template>
            </el-table-column>
        </el-table>

<!--分页-->   
<el-pagination
    layout="prev, pager, next"
    :total="50">
  </el-pagination>

  <el-dialog
  title="添加栏目信息"
  :visible.sync="visible"
  width="60%"
  >
  <el-form :model="form" label-width="80px">
      <el-form-item label="栏目名称">
          <el-input v-model="form.name"></el-input>
      </el-form-item>
      <el-form-item label="序号">
          <el-input v-model="form.num"></el-input>
      </el-form-item>
  </el-form>
  
  <span slot="footer" class="dialog-footer">
    <el-button size="small" @click="closeModalHandler">取 消</el-button>
    <el-button size="small" type="primary" @click="submitHandler">确 定</el-button>
  </span>
</el-dialog>
            
    </div>
</template>>

<script>
import request from '@/utils/request'
import querystring from 'querystring'
export default {
    //用于存放网页中需要调用的方法
    methods:{
        toDeleteHandler(id){
            this.$confirm('此操作将永久删除该文件, 是否继续?', '提示', {
          confirmButtonText: '确定',
          cancelButtonText: '取消',
          type: 'warning'
        }).then(() => {
          this.$message({
            type: 'success',
            message: '删除成功!' +id
          });
        })
        },

        toUpdateHandler(){
            this.visible=true;
        },

    loadData(){
      let url = "http://localhost:6677/categroy/findAll"
      request.get(url).then((response)=>{
        this.customers = response.data;
      })
    },

        submitHandler(){
            let url = "http://localhost:6677/categroy/saveOrUpdate" //添加的模块
            request({
                url,
                method:"POST",//post保存更新，数据大  get查询
                hesders:{
                    "Content-Type":"application/x-www-form-urlencoded"
                },
                data:querystring.stringify(this.form)//转化成查询字符串
            }).then((response)=>{
                //请求结束 模态框关闭
                this.closeModalHandler();
                //刷新
                this.loadData();
                //提示消息
                this.$message({
                    type:"success",
                    message:response.message
                })
            })
        },

        closeModalHandler(){
            this.visible=false;
        },
        toAddHandler(){
            this.visible=true;
        },
    },
    //用于存放要想网页中显示的数据
    data(){
        return{
            visible:false,
            categroy:[],
            form:{}, //数据来自数据库
            checked: true
        }
    },
    
    created(){
    // vue实例创建完毕 
    this.loadData()

    }
    }

</script>>

<style scoped>

</style>>