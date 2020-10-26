<template>
  <div class="login-form">
    <div v-if="!logged">
      <el-form :model="singInForm" status-icon label-width="100px">
        <el-form-item label="手机号" prop="tel">
          <el-input type="password" v-model="singInForm.tel"></el-input>
        </el-form-item>
        <el-form-item label="密码" prop="password">
          <el-input v-model="singInForm.password"></el-input>
        </el-form-item>
        <el-form-item>
          <el-button type="success" @click="singIn">登录</el-button>
          <a :href="qqUrl" target="_blank"><el-button type="primary" @click="qqSingIn">QQ登录</el-button></a>
        </el-form-item>
      </el-form>
    </div>
    <div v-if="logged" class="success-ui">
      <h1>登陆成功</h1>
    </div>
  </div>
</template>

<script>
  import $ from "jquery"
  export default {
    name: "Home",
    data() {
      return {
        qqUrl: "https://graph.qq.com/oauth2.0/authorize?response_type=code&client_id=101780702&redirect_uri=http://www.pawntest.com/qqlogin&state=test",
        // 模态框可见
        dialogVisible: false,
        // 是否已登录
        logged: sessionStorage.getItem("access-token"),
        // 登录表单
        singInForm: {
          tel: "",
          password: ""
        }
      }
    },
    methods: {
      // 登录
      singIn() {
        let that = this;
        $.ajax({
          url: "http://localhost/user/login",
          type: "POST",
          data: that.singInForm,
          success: res => {
            if (res.code === 400) {
              alert(res.message);
              return;
            }
            // 改变登录状态
            this.logged = true;
            // 保存令牌
            sessionStorage.setItem("access-token", res);
          }
        });
      },
      // QQ 登录
      qqSingIn() {
        $.ajax({
          url: "http://localhost/qqlogin",
          success: res => {
            sessionStorage.setItem("access-token", "token");
            location.reload();
          }
        });
      }
    }
  };
</script>

<style scoped>
  .el-form-item {
    width: 30%;
  }
  .el-button {
    width: 38%;
    margin-left: 8px;
  }
  .login-form {
    margin-left: 40%;
    margin-top: 200px;
  }
  .success-ui {
    margin-left: -60%;
  }
</style>