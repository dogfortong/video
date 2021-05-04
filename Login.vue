<template>
  <body>
    <div>
      <el-form ref="form" :rules="rules" :model="form" class="login-box">
        <h3 class="login-title">欢迎登录</h3>
        <el-form-item label="账号" prop="uid" style="color: white;">
          <el-input type="input" v-model="form.uid" placeholder="请输入账号"></el-input>
        </el-form-item>
        <el-form-item label="密码" prop="password">
          <el-input type="input" v-model="form.password" placeholder="请输入密码" show-password></el-input>
        </el-form-item>
        <el-form-item>
          <el-button type="primary" @click="submitForm('form')" class="login-button">登录</el-button>
        </el-form-item>
        </el-form-item>
        <el-form-item class="change-cache">
          <el-link type="primary" href="/main" class="no-login">游客登录</el-link>
          <el-link type="primary" href="/register" class="us-register">用户注册</el-link>
          <el-link type="primary" href="/findps" class="ps-back">找回密码</el-link>
        </el-form-item>
      </el-form>
    </div>
  </body>
</template>

<script>
  import axios from 'axios'
  import Qs from 'qs'
  export default {
    name: 'Login',
    data() {
      return {
        form: {
          uid: '',
          password: ''
        },
        rules: {
          uid: [{
            required: true,
            message: '请输入账号',
            trigger: 'blur'
          }],
          password: [{
            required: true,
            message: '请输入密码',
            trigger: 'blur'
          }]
        }
      }
    },
    methods: {
      submitForm(formName) {
        this.$refs[formName].validate((valid) => {
          if (!valid) {
            this.$message({
              type: 'warning',
              message: '请输入用户名或密码'
            });
            return false;
          } else {
            this.$axios({
                headers: {
                  'Content-Type': 'application/x-www-form-urlencoded'
                },
                method: 'post',
                url: '/login',
                data: Qs.stringify(this.form)
              })
              .then((response) => {
                  if (response.data.result == "1") {
                    this.$store.dispatch("asyncUpdateUser", {
                      name: response.data.uname,
                      head: response.data.head,
                      uid:this.form.uid
                    })
                    sessionStorage.setItem('state', JSON.stringify(this.$store.state));
                    this.$message({
                      type: 'success',
                      message: '登录成功',
                      duration: '2000'
                    });
                    console.log(this.$store.getters.getUser.head);
                    setTimeout(() => {
                      this.$router.push({
                        name: "Main",
                        params: {
                          name: response.data.uname,
                          head: response.data.head
                        }
                      })
                    }, 2000);
                  } else {
                    this.$message({
                      type: 'error',
                      message: '登录失败，请重新输入账号和密码！',
                      duration: '2000'
                    });
                  }
                },
                (response) => {
                  alert("false")
                })
          }
        });
      }
    }
  }
</script>

<style lang="scss" scoped>
  body {
    position: absolute;
    background: url("../../static/image/3.jpg");
    background-size: 100% 100%;
    height: 100%;
    width: 100%
  }

  .login-box {
    width: 350px;
    margin: 250px auto;
    border: 1px solid #DCDFE6;
    padding: 30px;
    border-radius: 30px;
    box-shadow: 0 2px 30px #DCDFE6;
  }

  .login-title {
    text-align: center;
    color: #303133;
  }

  .login-button {
    display: block;
    margin: 0 auto;
  }

  .no-login {
    margin-left: 15px;
  }

  .us-register {
    margin-left: 70px;
  }

  .ps-back {
    margin-left: 70px;
  }
</style>
