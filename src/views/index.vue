<template>
    <div>
        <img class="index" src="../assets/images/index.png">
    </div>
</template>
<script>
    require('../assets/scss/CV.scss');
    require('../assets/scss/iconfont/iconfont.css');
    require('../assets/scss/github-markdown.css');

    export default {
        mounted() {
            setTimeout(() => {
                if (window.window.sessionStorage.length === 0) {
                    this.$router.push({
                        name: 'login'
                    });
                } else {
                    this.$router.push({
                        name: 'list'
                    });
                }
            }, 0);
        }
    };
</script>
<style lang="scss">
    .index {
        width: 100%;
        background-color: #fff;
        margin-top: 40%;
    }
</style>
