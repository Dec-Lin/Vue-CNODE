<template>
    <div class="UserInfo">
        <div class="loading" v-if="isLoading">
            <!-- 在数据未返回时显示这个加载动画。 -->
            <img src="../assets/加载.gif" alt=" 加载动画">
        </div>

        <div class="UserInfomation">
            <div class="homepage"> 主页 / </div>
            <section>
                <img :src="userinfo.avatar_url" alt="📱头像">
                <span> {{userinfo.loginname}} </span>
                <p>{{userinfo.score}}积分</p>
                <p>注册时间：{{userinfo.create_at | formatDate}}</p>
            </section>
            <div class="reply_topic">
                <p class="topBar">回复的主题</p>
                <div class="lll">
                    <ul>
                        <li v-for="item in userinfo.recent_replies">
                            <router-link :to="{
                            name:'post_content',
                            params:{
                            id:item.id
                        }
                        }">
                                {{item.title}}
                            </router-link>
                        </li>
                    </ul>
                </div>
            </div>
            <div class="createdTheme">
                <p class="topBar">创建的主题</p>
                <div class="lll">
                    <ul>
                        <li v-for="item in userinfo.recent_topics">
                            <router-link :to="{
                            name:'post_content',
                            params:{
                            id:item.id
                        }
                        }">
                                {{item.title}}
                            </router-link>
                        </li>
                    </ul>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
    export default {
        name: 'UserInfo',
        data() {
            return {
                loading: false,
                userinfo: {}
            }
        },
        methods: {
            getData() {
                this.$http.get(`https://cnodejs.org/api/v1/user/${this.$route.params.name}`)
                    .then(res => {
                        this.isLoading = false //数据加载成功,去除加载动画
                        this.userinfo = res.data.data
                    })
                    .catch(function (err) {
                        console.log(err)
                    })
            }
        },
        beforeMount() {
            this.isLoading = true //数据加载成功之前加载动画
            this.getData()  // 在页面加载之前获取数据
        }
    }
</script>

<style scoped>
    a {
        text-decoration: none;
        color: #0088CC;
    }

    .UserInfo {

        border-radius: 3px;
        position: absolute;
        top: 65px;
        left: 60px;
        width: 835px;
    }

    .homepage {
        height: 40px;
        width: 100%;
        background: #f6f6f6;
        color: #80BD01;
        border-bottom: 1px solid #E5E5E5;
        border-radius: 5px 5px 0 0;
        font-size: 14px;
        line-height: 40px;
        padding-left: 10px;
    }

    .topBar {
        height: 40px;
        width: 100%;
        background: #f6f6f6;
        color: black;
        border-bottom: .5px solid #E5E5E5;
        border-radius: 5px 5px 0 0;
        font-size: 14px;
        line-height: 40px;
        padding-left: 10px;
    }

    .UserInfomation section {
        width: 100%;
        min-height: 100px;
        background: #fff;
        color: #778087;
        padding: 10px;
    }

    .UserInfomation section img {
        height: 50px;
        width: 50px;
    }

    .UserInfomation .reply_topic .lll {
        background: #fff;
        margin-top: -14px;
    }

    .UserInfomation .createdTheme .lll {
        background: #fff;
        margin-top: -14px;
    }

    .UserInfomation .reply_topic ul li,
    .UserInfomation .createdTheme ul li {
        list-style: none;
        height: 50px;
        line-height: 50px;
        width: 100%;
        color: red;
        font-size: 16px;
        border-bottom: .5px solid #F0F0F0;
    }
</style>