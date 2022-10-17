<template>
  <div class="login">
    <div class="login_form">
      <p>后台管理</p>
      <el-tabs v-model="activeName" @tab-click="handleClick">
        <el-tab-pane label="登录" name="first">
          <el-form :model="loginForm" :rules="rules" ref="loginForm" @submit.native.prevent>
            <el-form-item label="" prop="account" class="elItem">
              <el-input type="text" autocomplete="off" v-model="loginForm.account" prefix-icon="user"
                placeholder="请输入用户名">
              </el-input>
            </el-form-item>
            <el-form-item label="" prop="password">
              <el-input type="password" autocomplete="off" v-model="loginForm.password" prefix-icon="lock"
                placeholder="请输入密码"></el-input>
            </el-form-item>
            <el-form-item class="btns">
              <el-button type="primary" @click="goToLogin" native-type="submit">登录</el-button>
              <el-button @click="resetLoginForm">重置</el-button>
            </el-form-item>
          </el-form>
        </el-tab-pane>
        <el-tab-pane label="注册" name="second">
          <!-- //注册组件 -->
          <register></register>
        </el-tab-pane>
      </el-tabs>
    </div>
  </div>
</template>
 
<script lang="ts">
//引入注册组件
import register from '@/components/Register.vue';
export default {
  data() {
    var validateAccount = (rule, value, callback) => {
      if (value === "") {
        return callback(new Error("账号不能为空"));
      } else if (value === "admin") {
        callback();
      } else {
        callback(new Error("请输入正确的用户名"));
      }
    };
    var validatePassword = (rule, value, callback) => {
      if (value === "") {
        callback(new Error("请输入密码"));
      } else if (value === "123456") {
        callback();
      } else {
        callback(new Error("请输入正确的密码"));
      }
    };
    return {
      loginForm: {
        account: "",
        password: "",
      },
      activeName: 'first',//默认显示登录页面
      rules: {
        account: [
          {
            validator: validateAccount,
            trigger: "blur",
          },
        ],
        password: [
          {
            validator: validatePassword,
            trigger: "blur",
          },
        ],
      },
    };
  },
  methods: {
    //固定的账户密码判断实现简单的登录跳转功能，只能测试用
    goToLogin() {
      this.$refs["loginForm"].validate((valid) => {
        if (valid) {
          if (
            this.loginForm.account != "admin" ||
            this.loginForm.password != "123456"
          ) {
            this.$message.error("账号密码不正确");
            return false;
          } else {
            this.$message({ message: "登陆成功", type: "success" });
            this.$router.push("/home");
          }
        } else {
          this.$message.error("登陆失败");
          return false;
        }
      });
    },
    resetLoginForm() {
      this.$refs["loginForm"].resetFields();
    },
    handleClick() { }
  },
  components: {
    register
  }
};
</script>
 
<style scoped lang='less'>
.login {
  width: 100%;
  height: 100vh;
  // background-image: url("../assets/login/login.jpg");//背景图
  background-size: 100% 100%;
  background-position: center center;
  overflow: auto;
  position: relative;
  padding: 5%;

  display: flex;
  justify-content: center;
  align-items: center;

  // text-align: center;
  .login_form {
    // // width: 400px;
    width: 100%;
    // // height: 360px;
    // height: auto;
    // position: relative;
    // left: 50%;
    // top: 50%;
    // margin-left: -200px;
    padding: 4%;
    background: #fff;
    border-radius: 10px;
    box-shadow: 0 0 10px #ddd;

    display: inline;



    .btns {
      display: flex;
      justify-content: flex-end;
    }
  }

  p {
    font-size: 24px;
    text-align: center;
    font-weight: 600;
  }
}
</style>