<template>
  <el-container class="home-container">
    <link rel="stylesheet" href="../assets/fonts/iconfont.css">
    <!-- 头部区域 -->
    <el-header>
      <div>
        <img src="../assets/logo2.png" alt="">
        <span>问卷平台</span>
      </div>
      <el-menu :default-active="$route.path" class="el-menu-demo" mode="horizontal" :router="true">
        <el-menu-item index="/mine">我的问卷</el-menu-item>
        <el-menu-item index="/welcome">客服中心</el-menu-item>
     </el-menu>
      <p style="margin-right:0"><i class="iconfont icon-user" style="margin-right:2px"></i>{{userName}}</p>
      <el-button type="info" @click="logout">退出</el-button>
    </el-header>
    <!-- 页面主体区域 -->
    <el-container>
      <router-view></router-view>
  </el-container>
</el-container>
</template>

<script>
export default {
  data () {
    return {
      userName: this.$store.state.userName
    }
  },
  methods: {
    logout () {
      window.sessionStorage.removeItem('token')
      this.$router.push('/login')
    }
  }
}

</script>
<style>
/* 在全局CSS里引用: */
/* 滚动条宽度 */
::-webkit-scrollbar {
 width: 7px;
 height: 10px;
}
/* 滚动条的滑块 */
::-webkit-scrollbar-thumb {
 background-color: #a1a3a9;
 border-radius: 3px;
}
</style>
<style lang="less" scoped>
.home-container{
    height: 100%;
}
.el-container{
  background-color: #f7f8fa;
}
.el-header {
    background-color: #ffffff;
    border: 1px solid #eee;
    display: flex;
    justify-content: space-between;
    padding-left: 0;
    align-items: center;
    font-size: 20px;
    > div{
      display: flex;
      align-items: center;
      > img{
        height:60px;
      }
      span{
        margin-left: 15px;
      }
    }
    position: fixed;
    width: 100%;
    z-index: 500;
}
</style>
