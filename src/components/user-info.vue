<template>
    <div class="user-info">
        <!-- 未登录 -->
        <ul class="login-no" v-if="!user.getItem('user')">
            <li class="login" @click="goEnter"><a >登录</a></li>
        </ul>
        <!-- 已登录 -->
        <div class="login-yes" v-if="user.getItem('user')" @click="goUser">
            <div class="avertar"><img v-if="userInfo.avatar_url" :src="userInfo.avatar_url"></div>
            <div class="info">
                <p v-if="userInfo " v-text="userInfo.local.name "></p>
            </div>
        </div>
        <ul class="login-no" v-if="user.getItem('user')">
            <li class="login" @click="logOut"><a >登出</a></li>
        </ul>
    </div>
</template>
<script>
    import {
        mapGetters
    } from 'vuex';
    // import userStore from '../vuex/user';
    export default {
        replace: true,
        data() {
            return {
                user: window.window.sessionStorage
            };
        },
        computed: {
            ...mapGetters({
                userInfo: 'getUserInfo'
            })
        },
        methods: {
            goEnter() {
                this.$router.push({
                    name: 'login',
                    query: {
                        redirect: encodeURIComponent(this.$route.path)
                    }
                });
            },
            goUser() {
                this.$router.push({
                    name: 'user',
                    params: {
                        loginname: this.userInfo.loginname
                    }
                });
            },
            logOut() {
                window.window.sessionStorage.removeItem('user');
                this.$router.push({
                    name: 'login',
                    query: {
                        redirect: encodeURIComponent(this.$route.path)
                    }
                });
            }
        }
    };
</script>

<style lang="scss">

</style>
