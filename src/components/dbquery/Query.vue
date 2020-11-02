<template>
  <div class="classquerydb">
  <el-row >
      <el-col :span="15" :offset="0">
       <el-form  label-width="100px">
         <el-form-item  label="选择数据库:">
           <el-select v-model="dbid" filterable placeholder="请选择" style="width: 100%;">
             <el-option v-for="item in all_db_list" :key="item.value" :label="item.label" :value="item.value">
             </el-option>
           </el-select>
         </el-form-item>
        </el-form>
      </el-col>
  </el-row>

  <el-row >
      <el-col :span="15" :offset="0">
     <el-form  label-width="100px">
        <el-form-item label="输入SQL:">
          <el-input type="textarea" :rows="8" placeholder="请输入SQL" v-model="dbsql">
          </el-input>
        </el-form-item>
        <el-form-item>
          <el-button type="primary" @click="handleQuery">查询</el-button>
        </el-form-item>
      </el-form>
    </el-col>
  </el-row>

  <el-row>
    <el-col :span="24" :offset="0">
      <el-table :data="tableData" border max-height="420"  :fit=false
       header-row-class-name="classHeader" row-class-name="classRow" cell-class-name="classCell">
        <el-table-column v-for="col in tableCols" :key="col.index" :prop="col" :label="col" align="center"
                         width="150px" :show-overflow-tooltip=true>
        </el-table-column>
      </el-table>
    </el-col>
  </el-row>


    </div>
</template>

<script>
  export default {
    name: 'query',
    data(){
      return {
        all_db_list: [],
        dbid: null,
        dbsql:'show tables;',
        tableData: null,
        tableCols: null,
      }
    },
    mounted(){
      this.$store.dispatch('api_get_all_db_list').then(resp => {
        this.all_db_list = resp.data
      }).catch(error => {
        console.log('调用api_get_all_db_list接口失败')
      })
    },
    methods:{
      handleQuery(){
        this.$store.dispatch('api_querydb', {dbid:this.dbid, dbsql:this.dbsql}).then(resp => {
          this.tableData = resp.data.data.data
          this.tableCols = resp.data.data.cols
        }).catch(error => {
          console.log('调用api_querydb接口失败')
        })
      }
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
</style>
