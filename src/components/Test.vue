<template>
  <div id="test">
    <div>
      <ul>
        <li class="nav-item" @click="logout">
          <img src="../assets/exit.png"/>
          <div style="margin-top: 2px">返回首页</div>
        </li>
      </ul>
    </div>
    <div style="margin-left: 40%;width: 80px">剩余时间：</div>
    <count-down v-on:end_callback="countDownE_cb()"
                :startTime="startTime"
                :endTime="endTime"
                :tipTextEnd="'剩余时间:'"
                :endText="'测试结束'"
                :dayTxt="'天'"
                :hourTxt="'小时'"
                :minutesTxt="'分钟'"
                :secondsTxt="'秒'"
                style="width: 160px;margin-top: -38px;margin-left: 47%">
    </count-down>

    <div v-for="(item,index) in inputs" style="margin-left: 40%">
      <label class="txt">{{ index + 1 + pageSize * page}}</label>
      <el-input class="input" type="text" v-model="item.val" clearable
                @keyup.enter.native="nextFocus(index)"
                @keyup.up.native="beforeFocus(index)">
      </el-input>
    </div>
    <div class="btn">
      <el-button type="primary" icon="el-icon-arrow-left" @click="lastPage">上一页</el-button>
      <el-button type="primary" @click="submitTest">交卷</el-button>
      <el-button type="primary" @click="nextPage">下一页<i class="el-icon-arrow-right el-icon--right"></i></el-button>
    </div>
  </div>
</template>

<script>
  import ElInput from "../../node_modules/element-ui/packages/input/src/input.vue";
  import CountDown from 'vue2-countdown'
  import ElButton from "../../node_modules/element-ui/packages/button/src/button.vue";
  import {submitUrl} from "../api/api.js"
  import ElIcon from "../../node_modules/element-ui/packages/icon/src/icon.vue";

  export default {
    components: {
      ElIcon,
      ElButton,
      ElInput,
      CountDown
    },

    name: 'Test',
    data() {
      return {
        kind:'',
        pageSize: 5,
        page: 0,
        startTime: 0,
        endTime: 0,
        allInputs: [],
        focusIndex: 0,
        inputs: [
          {val: ""},
          {val: ""},
          {val: ""},
          {val: ""},
          {val: ""},
        ]
      }
    },
    created() {
      this.startTime = new Date().getTime();
      this.endTime = this.startTime + 1000 * 60 * 10;
      this.kind = this.$route.query.kind;
    },
    mounted(){
      let inputEles = document.getElementsByTagName("input");
//      console.log(inputEles[0])
      inputEles[0].focus();
    },
    methods: {
      countDownE_cb() {
        console.log("计时结束！")
        let inputEles = document.getElementsByTagName("input");
        document.getElementById('test').style.visibility = "hidden";
        for (let i = 0; i < inputEles.length; i++) {
          this.allInputs[i + this.page * this.pageSize] = this.inputs[i].val;
        }
        let inputList = this.allInputs;

//        console.log(inputList)
        this.$router.push({name: 'Result', params: {inputList: inputList,kind:this.kind }})
      },
      nextFocus(index) {
//        console.log("enter")
        let keyCode = event.keyCode;
        let inputEles = document.getElementsByTagName("input");
        if (keyCode === 13) {
          this.allInputs[index + this.page * this.pageSize] = this.inputs[index].val;
          // 如果是最后一个，清空inputs,page+1,光标定位到第一个输入框
          if (index === (inputEles.length - 1)) {
            if (Math.floor(this.allInputs.length / 5) === this.page) {
              for (let i = 0; i < inputEles.length; i++) {
                this.allInputs[i + this.page * this.pageSize] = this.inputs[i].val;
                this.inputs[i].val = "";
              }
            } else {
              for (let i = 0; i < inputEles.length; i++) {
                this.inputs[i].val = this.allInputs[i + (this.page + 1) * this.pageSize]
              }
            }
            this.page += 1;
            inputEles[0].focus();
          } else {
            inputEles[index + 1].focus();
          }
        }
      },
      beforeFocus(index) {
        console.log("up")
        let keyCode = event.keyCode;
        let inputEles = document.getElementsByTagName("input");
        if (keyCode === 38) {
          if (index === 0) {
            inputEles[0].focus();
          } else {
            for (let i = 0; i < inputEles.length; i++) {
              if (index === i) {
                inputEles[i - 1].focus();
                break;
              }
            }
          }
        }
      },
      lastPage() {
        let inputEles = document.getElementsByTagName("input");
        if (this.page > 0) {
          for (let i = 0; i < inputEles.length; i++) {
            this.allInputs[i + this.page * this.pageSize] = this.inputs[i].val;
            this.inputs[i].val = this.allInputs[i + (this.page - 1) * this.pageSize];
          }
          this.page -= 1;
          inputEles[inputEles.length - 1].focus();
        } else {
          inputEles[0].focus();
          this.$message('已经到达第一页');
        }
      },
      nextPage() {
        let inputEles = document.getElementsByTagName("input");
        if (this.page < 79) {
          if (Math.floor(this.allInputs.length / 5) === this.page) {
            for (let i = 0; i < inputEles.length; i++) {
              this.allInputs[i + this.page * this.pageSize] = this.inputs[i].val;
              this.inputs[i].val = "";
            }
          } else {
            for (let i = 0; i < inputEles.length; i++) {
              this.inputs[i].val = this.allInputs[i + (this.page + 1) * this.pageSize]
            }
          }
          this.page += 1;
          inputEles[0].focus();
        } else {
          inputEles[inputEles.length - 1].focus();
          this.$message('已经到达最后一页');
        }

      },
      submitTest() {
        let inputEles = document.getElementsByTagName("input");
        for (let i = 0; i < inputEles.length; i++) {
          console.log(this.inputs[i])
          this.allInputs[i + this.page * this.pageSize] = this.inputs[i].val;
        }
        this.$confirm('交卷后将不能继续答题, 是否继续?', '提示', {
          confirmButtonText: '确定',
          cancelButtonText: '取消',
          type: 'warning'
        }).then(() => {
          let inputList = this.allInputs;
//          console.log(inputList)
          this.$router.push({name: 'Result',params: {inputList: inputList,kind:this.kind }})
        }).catch(() => {
          console.log("错误")
        });
      },
      logout: function () {
        this.$router.push({path: '/'});
      }
    },

  }
</script>

<style scoped>
  .disable {
    pointer-events: none;
  }

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

  .txt {
    margin-top: 20px;
    margin-left: 2px;
    width: 10px;

  }

  .input {
    width: 200px;
    margin-top: 20px;
    margin-left: 10px;
  }

  .btn {
    margin-top: 30px;
    margin-left: 38%;
  }
</style>
