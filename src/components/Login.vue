<template>
  <div>
    <div class="txt" align="center">
      欢迎来到数字录入测试系统！
    </div>
    <div class="main">
      <el-button class="btn" @click="start()" icon="el-icon-thumb">进入测试</el-button>
    </div>
  </div>
</template>

<script>
  export default {
    name: 'Login',
    data() {
      return {}
    },
    methods: {
      start() {
        this.$router.push({path: '/test'});
      }
    }

  }
</script>

<style>
  .main {
    /*background: url("../assets/background.jpg") no-repeat;*/
    /*background-size: 100% 100%;*/
    /*-moz-background-size: 100% 100%;*/
    width: 100%;
    height: 500px;
  }

  .txt {
    /*background: url("../assets/header.jpg") no-repeat;*/
    /*background-size: 100% 100%;*/
    /*-moz-background-size: 100% 100%;*/
    width: 100%;
    height: 80px;
  }

  .btn {
    margin-top: 100px;
    margin-left: 45%;
  }
</style>
