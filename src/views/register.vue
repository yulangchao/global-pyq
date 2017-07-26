<template>
    <div class="register-page">
        <nv-head page-type="登录">
        </nv-head>
        <section class="page-body"  @keypress.enter.prevent="register">
            <div class="label">
                <input class="txt" type="text" placeholder="Username" v-model="token" maxlength="36" required>
                <input class="txt" type="password" placeholder="Password" v-model="password" maxlength="36" required>
                <input class="txt" type="password" placeholder="Password" v-model="passwordconfirm" maxlength="36" required>
                <input class="txt" type="text" placeholder="Name" v-model="name" maxlength="36" required>
                <input class="txt" type="text" placeholder="Email" v-model="email" maxlength="36" required>
                <select class="add-tab" v-model="gender">
                    <option value="-1" disabled selected>Select your Gender</option>
                    <option value="0">Male</option>
                    <option value="1">Female</option>
                </select>
            </div>
            <div class="label">
                <a class="button" @click="register">Register</a>
            </div>
        </section>
    </div>
</template>

<script>
    import $ from 'webpack-zepto';
    import nvHead from '../components/header.vue';

    export default {
        mounted() {

        },
        data() {
            return {
                token: '',
                password: '',
                passwordconfirm: '',
                email: '',
                name: '',
                gender: -1
            };
        },
        methods: {
            register() {
                if (this.token === '' || this.password === '') {
                    this.$alert('can not be empty!');
                    return false;
                } else if (this.password !== this.passwordconfirm) {
                    this.$alert('Passwords are not matched!');
                    return false;
                }

                $.ajax({
                    type: 'POST',
                    url: 'http://us.richardyych.cc:1111/api/auth/signup',
                    data: {
                        username: this.token,
                        password: this.password,
                        email: this.email,
                        name: this.name,
                        gender: this.gender,
                        avatar_url: 'https://s-media-cache-ak0.pinimg.com/originals/c5/7e/a0/c57ea04f0b2cbdeab1d94a1d0352dfbc.jpg'
                    },
                    dataType: 'json',
                    success: (res) => {
                        this.$router.push({
                            path: 'login'
                        });
                    },
                    error: (res) => {
                        this.$alert(res.response.split('<pre>')[1].split('</pre>')[0]);
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
            .add-tab {
                margin: 10px 0;
                padding: 12px 0;
                border: 1px solid #4fc08d;
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
