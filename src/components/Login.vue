<template>
  <div>
    <div align="center">
      欢迎来到数字录入测试系统！
    </div>
    <div class="main">
      <div class="radio">
        <el-radio v-model="radio" label="1" size="medium" border>试卷1</el-radio>
        <el-radio v-model="radio" label="2" size="medium" border>试卷2</el-radio>
      </div>
      <el-button class="btn" @click="start()" icon="el-icon-thumb">进入测试</el-button>
    </div>
  </div>
</template>

<script>
  export default {
    name: 'Login',
    data() {
      return {
        radio: '1'
      }
    },
    methods: {
      start() {
        let kind = this.radio;
        this.$router.push({path: '/test', query: {kind: kind}});
      }
    }

  }
</script>

<style scoped>
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

  .radio {
    margin-top: 50px;
    margin-left: 40%;
  }

  .btn {
    margin-top: 100px;
    margin-left: 45%;
  }
</style>
