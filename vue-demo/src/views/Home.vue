<template>
  <div class="home">
    <el-container>
        <el-header>Header</el-header>
        <el-container>
            <el-aside width="10px">Aside</el-aside>
            <el-main>
                <el-button type="primary" @click="dialogFormVisible=true">添加</el-button>
                <el-table
                    :data="tableData"
                    style="width: 100%">
                    <el-table-column
                    label="姓名"
                    prop="user">
                    </el-table-column>
                    <el-table-column
                    label="密码"
                    prop="idd">
                    </el-table-column>
                    <el-table-column
                    label="地址"
                    prop="title">
                    </el-table-column>
                    <el-table-column
                    align="right">
                    <template slot="header" slot-scope="scope">
                        操作
                    </template>
                    <template slot-scope="scope">
                        <el-button
                        size="mini"
                         @click="eddit(scope.row.ids)"
                        
                     >Edit</el-button>
                        <el-button
                        size="mini"
                        type="danger"
                        @click="del(scope.row.ids)"
                      >Delete</el-button>
                    </template>
                    </el-table-column>
                </el-table>
                <div class="block">
                  <span class="demonstration">调整每页显示条数</span>
                  <el-pagination
                    @size-change="handleSizeChange"
                    @current-change="handleCurrentChange"
                    :page-sizes="[3, 4, 5, 6]"
                    :page-size=limit
                    layout="sizes, prev, pager, next"
                    :total=sum>
                  </el-pagination>
                </div>
            </el-main>
        </el-container>
    </el-container>

     <el-dialog title="添加成员" :visible.sync="dialogFormVisible">
        <el-form :model="form">
            <el-form-item label="姓名" :label-width="formLabelWidth">
                <el-input v-model="form.user" autocomplete="off"></el-input>
            </el-form-item>
            <el-form-item label="密码" :label-width="formLabelWidth">
                <el-input v-model="form.idd" autocomplete="off"></el-input>
            </el-form-item>
             <el-form-item label="家庭住址" :label-width="formLabelWidth">
                <el-input v-model="form.title" autocomplete="off"></el-input>
            </el-form-item>
           
           
        </el-form>
        <div slot="footer" class="dialog-footer">
            <el-button @click="dialogFormVisible = false">取 消</el-button>
            <el-button type="primary"  @click="add">确 定</el-button>
        </div>
    </el-dialog>
 
  
  </div>
</template>
<script>
import axios from 'axios'
  export default {
    methods: {
      handleClick(row) {
        console.log(row);
      }
    },

    data() {
      return {
        tableData: [],
        limit:3,
        pagenum:1,
        dialogFormVisible: false,
        formLabelWidth: '120px',
        sum:0,
        form: {
                user: '',
                idd:'',
                title:'',
              
            },
            id:0
      }
    },created(){
        this.getDate()
     },
     methods:{
       handleSizeChange(val) {
        this.limit=val
        axios.get(`/api/list?page=${this.pagenum}&&pageSize=${this.limit}`).then(res=>{
          this.tableData=res.data.data
        })
      },
      handleCurrentChange(val) {
        this.pagenum=val
        axios.get(`/api/list?page=${this.pagenum}&&pageSize=${this.limit}`).then(res=>{
          this.tableData=res.data.data
        })
      },
       getDate(){
         axios.get('/api/list').then((res)=>{
              this.sum=res.data.sum
              this.tableData=res.data.data
           })
       },
       add(){
         
         if(this.id){
           axios.post('/api/edit',{...this.form,ids:this.id}).then(res=>{
           this.getDate()
           this.dialogFormVisible = false;
           this.form={
                user: '',
                idd:'',
                title:'',
              
            },
          console.log(this.from)
         })
         }else{
           axios.post('/api/add',{...this.form}).then(res=>{
           console.log(this.form)
           this.getDate()
           this.dialogFormVisible = false;
           
           this.form={
                user: '',
                idd:'',
                title:'',
              
            }
         })
         }
       },
       del(ids){
         console.log(ids)
          axios.get('/api/del',{params:{ids:ids}}).then(res=>{
           this.getDate()
     
         })
       },
       eddit(ids){
        console.log(ids)
        this.id=ids

         this.dialogFormVisible = true;
        
       },
       changePage(cur){
         this.pagenum=cur;
         this.getDate()
       }
        
     }
  }
</script>
<style>
.el-header, .el-footer {
    background-color: #B3C0D1;
    color: #333;
    text-align: center;
    line-height: 60px;
  }
  
  .el-aside {
    background-color: #D3DCE6;
    color: #333;
    text-align: center;
  }
  
  .el-main {
    background-color: #E9EEF3;
    color: #333;
    text-align: center;
  }
  
  body > .el-container {
    margin-bottom: 40px;
  }

  .home,.el-container{
      width:100%;
      height: 100%;
      overflow: hidden;
  }
  
</style>