<template>
  <div id="login_container">
    <div id="login_box">
      <div id="login_logo">
        <img src="../assets/img/logo.png">
      </div>
      <el-form
        ref="loginFormRef"
        :model="loginForm"
        :rules="loginFormRules"
      >
        <el-form-item prop="username">
          <el-input v-model="loginForm.username">
            <i
              slot="prefix"
              class="iconfont icon-user"
            ></i>
          </el-input>
        </el-form-item>
        <el-form-item prop="password">
          <el-input
            type="password"
            v-model="loginForm.password"
          >
            <i
              slot="prefix"
              class="iconfont icon-password"
            ></i>
          </el-input>
        </el-form-item>
        <el-row>
          <el-col :offset='15'>
            <el-button
              type="primary"
              @click='login'
            >登录</el-button>
            <el-button
              type="info"
              @click="resetForm"
            >重置</el-button>
          </el-col>
        </el-row>
      </el-form>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      loginForm: {
        username: '',
        password: ''
      },
      loginFormRules: {
        username: [
          { required: true, message: '请输入用户名称', trigger: 'blur' }
        ],
        password: [{ required: true, message: '请输入密码', trigger: 'blur' }]
      }
    }
  },
  methods: {
    login() {
      this.$refs.loginFormRef.validate(async valid => {
        if (valid === true) {
          const { data: res } = await this.$http.post('login', this.loginForm)
          console.log(res)
          if (res.meta.status !== 200) {
            return this.$message.error('用户名或密码不存在')
          } else {
            window.sessionStorage.setItem('token', res.data.token)
            this.$router.push('/home')
            return this.$message.success('登录成功')
          }
        }
      })
    },
    resetForm() {
      this.$refs.loginFormRef.resetFields()
    }
  }
}
</script>

<style lang="css" scoped>
#login_container {
  background-color: #2b4b6b;
  height: 100%;
  overflow: hidden;
}
#login_box {
  width: 450px;
  height: 304px;
  background-color: #fff;
  border-radius: 4px;
  position: absolute;
  left: 50%;
  top: 50%;
  transform: translate(-50%, -50%);
}
#login_logo {
  width: 130px;
  height: 130px;
  border-radius: 50%;
  background-color: #fff;
  border: 1px solid #eee;
  padding: 8px;
  position: absolute;
  left: 50%;
  transform: translate(-50%, -50%);
  box-shadow: 0 0 10px #eee;
}
#login_logo img {
  width: 100%;
  height: 100%;
  border-radius: 50%;
  background-color: #eee;
}
.el-form {
  width: 100%;
  position: absolute;
  bottom: 0px;
  padding: 20px;
  box-sizing: border-box;
}
</style>
