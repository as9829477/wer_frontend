<template>
     <el-form :rules="rules" class="login-container" lable-postition="left" label-with="0px" v-loading="loading">
        <h3 class="login_title">系统登陆</h3>
       <el-form-item prop="account">
            <el-input type="text" v-model="loginFrom.username" auto-complete="off" placeholder="账号"></el-input>
       </el-form-item>
       <el-form-item prop="checkPass">
            <el-input type="password" v-model="loginFrom.password" auto-complete="off" aria-placeholder="密码"></el-input>
       </el-form-item>
       <el-checkbox class="login_remember" v-model="checked" label-position="left">记住密码</el-checkbox>
       <el-form-item style="width: 100%">
              <el-button type="primary" style="width: 100%" @click="submitClick">登录</el-button>
       </el-form-item>
     </el-form>
</template>

<script>
    export default {
       data() {
         return {
           rules: {
             account:[{required:true,message:'请输入用户名',trigger:'blur'}],
             checkPass:[{required:true,message:'请输入密码',trigger:'blur'}]
           },
           checked:true,
           loginFrom:{
             username:'admin',
             password:'123'
           },
           loading:false
         }
       },
      methods:{
        submitClick:function () {
           var _this=this;
           this.loading=true;
           this.postRequest('/login',{
             username:this.loginFrom.username,
             password: this.loginFrom.password
           }).then(resp=>{
             _this.loading=false;
             if (resp && resp.status==200) {
                var  data=resp.data;
                _this.$scope.commit('login',data.obj);
                var path=_this.$route.query.redirect;
                _this.$router.replace({path:path=='/' || path==undefined ? '/home':path});
             }
           });
        }
      }
    }
</script>

<style scoped>
    .login-container{
      /*//该属性允许您为元素添加圆角边框！*/
        border-radius:15px;
      /*background-clip:规定背景的绘制区域，里面有三个值border-box：背景被裁剪到边框盒*/
      /*                                               padding-box：背景被裁剪到内边距框。*/
      /*                                               content-box    背景被裁剪到内容框。*/
      background-clip:padding-box;
        /*上下外边距为180px，左右外边距自动*/
      margin: 180px auto;
      width: 350px;
      /*padding是指自身边框到自身内部另一个容器边框之间的距离，就是容器内距离。*/
      padding: 35px 35px 15px 35px;
      /*属性在一个声明中设置所有的背景属性*/
      background: #fff;
      /*4 个边框的样式：border-width border-style border-color */
      border: 1px solid #eaeaea;
      /*外阴影：box-shadow: X轴  Y轴  Rpx  color;*/
      box-shadow: 0 0 25px #cac6c6;
    }
  .login_title{
    margin: 0px auto 40px auto;
    /*文本对齐方式*/
    text-align: center;
    color: #505458;
  }
  .login_remember{
    margin: 0px 0px 35px 0px;
    text-align: left;
  }
</style>
