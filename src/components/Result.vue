<template>
  <div>
    <div>
      <ul>
        <li class="nav-item" @click="logout">
          <img src="../assets/exit.png"/>
          <div style="margin-top: 2px">返回首页</div>
        </li>
      </ul>
    </div>
    <div style="margin-top: 30px;margin-left: 40%">你的成绩：{{score}}分 || 错误个数：{{count}}个</div>
    <el-table
      v-loading="loading"
      :data="errorRecords"
      style="width: 60%;margin: 20px 0 0 20%">
      <el-table-column
        prop="index"
        label="题目序号"
        width="180">
      </el-table-column>
      <el-table-column
        prop="dbvalue"
        label="正确答案"
        align="center">
      </el-table-column>
      <el-table-column
        prop="inputvalue"
        label="你的答案"
        align="center">
      </el-table-column>
    </el-table>
  </div>
</template>

<script>
  import {submitUrl} from "../api/api.js"
  export default {
    name: 'Result',
    data: function () {
      return {
        loading:true,
        score: 0,
        count:0,
        errorRecords: [],
      }
    },
    created(){
      let inputList = this.$route.params.inputList;
      console.log(inputList)
      this.$axios.post(submitUrl(inputList)).then(res => {
        console.log(res.data)
        this.loading = false;
        this.score = res.data.score;
        this.count = res.data.count;
        this.errorRecords = res.data.errorrecords;
      })
    },
    methods:{
      logout: function () {
        this.$router.push({path: '/'});
      }
    }
  }
</script>
<style>
  .nav-item {
    width: 50px;
    margin: 0;
    display: inline-block;
    float: right;
    padding: 0;
    font-size: 10px;
    vertical-align: middle;
    text-align: center;
    cursor: pointer;
  }
</style>
