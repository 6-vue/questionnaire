<template>
<div class="myform_container" v-loading="flag2">
<div class="content_box" id="content" v-if="flag&&flag3">
   <div class="begin_content">
       <p class="title">
       {{this.Questionnaire.title}}
       </p>
    <div class="bar"></div>
   </div>
   <div class="question_box">
    <el-form :model="Questionnaire">
      <el-form-item v-for="(q,index) in Questionnaire.questions" :key="index" :label="q.question">
        <el-input v-if="q.type=='input'" v-model="answers.questions[index].answer[0]"></el-input>
        <el-checkbox-group v-if="q.type=='checkbox'" v-model="answers.questions[index].answer[0]">
          <el-checkbox v-for="(c,index) in q.choices" :key="index" :label="c"></el-checkbox>
        </el-checkbox-group>
        <el-radio-group v-if="q.type=='radio'" v-model="answers.questions[index].answer[0]">
          <el-radio v-for="(c,index) in q.choices" :key="index" :label="c"></el-radio>
        </el-radio-group>
      </el-form-item>
      <el-form-item>
        <el-button type="primary" @click="submitForm" :disabled="can">提交</el-button>
        <el-button>取消</el-button></el-form-item>
    </el-form>
    </div>
    <div class="clearfloat"></div>
    </div>
<div class="error" font-family="PingFang SC" v-if="!flag">问卷未发布或时间已截止，请联系管理员进行更改</div>
<div class="error" font-family="PingFang SC" v-if="!flag3">感谢您的认真填写</div>
</div>
</template>>
<script>
export default {
  data () {
    return {
      flag: true,
      flag2: true,
      flag3: true,
      myindex: '',
      userName: '',
      Questionnaire: {},
      answers: { questions: [] },
      can: false
    }
  },
  methods: {
    async submitForm () {
      for (var i = 0; i < this.Questionnaire.questions.length; i++) {
        if (this.answers.questions[i].answer[0] === '') {
          return this.$message.error('问题答案不能为空，请重新输入')
        }
      }
      console.log({
        index: this.myindex, userName: this.userName, answers: this.answers
      })
      const { data: res } = await this.$http.post('submitAnswer', {
        index: this.myindex, userName: this.userName, answers: this.answers
      })
      console.log(res)
      console.log(this.answers)
      this.$message.success('提交成功')
      this.flag3 = false
      window.scrollTo(0, 0)
    },
    async getQuestionnaire () {
      const { data: res } = await this.$http.post('myForm', {
        index: this.myindex, userName: this.userName
      })
      this.Questionnaire = res.data
      this.flag2 = false
      console.log(res.data)
      if (this.Questionnaire.status === '未发布' || this.Questionnaire.status === '已截止') {
        this.flag = false
        window.scrollTo(0, 0)
      }
      for (var i = 0; i < this.Questionnaire.questions.length; i++) {
        if (this.Questionnaire.questions[i].type === 'checkbox') {
          this.answers.questions.push({ answer: [[]] })
        } else {
          this.answers.questions.push({ answer: [''] })
        }
      }
    }
  },
  created () {
    this.myindex = this.$route.params.id
    this.userName = this.$route.params.userName
    this.getQuestionnaire()
  },
  mounted () {
    console.log(window.top.location.href.split('/').slice(-2, -1)[0])
    if (window.top.location.href.split('/').slice(-2, -1)[0] === 'preview') {
      this.can = true
    }
  }
}
</script>
<style>
.content_box::-webkit-scrollbar {
  width: 4px;
  height: 4px;
}
.content_box::-webkit-scrollbar-thumb {
  /*border-radius: 5px;
  -webkit-box-shadow: inset 0 0 5px rgba(0, 0, 0, 0.2);
  box-shadow: inset 0 0 5px rgba(0, 0, 0, 0.2);
  background: rgba(0, 0, 0, 0.2);*/
  border-radius: 10px;
  background-color: skyblue;
  background-image: -webkit-linear-gradient(45deg,
      rgba(255, 255, 255, 0.2) 25%,
      transparent 25%,
      transparent 50%,
      rgba(255, 255, 255, 0.2) 50%,
      rgba(255, 255, 255, 0.2) 75%,
      transparent 75%,
      transparent);
}

.content_box::-webkit-scrollbar-track {
  /*-webkit-box-shadow: inset 0 0 5px rgba(0, 0, 0, 0.2);
  box-shadow: inset 0 0 5px rgba(0, 0, 0, 0.2);
  border-radius: 0;
  background: rgba(0, 0, 0, 0.1);*/
  box-shadow: inset 0 0 5px rgba(0, 0, 0, 0.2);
  background: #ededed;
  border-radius: 10px;
}
</style>
<style lang="less" scoped>
.error{
  display:flex;
  margin-top:20%;
  justify-content: center;
  font-size: 50px;
  color:#606266;
}
.el-header {
    background-color: #ffffff;
    border: 1px solid #eee;
    display: flex;
    justify-content: space-between;
    padding-left: 0;
    align-items: center;
    font-size: 30px;
    margin-top: 60px;
    position: fixed;
    width: 100%;
    z-index: 500;
    > div{
      display: flex;
      align-items: center;
      span{
        margin-left: 15px;
      }
    }
}
.el-form-item__label{
    font-weight:bold;
    font-size:35px;
}

div{
    display: block;
    left:0%;
    top:0%;
}
.myform_container{
    background-color: #F5F5F5;
    height: 100%;
    margin-bottom: 2%;
}
.begin_content{
    display: block;
    margin-top: 20px;
}
.title{
    font-weight:bold;
    font-size:18px;
}
.bar{
    float: left;
    width: 100%;
    height: 3px;
    background-color: #53a4f4;
}
.content_box{
    width: 80%;
    background-color: #fff;
    padding: 1% 5% 3%;
    position: absolute;
    left: 5%;
    top: 5%;
    border-radius: 10px 0px 0px 10px;
    border:1px solid #ccc;
    max-height:85%;
    overflow-y:auto;
}
#content{
  background-color: #fff;
}
.question_box{
    margin-top: 20px;
}
.clearfloat{
   clear:both;
   height:0;
   font-size:1px;
   line-height:0px;
}
</style>
