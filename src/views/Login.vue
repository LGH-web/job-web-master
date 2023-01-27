<template>
    <div>
        <div class="login-header">
            <div class="login-main">
                <div class="system-title">
                    高校毕业生就业管理平台
                </div>
            </div>

        </div>
        <div class="login-container">
            <div class="login-main">
                <div class="show"><img src="~@/assets/images/manage-bg.png"></div>
                <div class="box">
                    <div class="title">系统登录</div>
                    <div  class="system-title2">
<!--                    <span class="reg-login" v-if="!loginState">-->
<!--                        <router-link to="/register">注册</router-link></span>-->
<!--                        <span> | </span>-->
                        <span><router-link to="/">返回首页</router-link>
                </span>
                    </div>
                    <el-form ref="loginForm" :model="form" :rules="rules">
                        <el-form-item prop="account">
                            <el-input v-model="form.account" class="login-user"
                                      placeholder="请输入用户名"></el-input>
                        </el-form-item>
                        <el-form-item prop="password">
                            <el-input type="password" v-model="form.password" class="login-pwd"
                                      placeholder="请输入密码"></el-input>
                        </el-form-item>
                        <el-form-item prop="verifycode">
                            <el-row :span="24">
                                <el-col :span="12">
                                        <el-input v-model="form.verifycode" class="login-check"
                                                  placeholder="点击图片更换验证码"></el-input>
                                </el-col>
                                <el-col :span="12">
                                    <div class="login-code" @click="refreshCode">
                                        <!--验证码组件-->
                                        <Identify :identifyCode="identifyCode"></Identify>
                                    </div>
                                </el-col>
                            </el-row>
                        </el-form-item>
                        <el-form-item>
                            <el-select v-model="form.type" placeholder="请选择类型">
                                <el-option
                                        v-for="item in options"
                                        :key="item.value"
                                        :label="item.label"
                                        :value="item.value">
                                </el-option>
                            </el-select>
                        </el-form-item>
                        <el-form-item>
                            <el-button type="primary" @click="login" class="login-btn" :loading="false">登录</el-button>
                        </el-form-item>
                    </el-form>
                </div>
            </div>
        </div>
        <div class="login-footer">
            陇ICP备20002220号-1   201406171402972048995   甘公网安备 62010502000947号   版权所有©甘肃政法大学 通讯地址：甘肃省兰州市安宁西路6号
        </div>
    </div>
</template>

<script>
    import screenfull from "screenfull";
    import Identify from "../components/Identify";

    export default {
        name: "Login",
        data:function () {
            /* 自定义验证码规则 */
            const validateVerifycode = (rule, value, callback) => {
                if (value === '') {
                    callback(new Error('请输入验证码'))
                } else if (value !== this.identifyCode) {
                    console.log('validateVerifycode:', value);
                    callback(new Error('验证码不正确!'))
                } else {
                    callback()
                }
        }
            return {
                identifyCodes: '1234567890ABCDEFGHIGKLMNOPQRSTUVWXYZ',
                identifyCode: '',
                form: {
                    account: '',
                    password: '',
                    type: 0,
                    verifycode: ''
                },
                options: [{label: '管理员', value: 0}, {label: '企业', value: 1}, {label: '学生', value: 2}],
                rules: {
                    account: [{required: true, message: "用户名不能为空"}],
                    password: [{required: true, message: "密码不能为空"}],
                    verifycode: [{required: true, validator: validateVerifycode}]
                }
            }
        },
        components: {
            Identify
        },
        methods: {
            handleFullScreen() {
                screenfull.toggle();
            },
            handleCommand(type) {
                if (type === 'home') {
                    this.$router.push('/');
                }
            },
            login() {
                this.$refs['loginForm'].validate(valid => {
                    if (valid) {
                        //用户登录操作
                        this.$store.dispatch('login', this.form).then(res => {
                            if (res.code == 200) {
                                this.$router.push('/index');
                            }
                        })
                    }
                });
            },
            randomNum(min, max) {
                return Math.floor(Math.random() * (max - min) + min)
            },
            refreshCode() {
                this.identifyCode = "";
                this.makeCode(this.identifyCodes, 4);
            },
            //获取验证码的值
            makeCode(o, l) {
                for (let i = 0; i < l; i++) {
                    //通过循环获取字符串内随机几位
                    this.identifyCode +=
                        this.identifyCodes[this.randomNum(0, this.identifyCodes.length)];
                }
                console.log(this.identifyCode)
            },
            mounted: function () {
                this.identifyCode = "";
                //参数：（1）随机串内容。（2）验证码显示位数
                this.makeCode(this.identifyCodes, 4);
                history.pushState(null, null, document.URL);
                if (window.history && window.history.pushState) {
                    this.$(window).on('popstate', function () {
                        window.history.pushState('forward', null, '');
                        window.history.forward(1);
                    });
                    window.history.pushState('forward', null, '');  //在IE中必须得有这两行
                    window.history.forward(1);
                }
            },
            // created() {
            //     this.refreshCode()
            // }
        }
    }
</script>

<style scoped>
    .login-header {
        height: 20%;
        width: 100%;
        position: absolute;
        top: 0;
    }

    .login-container {
        width: 100%;
        position: absolute;
        top: 15%;
        bottom: 15%;
        background-color: #d6fffa;
    }

    .login-main {
        width: 1200px;
        height: 100%;
        margin: auto;
    }

    .system-title {
          font-size: 36px;
          margin-top: 20px;
          margin-left: -35px;
          font-weight: bold;
          color: #062455;
      }
    .system-title2{
        float: right;
        margin-top: -44px;
        font-size: 18px;
    }
    .show {
        float: left;
        position: absolute;
        bottom: 0;
        left: 60px;
        top: 0;
        padding-top: 50px;
        width: 750px;
    }

    .show img {
        height: 90%;
        display: block;
        margin-left: 20px;
    }

    .box {
        width: 30%;
        padding: 20px 40px;
        float: right;
        background-color: #fff;
        border-radius: 5px;
        margin-top: 0.8%;
    }

    .box .title {
        font-size: 18px;
        margin-bottom: 20px;
    }

    .login-btn {
        width: 100%;
        background-color: #0F9296;
        border: 0px;
    }

    .login-btn:hover {
        width: 100%;
        background-color: #007B73;
        border: 0px;
    }

    .login-user >>> .el-input__inner {
        background: url('~@/assets/images/user.png') no-repeat center left;
        padding-left: 25px;
    }

    .login-pwd >>> .el-input__inner {
        background: url('~@/assets/images/password.png') no-repeat center left;
        padding-left: 25px;
    }
    .login-check >>> .el-input__inner {
        background: url('~@/assets/images/check.png') no-repeat center left;
        padding-left: 25px;
    }

    .login-footer {
        position: absolute;
        bottom: 0px;
        height: 15%;
        width: 100%;
        text-align: center;
        color: #999999;
        padding-top: 40px;
        font-size: 14px;
        box-sizing: border-box;
        background-color: #F0F0EE;
    }
</style>
