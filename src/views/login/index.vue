<template>
  <div class="login-container">
    <div class="loginBox">
      <el-form ref="loginForm" :model="loginForm" :rules="loginRules" class="login-form" auto-complete="on" label-position="left">
        <div class="title-container">
          <img src="../../assets/common/logo.png" alt="">
        </div>
        <el-form-item class="formItem" prop="username">
          <span class="svg-container">
            <i class="el-icon-mobile" />
          </span>
          <el-input
            ref="username"
            v-model="loginForm.username"
            class="loginInput"
            placeholder="Username"
            name="username"
            type="text"
            tabindex="1"
            auto-complete="on"
          />
        </el-form-item>

        <el-form-item class="formItem" prop="password">
          <span class="svg-container">
            <i class="el-icon-lock" />
          </span>
          <el-input
            :key="passwordType"
            ref="password"
            v-model="loginForm.password"
            class="loginInput"
            :type="passwordType"
            placeholder="Password"
            name="password"
            tabindex="2"
            auto-complete="on"
            @keyup.enter.native="handleLogin"
          />
          <span class="show-pwd" @click="showPwd">
            <svg-icon :icon-class="passwordType === 'password' ? 'eye' : 'eye-open'" />
          </span>
        </el-form-item>
        <el-checkbox  v-model="checked"  class="mima">记住密码</el-checkbox>
        <el-button class="loginBtn" :loading="loading" type="primary" @click.native.prevent="handleLogin">
          立即登录
        </el-button>

        <div class="tips">
          <span style="margin-right:20px;">username: admin</span>
          <span> password: any</span>
        </div>

      </el-form>
      <div class="left">
        <img src="../../assets/common/contentBg.png" alt="">
      </div>
    </div>
  </div>
</template>

<script>

export default {
  name: 'Login',
  data() {
    return {
      checked: false,
      loginForm: {
        username: 'admin',
        password: '111111'
      },
      loginRules: {
        username: [{ required: true, trigger: 'blur' }],
        password: [{ required: true, trigger: 'blur' }]
      },
      loading: false,
      passwordType: 'password',
      redirect: undefined
    }
  },
  watch: {
    $route: {
      handler: function(route) {
        this.redirect = route.query && route.query.redirect
      },
      immediate: true
    }
  },
  methods: {
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
    handleLogin() {
      this.$refs.loginForm.validate(valid => {
        if (valid) {
          this.loading = true
          this.$store.dispatch('user/login', this.loginForm).then(() => {
            this.$router.push({ path: this.redirect || '/' })
            this.loading = false
          }).catch(() => {
            this.loading = false
          })
        } else {
          console.log('error submit!!')
          return false
        }
      })
    }
  }
}
</script>

<style lang="scss">
/* 修复input 背景不协调 和光标变色 */
/* Detail see https://github.com/PanJiaChen/vue-element-admin/pull/927 */

$bg:#283443;
$light_gray:black;
$cursor: #fff;

@supports (-webkit-mask: none) and (not (cater-color: $cursor)) {
  .login-container .el-input input {
    color: $cursor;
  }
}
.mima{
  position: absolute;
  left: 80px;
  bottom: 140px;
  .el-checkbox__inner{
    border: #ffb200 1px solid;
  }
}
.el-checkbox__input.is-checked+.el-checkbox__label {
  color: #ffb200;
}
.el-checkbox__input.is-checked .el-checkbox__inner {
  background-color: #ffb200;
  border-color: #ffb200;
}
/* reset element-ui css */
.login-container {
  .loginBox{
    width: 958px;
    height: 516px;
    background-color: #fff;
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%,-50%);
    border-radius: 40px;
    box-shadow: 0 0 20px rgb(93 93 93 / 33%);
    display: flex;
    .login-form{
      flex: 2;
      .formItem{
        width: 280px;
        height: 50px;
        margin-left: 40px;
        margin-bottom:30px;
        position: relative;
        top: -30px;
        .el-icon-mobile, .el-icon-lock{
          font-size: 20px;
        }
        .loginInput{
          outline: #ffb200;
          // border: #ffb200 1px solid;
          position: relative;
          left: 11px;
          background-color: #f8f5f5;
        }
      }
      .loginBtn{
        width: 280px;
        height: 50px;
        margin-left: 40px;
        margin-bottom:30px;
        background: #ffb200;
        border-radius: 8px;
        box-shadow: 0 2px 9px 1px rgb(255 178 0 / 47%);
        font-size: 16px;
        font-weight: 600;
        text-align: center;
        color: #332929;
        line-height: 22px;
        border: 0;
      }
    }
    .left{
      flex: 3;
      img{
        width: 101%;
        height: 100%;
      }
    }
  }
  .el-input {
    display: inline-block;
    height: 47px;
    width: 85%;

    input {
      background: transparent;
      border: 0px;
      -webkit-appearance: none;
      border-radius: 0px;
      padding: 12px 5px 12px 15px;
      color: $light_gray;
      height: 47px;
      caret-color: $cursor;

      &:-webkit-autofill {
        box-shadow: 0 0 0px 1000px $bg inset !important;
        -webkit-text-fill-color: $cursor !important;
      }
    }
  }

  .el-form-item {
    border: 1px solid rgba(255, 255, 255, 0.1);
    background: rgba(0, 0, 0, 0.1);
    border-radius: 5px;
    color: #454545;
  }
}
</style>

<style lang="scss" scoped>
$bg:#fff;
$dark_gray:#889aa4;
$light_gray:#eee;

.login-container {
  min-height: 100%;
  width: 100%;
  background-color: $bg;
  overflow: hidden;

  .login-form {
    position: relative;
    width: 520px;
    max-width: 100%;
    padding: 160px 35px 0;
    margin: 0 auto;
    overflow: hidden;
  }

  .formCheckbox{
    .el-checkbox__inner{
       border: #ffb200 1px solid;
      }
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
    top: -90px;
    text-align: center;
    img {
      width: 150px;
      height: 64px;
    }

    .title {
      font-size: 26px;
      color: $light_gray;
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
}
</style>
