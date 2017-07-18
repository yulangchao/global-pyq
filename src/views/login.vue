<template>
    <div class="login-page">
        <nv-head page-type="登录">
        </nv-head>
        <section class="page-body">
            <div class="label">
                <input class="txt" type="text" placeholder="Username" v-model="token" maxlength="36">
                <input class="txt" type="text" placeholder="Password" v-model="password" maxlength="36">
            </div>
            <div class="label">
                <a class="button" @click="logon">登录1</a>
            </div>
        </section>
    </div>
</template>

<script>
    import $ from 'webpack-zepto';
    import nvHead from '../components/header.vue';

    export default {
        mounted() {
            if (!window.window.sessionStorage.getItem('user')) {
                this.$router.push({
                    name: 'login'
                });
            } else {
                this.$router.push({
                    name: 'list'
                });
            }
        },
        data() {
            return {
                token: '',
                password: ''
            };
        },
        methods: {
            logon() {
                if (this.token === '') {
                    this.$alert('can not be empty!');
                    return false;
                }
                $.ajax({
                    type: 'POST',
                    url: 'http://us.richardyych.cc/api/auth/login',
                    data: {
                        username: this.token,
                        password: this.password
                    },
                    dataType: 'json',
                    success: (res) => {
                        delete res['local']['password'];
                        res.loginname = res.local.name;
                        res.avatar_url = 'https://avatars0.githubusercontent.com/u/3118295?v=3&s=120';
                        window.window.sessionStorage.user = JSON.stringify(res);
                        this.$store.dispatch('setUserInfo', res);
                        let redirect = decodeURIComponent(this.$route.query.redirect || '/');
                        this.$router.push({
                            path: redirect
                        });
                    },
                    error: (res) => {
                        var error = JSON.parse(res.responseText);
                        this.$alert(error.error_msg);
                    }
                });
            }
        },
        components: {
            nvHead
        }
    };
</script>
<style lang="scss">
    .page-body {
        padding: 50px 15px;
        min-height: 400px;
        background-color: #fff;
        .label {
            display: inline-block;
            width: 100%;
            margin-top: 15px;
            position: relative;
            left: 0;
            top: 0;
            .txt {
                padding: 12px 0;
                border: none;
                border-bottom: 1px solid #4fc08d;
                background-color: transparent;
                width: 100%;
                font-size: 14px;
                color: #313131;
            }
            .button {
                display: inline-block;
                width: 99%;
                height: 42px;
                line-height: 42px;
                border-radius: 3px;
                color: #fff;
                font-size: 16px;
                background-color: #4fc08d;
                border: none;
                border-bottom: 2px solid #3aa373;
                text-align: center;
                vertical-align: middle;
            }
            .file {
                position: absolute;
                top: 0;
                left: 0;
                height: 42px;
                width: 48%;
                outline: medium none;
                opacity: 0;
            }
        }
    }
</style>
