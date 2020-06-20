<template>
    <div>
        <div class="loading" v-if="isLoading">
            <!-- 在数据未返回时显示这个加载动画。 -->
            <img src="../assets/加载.gif" alt="加载动画">
        </div>

        <div v-else>
            <div class="topic_header">
                <div class="topic_title">{{ post.title }}
                    <ul class="clearfix">
                        <li>发布于 {{post.create_at | formatDate}}</li>
                        <li>作者 {{post.author.loginname}}</li>
                        <li>{{post.visit_count}} 次浏览</li>
                        <li>来自 {{post|tabFormatter}}</li>
                    </ul><br>
                </div>
                <div v-html="post.content" class="topic_content"></div>

                <div class="writeBack">
                    <div class="top_bar">回复</div>
                    <div v-for="(reply,index) in post.replies" class="replySec">
                        <div class="replyUp">
                            <router-link :to="{
                            name:'user_info',
                            params:{
                                name:reply.author.loginname
                            }
                        }">
                                <img :src="reply.author.avatar_url" alt="📱头像">
                            </router-link>
                            <router-link :to="{
                            name:'user_info'
                        }">
                                <span>{{reply.author.loginname}}</span>
                            </router-link>
                            <span>
                                {{index+1}}楼
                            </span>
                            <span v-if="reply.ups.length> 0">
                                📱{{reply.ups.length}}
                            </span>
                            <span v-else></span>
                        </div>
                        <p v-html="reply.content"></p>

                    </div>
                </div>

            </div>

        </div>

    </div>

</template>

<script>
    export default {
        name: "Article",
        data() {
            return {
                isLoading: false,
                post: {  //代表当前文章页的所有内容和属性
                }
            }
        },
        methods: {
            getArticleData() {
                // this.$http.get('https://cnodejs.org/api/v1/topic/', this.$route.params.id)
                this.$http.get(`https://cnodejs.org/api/v1/topic/${this.$route.params.id}`)
                    .then(res => {
                        if (res.data.success == true) {
                            this.isLoading = false
                            this.post = res.data.data
                        }
                    })
                    .catch(err => {
                        console.log(err)
                    })
            }
        },
        beforeMount() {
            this.isLoading = true
            this.getArticleData()
        },
        watch: {  //检测路由变化执行函数
            '$route'(to, from) {
                this.getArticleData()
            }
        }
    }
</script>

<style>
    * {
        box-sizing: border-box;
    }

    .clearfix::after {
        display: block;
        clear: both;
        content: '';
    }

    .topic_header {
        background: #fff;
        border-radius: 3px;
        position: absolute;
        top: 65px;
        left: 60px;
        width: 835px;
        padding: 15px;
    }

    .topic_header>.topic_title {
        font-size: 22px;
        color: black;
        font-weight: 700;
    }


    .topic_header>.topic_title:before {
        content: '';
        height: 2px;
        width: 100%;
        background: #E5E5E5;
        position: absolute;
        top: 100px;
        left: 0;
    }

    .topic_header>.topic_title>ul {
        margin-left: -35px;
    }

    .topic_header>.topic_title>ul>li {
        float: left;
        margin: 0 10px;
        color: #838383;
        font-size: 12px;
        font-weight: 400;
    }

    .topic_content img {
        width: 100%;
    }

    .replyUp>a {
        color: #666666;
        font-weight: 700;
    }

    .replyUp>span {
        color: #0088cc;
    }

    .replyUp>a>img {
        height: 30px;
        width: 30px;
    }

    .replySec {
        padding-left: 10px;
        width: 100%;
        min-height: 80px;
        border-bottom: 1px solid #EEEEEE
    }

    .replySec .markdown-text p {
        margin-left: 40px;
    }



    .topic_content {
        margin-top: -40px;
    }

    .topic_content .markdown-text h2 {
        font-size: 26px;
        color: black;
        border-bottom: 1px solid #EEEEEE;
    }

    .topic_content .markdown-text p {
        font-size: 15px;
        line-height: 1.7em;
        overflow: auto;

    }

    .topic_content .markdown-text ul {
        font-size: 13px;
        line-height: 25px;
    }

    .topic_content .markdown-text a {
        text-decoration: none;
        color: #0088CC;
    }

    .topic_content .markdown-text h3 {
        font-size: 26px;
        color: black;
        border-bottom: 1px solid #EEEEEE;
    }

    .topic_content .markdown-text code {
        display: inline-block;
        width: 100%;
        background: #f3f3f3;
        font-size: 14px;
        border-radius: 0;
        padding: 0 15px;
        border: none;
        margin: 20px -10px;
        border-width: 1px 0;
    }

    .writeBack {
        background: #fff;
        border-radius: 3px;
        position: absolute;
        top: 100%;
        right: 0px;
        width: 835px;
        margin-top: 10px;
    }

    .writeBack>.top_bar {
        border-radius: 3px;
        height: 50px;
        width: 100%;
        background: #f6f6f6;
        line-height: 50px;
        padding: 0 10px;
        font-size: 14px;
        border-bottom: 1px solid #EEEEEE
    }

    .writeBack .replyUp {
        margin-top: 10px;
    }

    .replySec .markdown-text p {
        font-size: 15px;
    }

    .replySec .markdown-text a {
        text-decoration: none;
        color: #0088CC;
    }
</style>