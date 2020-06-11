<template>
    <el-container class="body-bg">
  <el-header>
      <el-button @click="exit()"><i class="el-icon-back" style="margin-right:3px"></i>退出预览</el-button>
      <span>预览时填写的数据不予提交</span>
  </el-header>
  <el-main id='1'>
      <span>
          <el-button autofocus @click="phone=true"><i class="el-icon-mobile" style="margin-right:3px"></i>手机</el-button>
          <el-button @click="pc()"><i class="el-icon-c-scale-to-original" style="margin-right:3px"></i>电脑</el-button>
      </span>
      <div class="outside" v-show="phone">
        <div class="phone" >
            <iframe ref="frame" id="frame" :src="url" scrolling='no'></iframe>
        </div>
      </div>
      <div class="out" v-show="!phone">
        <iframe ref="frame2" id="frame2" scrolling='no'></iframe>
      </div>
  </el-main>
</el-container>
</template>
<script>
console.log(window)// javaScript的对象与window不同
export default {
  data: function () {
    return {
      phone: true,
      url: '',
      url2: ''
    }
  },
  methods: {
    exit () {
      window.history.back(-1)
    },
    pc () {
      this.phone = false
      console.log(this.$refs.frame)
      this.$refs.frame2.contentWindow.location.replace(this.url)
    }
  },
  created () {
    const url = window.location.href.split('/')
    this.url = url.slice(0, -2).join('/') + '/myform/' + this.$store.state.userName + '/' + parseInt(this.$route.params.id)
  },
  mounted () {
    var iframe = this.$refs.frame
    if (iframe.attachEvent) { // 兼容IE写法
      iframe.attachEvent('onload', function () {
        var that = iframe.contentWindow.document.getElementById('content')
        console.log(that)
        // iframe加载完成后要进行的操作
        that.onscroll = function () {
          // 变量scrollTop是滚动条滚动时，距离顶部的距离
          var scrollTop = that.scrollTop
          // 变量windowHeight是可视区的高度
          var windowHeight = that.clientHeight
          // 变量scrollHeight是滚动条的总高度
          var scrollHeight = that.scrollHeight
          // 滚动条到底部的条件
          console.log(scrollHeight)
          if (scrollTop + windowHeight === scrollHeight) {
            document.getElementById('1').scrollTo(0, scrollHeight)
            console.log(scrollHeight)
          }
          if (scrollTop === 0) {
            document.getElementById('1').scrollTo(0, 0)
            console.log(scrollHeight)
          }
        }
      })
    } else {
      console.log('firefox')
      iframe.onload = function () {
        var that = iframe.contentWindow.document.getElementById('content')
        console.log(that)
        // iframe加载完成后要进行的操作
        that.onscroll = function () {
          // 变量scrollTop是滚动条滚动时，距离顶部的距离
          var scrollTop = that.scrollTop
          // 变量windowHeight是可视区的高度
          var windowHeight = that.clientHeight
          // 变量scrollHeight是滚动条的总高度
          var scrollHeight = that.scrollHeight
          // 滚动条到底部的条件
          console.log(document.getElementById('1'))
          if (scrollTop + windowHeight === scrollHeight) {
            document.getElementById('1').scrollTo(0, scrollHeight)
            console.log(scrollHeight)
          }
          if (scrollTop === 0) {
            document.getElementById('1').scrollTo(0, 0)
            console.log(scrollHeight)
          }
        }
      }
    }
  }
}
</script>

<style lang="less" scoped>
#1{
    background-color: #E9EEF3;
}
#frame{
    width: 318px;
    height: 568px;
    margin: 74px 0 0 0px;
    border: none;
}
#frame2{
    width: 100%;
    margin: 0px;
    height: 100%;
    border: none;
}
.el-header{
    background-color: #B3C0D1;
    color: #333;
    line-height: 60px;
    .el-button{
        margin:5px 0 0 5px;
    }
    span{
        margin:5px 0 0 34%;
    }
  }
  .el-main {
    background-color: #E9EEF3;
    color: #333;
    text-align: center;
    height:100%;
    display: inline-block;
    overflow: hidden;
  }
  .el-container{
      height:100%;
  }
  .outside{
      text-align: center;
  }
  .out{
      display: inline-block;
      height: 100%;
      width: 100%;
      overflow: hidden;
  }
  .phone{
    display: inline-block;
    position: relative;
    margin-top: 10px;
    justify-content: center;
    width: 372px;
    height: 744px;
    background-image: url(../assets/phone.png);
    background-size: 372px 744px;
  }
</style>
