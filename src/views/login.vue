<template>
    <div class="wh100p login-box">
        <div class="login-page">
            <div class="form">
                <form class="register-form" v-if="canRegister">
                    <input type="text" placeholder="name"/>
                    <input type="password" placeholder="password"/>
                    <input type="text" placeholder="email address"/>
                    <button>create</button>
                    <p class="message">Already registered? <a href="#">Sign In</a></p>
                </form>
                <form class="login-form" @submit.prevent="login">
                    {{login.username}}
                    <input v-model="loginUsername" type="text" placeholder="用户名： root"/>
                    <input v-model="loginPassword" type="password" placeholder="密码: root"/>
                    <div class="captcha clearfix" v-if="needCaptcha">
                        <input v-model="loginCaptcha" type="text" placeholder="右侧验证码"/>
                        <div class="captcha-img-box">
                            <img src="~assets/captcha.jpg" alt="验证码">
                        </div>
                    </div>
                    <button type="submit">登录</button>
                    <p class="message" v-if="canRegister">没有账号? <a href="#">注册账号</a></p>
                </form>
            </div>
        </div>
    </div>
</template>

<script>
    import $ from 'assets/jquery-vendor';
    import systemService from 'services/systemService';

    export default {
        data() {
            return {
                loginUsername: '',
                loginPassword: '',
                loginCaptcha: '',
                needCaptcha: false,
                canRegister: false,
                loginTime: 0
            };
        },
        created() {
            $('body').on('click', '.login-page .message a', function () {
                $('form').animate({height: 'toggle', opacity: 'toggle'}, 'slow');
            });
        },
        computed: {
            userInfo() {
                return this.$store.state.user.info;
            }
        },
        methods: {
            login() {
                if (!this.loginUsername || !this.loginPassword) {
                    this.$alert('用户名或密码不能为空');
                } else if (this.loginUsername === 'root' && this.loginPassword === 'root') {
                    systemService.login().then(({data}) => {
                        console.log(data);
                        this.$store.commit('setUserInfo', data.data);
                        this.$store.commit('dialogInit');
                        this.$router.push('/main/');
                    });
                } else if (this.loginTime < 3) {
                    this.loginTime++;
                    this.needCaptcha = false;
                } else {
                    this.needCaptcha = true;
                }
            }
        }
    };

</script>

<style lang="scss" rel="stylesheet/scss" scoped>
    .login-box {
        display: flex;
        justify-content: center;
        align-items: center;
        background: #f2f2f2;
    }

    .login-page {
        width: 360px;

    }

    .form {
        position: relative;
        z-index: 1;
        background: #FFFFFF;
        max-width: 360px;
        /*margin: 0 auto 100px;*/
        padding: 40px;
        text-align: center;
        box-shadow: 0 0 20px 0 rgba(0, 0, 0, 0.2), 0 5px 5px 0 rgba(0, 0, 0, 0.24);
    }

    .form input {
        /*font-family: "Roboto", sans-serif;*/
        outline: 0;
        background: #f2f2f2;
        width: 100%;
        border: 0;
        margin: 0 0 15px;
        padding: 15px;
        box-sizing: border-box;
        font-size: 14px;
    }

    .captcha {
        input {
            width: 50%;
            float: left;
        }
        .captcha-img-box {
            width: 50%;
            float: left;
            display: flex;
            justify-content: center;
            align-items: center;
            height: 46px;
            img {
                max-width: 90%;
                min-height: 90%;
            }
        }
    }

    .form button {
        /*font-family: "Roboto", sans-serif;*/
        text-transform: uppercase;
        outline: 0;
        background: #4CAF50;
        width: 100%;
        border: 0;
        padding: 15px;
        color: #FFFFFF;
        font-size: 14px;
        transition: all 0.3s ease;
        cursor: pointer;
    }

    .form button:hover, .form button:active, .form button:focus {
        background: #43A047;
    }

    .form .message {
        margin: 15px 0 0;
        color: #b3b3b3;
        font-size: 12px;
    }

    .form .message a {
        color: #4CAF50;
        text-decoration: none;
    }

    .form .register-form {
        display: none;
    }

    .container {
        position: relative;
        z-index: 1;
        max-width: 300px;
        margin: 0 auto;
    }

    .container:before, .container:after {
        content: "";
        display: block;
        clear: both;
    }

    .container .info {
        margin: 50px auto;
        text-align: center;
    }

    .container .info h1 {
        margin: 0 0 15px;
        padding: 0;
        font-size: 36px;
        font-weight: 300;
        color: #1a1a1a;
    }

    .container .info span {
        color: #4d4d4d;
        font-size: 12px;
    }

    .container .info span a {
        color: #000000;
        text-decoration: none;
    }

    .container .info span .fa {
        color: #EF3B3A;
    }

    body {
        background: #76b852; /* fallback for old browsers */
        background: -webkit-linear-gradient(right, #76b852, #8DC26F);
        background: -moz-linear-gradient(right, #76b852, #8DC26F);
        background: -o-linear-gradient(right, #76b852, #8DC26F);
        background: linear-gradient(to left, #76b852, #8DC26F);
        /*font-family: "Roboto", sans-serif;*/
        -webkit-font-smoothing: antialiased;
        -moz-osx-font-smoothing: grayscale;
    }


</style>
