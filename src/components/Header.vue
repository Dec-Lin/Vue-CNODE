<template>
    <div class="header">
        <router-link :to="{name:'root'}">
            <img src="https://static2.cnodejs.org/public/images/cnodejs_light.svg" alt="">
        </router-link>
        <ul>
            <li><a href="#">首页</a></li>
            <li><a href="#">新手入门</a></li>
            <li><a href="#">API</a></li>
            <li><a href="#">关于</a></li>
            <li><a href="#">注册</a></li>
            <li><a href="#">登录</a></li>
        </ul>
    </div>
</template>

<script>
    export default {
        name: "Header"
    }
</script>

<style scoped>
    /* scoped表示这里的样式只对当前这个组件有效 */

    .header {
        width: 100%;
        background-color: #333;
        height: 50px;
    }

    img {
        max-width: 120px;
        margin-left: 50px;
        margin-top: 10px;
    }

    ul {
        float: right;
        margin: 5px;
        margin-right: 60px;
    }

    li {
        list-style: node;
        display: inline-block;
        padding: 10px 15px;
    }

    a {
        text-decoration: none;
        color: #ccc;
        font-size: 13px;
        text-shadow: none;
    }
</style>