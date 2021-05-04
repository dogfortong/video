<template>
  <body>
    <div>

      <el-dialog title="提示" :visible.sync="dialogVisible" width="30%" :before-close="handleClose"
        style="text-align: center;">
        <span>
          <h3>[用户名] {{form.uname}}</h3>
          <h3>[账号] {{account}}</h3>
          <h3>[密码] {{form.password}}</h3>
        </span>
        <span slot="footer" class="dialog-footer">
          <el-button type="primary" @click="gotoLogin">确 定</el-button>
        </span>
      </el-dialog>

      <el-form ref="form" :rules="rules" :model="form" class="login-box">
        <h3 class="login-title">注册界面</h3>
        <el-form-item label="用户名" prop="uname" style="color: white;">
          <el-input type="input" v-model="form.uname" placeholder="请输入用户名"></el-input>
        </el-form-item>
        <el-form-item label="密码" prop="password">
          <el-input type="input" v-model="form.password" placeholder="请输入密码" show-password></el-input>
        </el-form-item>
        <el-form-item>
          <el-button type="primary" @click="submitForm('form')" class="login-button">确认</el-button>
        </el-form-item>
        </el-form-item>
      </el-form>
    </div>
  </body>
</template>

<script>
  import axios from 'axios'
  import Qs from 'qs'
  export default {
    name: 'Register',
    data() {
      return {
        dialogVisible: false,
        account: '',
        form: {
          uname: '',
          password: ''
        },

        rules: {
          uname: [{
            required: true,
            message: '请输入用户名',
            trigger: 'blur'
          }],
          password: [{
            required: true,
            message: '请输入密码',
            trigger: 'blur'
          }]
        },

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
            }
            else {
              this.$axios({
                  headers: {
                    'Content-Type': 'application/x-www-form-urlencoded'
                  },
                  method: 'post',
                  url: '/register',
                  data: Qs.stringify(this.form)
                })
                .then((response) => {
                    this.account = response.data.uid
                    console.log(response.data.uid);
                    this.$message({
                      type: 'success',
                      message: '注册成功',
                      duration: '2000'
                    });
                    setTimeout(() => {
                      this.dialogVisible = true;
                    }, 2000);
                  })
               }
            })
          },

          gotoLogin() {
            this.$router.push("/login");
          }

        }

      }
</script>

<style>
  body {
    position: absolute;
    background: url("../../static/image/5.jpg");
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
