<template>
    <div style="height: 100%;">
        <nv-head page-type="关于" :fix-head="true" :need-add="true" ></nv-head>

        <dl class="about-info">

            <dt>About this project</dt>
            <dd>This project is based on the <a href="https://github.com/shinygang/Vue-cnodejs">
                    shinygang</a></dd>

            <dt>Source</dt>

            <dd>
                <a href="https://github.com/yulangchao/global-pyq">
                    https://github.com/yulangchao/global-pyq</a>
            </dd>

            <dt>Feedback</dt>
            <dd>
                <a href="https://github.com/yulangchao/global-pyq/issues">
                    report issues</a>
            </dd>

            <dt>Current Version</dt>
            <dd>V2.0</dd>

        </dl>
    </div>
</template>
<script>
    import nvHead from '../components/header.vue';
    export default {
        components: {
            nvHead
        }
    };
</script>
