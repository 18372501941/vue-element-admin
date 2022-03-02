<template>
  <div class="login-container">
    <div class="login-wrap">
      <div class="login-box-left">
        <img src="../../assets/login/main_icon.png" alt="">
      </div>
      <div class="login-box-right">
        <el-form ref="loginForm" :model="loginForm" :rules="loginRules" class="login-form"  label-position="left">
          <div class="title-container">
            <h3 class="title">企业职业健康管理自查系统</h3>
          </div>
          <el-tabs style="margin-bottom:15px"  v-model="activeName" centered>
            <el-tab-pane label="账号密码登录" name="login"></el-tab-pane>
            <el-tab-pane label="注册账号" name="register"></el-tab-pane>
          </el-tabs>
          <el-form-item prop="username">
            <el-input
              ref="username"
              v-model="loginForm.username"
              placeholder="用户名称"
              name="username"
              type="text"
              
            >
            <svg-icon slot="prefix" icon-class="user" />
            </el-input>
          </el-form-item>
          <!-- <el-tooltip v-model="capsTooltip" content="Caps lock is On" placement="right" manual> -->
            <el-form-item prop="password">
              <el-input
                :key="passwordType"
                ref="password"
                v-model="loginForm.password"
                :type="passwordType"
                placeholder="密码"
                name="password"
                
                @keyup.native="checkCapslock"
                @blur="capsTooltip = false"
                @keyup.enter.native="handleLogin"
              >
              <svg-icon slot="prefix" icon-class="password" />
              <svg-icon  slot="suffix" @click="showPwd" :icon-class="passwordType === 'password' ? 'eye' : 'eye-open'" />
              </el-input>
            </el-form-item>
          <!-- </el-tooltip> -->
          <div class="index-btn-wrap">
            <el-button
              v-if="activeName === 'login'"
              :loading="loading"
              type="primary"
              style="width:100%;margin-bottom:30px;"
              @click.native.prevent="handleLogin"
            >登录</el-button>
            <el-button
              v-else
              :loading="loading"
              type="primary"
              style="width:100%;margin-bottom:30px;"
              @click.native.prevent="handleRegister"
            >注册</el-button>
          </div>
        </el-form>
        <el-dialog title="Or connect with" :visible.sync="showDialog">
          Can not be simulated on local, so please combine you own business simulation! ! !
          <br>
          <br>
          <br>
          <social-sign />
        </el-dialog>
      </div>
    </div>
  </div>
</template>

<script>
import { validUsername } from '@/utils/validate'
import SocialSign from './components/SocialSignin'

export default {
  name: 'Login',
  components: { SocialSign },
  data() {
    const validateUsername = (rule, value, callback) => {
      if (!validUsername(value)) {
        callback(new Error('Please enter the correct user name'))
      } else {
        callback()
      }
    }
    const validatePassword = (rule, value, callback) => {
      if (value.length < 6) {
        callback(new Error('The password can not be less than 6 digits'))
      } else {
        callback()
      }
    }
    return {
      loginForm: {
        username: '',
        password: ''
      },
      loginRules: {
        username: [{ required: true, trigger: 'blur', message: '请输入账号', }],
        password: [{ required: true, trigger: 'blur', message: '请输入密码', }]
      },
      activeName:'login',
      passwordType: 'password',
      capsTooltip: false,
      loading: false,
      showDialog: false,
      redirect: undefined,
      otherQuery: {}
    }
  },
  watch: {
    $route: {
      handler: function(route) {
        const query = route.query
        if (query) {
          this.redirect = query.redirect
          this.otherQuery = this.getOtherQuery(query)
        }
      },
      immediate: true
    }
  },
  created() {
    // window.addEventListener('storage', this.afterQRScan)
  },
  mounted() {
    // if (this.loginForm.username === '') {
    //   this.$refs.username.focus()
    // } else if (this.loginForm.password === '') {
    //   this.$refs.password.focus()
    // }
  },
  destroyed() {
    // window.removeEventListener('storage', this.afterQRScan)
  },
  methods: {
    checkCapslock(e) {
      const { key } = e
      this.capsTooltip = key && key.length === 1 && (key >= 'A' && key <= 'Z')
    },
    showPwd() {
      if (this.passwordType === 'password') {
        this.passwordType = ''
      } else {
        this.passwordType = 'password'
      }
      this.$nextTick(() => {
        this.$refs.password.focus()
      })
    },
    handleRegister(){
      this.$refs.loginForm.validate(valid => {
        if (valid) {
          this.$message({
            message: '恭喜你注册成功，请登录',
            type: 'success'
          });
          this.activeName = 'login';
          this.$refs.loginForm.resetFields();
        } else {
          console.log('error submit!!')
          return false
        }
      })
    },
    handleLogin() {
      this.$refs.loginForm.validate(valid => {
        if (valid) {
          this.loading = true
          this.$store.dispatch('user/login', this.loginForm)
            .then(() => {
              this.$router.push({ path: this.redirect || '/', query: this.otherQuery })
              this.loading = false
            })
            .catch(() => {
              this.loading = false
            })
        } else {
          console.log('error submit!!')
          return false
        }
      })
    },
    getOtherQuery(query) {
      return Object.keys(query).reduce((acc, cur) => {
        if (cur !== 'redirect') {
          acc[cur] = query[cur]
        }
        return acc
      }, {})
    }
    // afterQRScan() {
    //   if (e.key === 'x-admin-oauth-code') {
    //     const code = getQueryObject(e.newValue)
    //     const codeMap = {
    //       wechat: 'code',
    //       tencent: 'code'
    //     }
    //     const type = codeMap[this.auth_type]
    //     const codeName = code[type]
    //     if (codeName) {
    //       this.$store.dispatch('LoginByThirdparty', codeName).then(() => {
    //         this.$router.push({ path: this.redirect || '/' })
    //       })
    //     } else {
    //       alert('第三方登录失败')
    //     }
    //   }
    // }
  }
}
</script>

