<template>
   <div class="rdsmeta">
     <el-row>
       <el-col :span="22">
         <el-table :data="tableData" border max-height="800"  :fit=false style="width: 100%"
                   header-row-class-name="classHeader" row-class-name="classRow" cell-class-name="classCell" >

           <el-table-column type="expand">
             <template slot-scope="props">
               <el-form label-position="left" inline class="demo-table-expand" label-width="10%" label-suffix=":">
                 <el-form-item v-for="col in tableCols" :key="col.index" :label="col" align="left">
                   <span>{{props.row[col]}}</span>
                 </el-form-item>
               </el-form>
             </template>
           </el-table-column>

           <el-table-column v-for="col in tableCols" :key="col.index" :prop="col" :label="col" align="center" width="150px"  >
           </el-table-column>
         </el-table>
       </el-col>
     </el-row>
   </div>
</template>

<script>
  export default {
    name: 'rds',
    data(){
      return {
        tableData:null,
        tableCols:null
      }
    },
    mounted() {
      this.$store.dispatch('api_get_rds_meta').then(resp => {
        this.tableCols = resp.data.data.cols
        this.tableData = resp.data.data.data
      }).catch(error => {
        console.log('调用api_get_rds_meta接口失败')
      })
    }
  }
</script>



<style>
.classRow {
  color: blue;
}
.classHeader {
  color: orangered;
}
.classCell {
  color: green;
  height: 30px;
}

.demo-table-expand {
  font-size: 0;
}
.demo-table-expand label {
  width: 90px;
  color: #99a9bf;
}
.demo-table-expand .el-form-item {
  margin-right: 0;
  margin-bottom: 0;
  width: 100%;
}
</style>
