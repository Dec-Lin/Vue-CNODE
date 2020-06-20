<template>
    <div class="slidebar">
        <div class="authorSummary">
            <div class="topBar">作者</div>
            <router-link :to="{
                name:'user_info',
                params:{
                    name:userinfo.loginname
                }
            }">
                <img :src="userinfo.avatar_url" alt="">
            </router-link>
        </div>
        <div class="recent_topics">
            <div class="topBar">作者最近主题</div>
            <ul>
                <li v-for="list in topicAmount">
                    <router-link :to="{
                        name:'post_content',
                        params:{
                            id:list.id,
                            name:list.author.loginname
                        }
                    }">
                        {{list.title}}
                    </router-link>
                </li>
            </ul>
        </div>
        <div class="recent_replies">
            <div class="topBar">作者最近回复</div>
            <ul>
                <li v-for="list in replyAmount">
                    <router-link :to="{
                        name:'post_content',
                        params:{
                            id:list.id,
                            name:list.author.loginname
                        }
                    }">
                        {{list.title}}
                    </router-link>
                </li>
            </ul>
        </div>
    </div>
</template>

<script>
    export default {
        name: 'SlideBar',
        data() {
            return {
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
            this.isLoading = true
            this.getData()
        },
        computed: {
            topicAmount() {
                if (this.userinfo.recent_topics) {
                    return this.userinfo.recent_topics.slice(0, 5)
                } else {
                    return this.userinfo.recent_topics
                }
            },
            replyAmount() {
                if (this.userinfo.recent_replies) {
                    return this.userinfo.recent_replies.slice(0, 5)
                } else {
                    return this.userinfo.recent_replies
                }
            }
        }
    }
</script>

<style scoped>
    .slidebar {
        position: absolute;
        top: 70px;
        right: 65px;
        width: 285px;
    }

    .authorSummary img {
        width: 30px;
        height: 30px;
    }

    .authorSummary {
        background: white;
        border-radius: 3px;
    }

    .recent_topics {
        margin-top: 20px;
        background: white;
        border-radius: 3px;
    }

    .recent_replies {
        margin-top: 20px;
        background: white;
        border-radius: 3px;
    }

    .topBar {
        color: #444444;
        background: #f6f6f6;
        height: 40px;
        border-radius: 3px 3px 0 0;
        padding: 10px;
    }

    img {
        height: 48px;
        width: 48px;
        border-radius: 3px;
        margin: 10px;
    }

    ul a {
        font-size: 12px;
        text-decoration: none;
        color: #778087;
    }

    .recent_replies ul,
    .recent_topics ul {
        margin-top: 0px;
        margin-bottom: 0px;
        list-style: none;
        padding-left: 14px;
    }

    li {
        padding: 3px 0;
    }
</style>