<style lang="scss">
/* 修复input 背景不协调 和光标变色 */
/* Detail see https://github.com/PanJiaChen/vue-element-admin/pull/927 */

$bg:#283443;
$light_gray:#fff;
$cursor: #fff;

@supports (-webkit-mask: none) and (not (cater-color: $cursor)) {
  .login-container .el-input input {
    // color: $cursor;
  }
}

/* reset element-ui css */
.login-container {
   .el-input {
     display: inline-block;
     height: 47px;
     width: 100%;

    input {
  //     background: transparent;
  //     border: 0px;
  //     -webkit-appearance: none;
  //     border-radius: 0px;
  //     padding: 12px 5px 12px 15px;
  //     color: $light_gray;
      //  height: 47px;
      //  caret-color: $cursor;

      &:-webkit-autofill {
        box-shadow: 0 0 0px 1000px $bg inset !important;
         -webkit-text-fill-color: $cursor !important;
       }
    }
   }

  .el-form-item {
    margin-bottom: 32px;
    // border: 1px solid #dcdfe6;
    // background: rgba(0, 0, 0, 0.1);
    // border-radius: 5px;
    // color: #454545;
  }
  .el-tabs__nav-wrap::after {
    content: "";
    position: absolute;
    left: 0;
    bottom: 0;
    width: 0;
    height: 0px;
}
.el-tabs__nav-scroll{
  text-align: center;
}
.el-tabs__nav{
  float:none;
  margin: 0 auto;
  display: inline-block;
 
}
}
.index-btn-wrap{
  display: flex;
  flex-direction: row;
  justify-content: space-between;
}
</style>

<style lang="scss" scoped>
$bg:#2d3a4b;
$dark_gray:#889aa4;
$light_gray:#eee;

.login-container {
  min-height: 100%;
  width: 100%;
  // background-color: $bg;
  // background: #0088f4;
  overflow: hidden;
  display: flex;
  flex-direction: row;
  justify-content: center;
  align-items: center;
  background-image: url('../../assets/login/bg_icon.png');
  background-size: cover;
  background-repeat: no-repeat;
  .login-wrap{
    width: 85%;
    height: 85vh;
    background: #fff;
    border-radius: 5px;
    display: flex;
    flex-direction: row;
    justify-content: space-around;
    align-items: center;
    .login-box-left{
      width: 50%;
      box-sizing: border-box;
      text-align: center;
      img{
        width: 70%;
      }
    }
    .login-box-right{
      width: 50%;
    }
  }
  .login-form {
    position: relative;
    width: 76%;
    max-width: 100%;
    padding: 0px 0 0;
    margin: 0 auto;
    overflow: hidden;
  }

  .tips {
    font-size: 14px;
    color: #fff;
    margin-bottom: 10px;

    span {
      &:first-of-type {
        margin-right: 16px;
      }
    }
  }

  .svg-container {
    padding: 6px 5px 6px 15px;
    color: $dark_gray;
    vertical-align: middle;
    width: 30px;
    display: inline-block;
  }

  .title-container {
    position: relative;

    .title {
      font-size: 26px;
      // color: $light_gray;
      color: #1890ff;
      margin: 0px auto 40px auto;
      text-align: center;
      font-weight: bold;
    }
  }

  .show-pwd {
    position: absolute;
    right: 10px;
    top: 7px;
    font-size: 16px;
    color: $dark_gray;
    cursor: pointer;
    user-select: none;
  }

  .thirdparty-button {
    position: absolute;
    right: 0;
    bottom: 6px;
  }

  @media only screen and (max-width: 470px) {
    .thirdparty-button {
      display: none;
    }
  }
}
</style>
