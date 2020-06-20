<template>
    <div class="PostList">
        <div class="loading" v-if="isLoading">
            <!-- 在数据未返回时显示这个加载动画。 -->
            <img src="../assets/加载.gif" alt=" 加载动画">
        </div>
        <div class="posts" v-else>
            <!-- 主题帖列表 -->
            <div class="posts_header">
                <a href="#" class="tepic-tab active">全部</a>
                <a href="#" class="tepic-tab">精华</a>
                <a href="#" class="tepic-tab">分享</a>
                <a href="#" class="tepic-tab">问答</a>
                <a href="#" class="tepic-tab">招聘</a>
                <a href="#" class="tepic-tab">客户端测试</a>
            </div>
            <ul>
                <li v-for="post in posts">
                    <!-- 用户头像 -->
                    <img :src="post.author.avatar_url" alt="📱头像">
                    <span>
                        <!-- 回复量和浏览量 -->
                        <span class="allCount">
                            <span class="reply_count">{{post.reply_count}}</span>
                            / <span class="visit_count">{{post.visit_count}}</span>
                        </span>
                        <!-- 贴子的分类 -->
                        <span :class="[{put_good:(post.good==true),put_top:(post.top==true),
                        topList_tab:(post.top!=true && post.good!=true)}]">{{post | tabFormatter}}</span>
                        <!-- <span>{{post | tabFormatter}}</span> -->
                        <!-- 标题 -->
                        <router-link :to="{name:'post_content',params:{
                            id:post.id,
                            name:post.author.loginname
                        }}">
                            <span>
                                {{post.title}}
                            </span>
                        </router-link>
                        <!-- 最后回复时间 -->
                        <span class="last_reply">
                            {{post.last_reply_at | formatDate}}
                        </span>
                    </span>
                </li>
                <li>
                    <!-- 分页 -->
                    <pagination @handleList="renderList"></pagination>
                </li>
            </ul>
        </div>
    </div>
</template>

<script>
    import Pagination from './Pagination.vue'

    export default {
        name: "PostList",
        data() {
            return {
                isLoading: false,
                posts: [], //代表页面的列表数组
                postpage: 1
            }
        },
        components: {
            Pagination
        },
        methods: {
            getData() {
                this.$http.get('https://cnodejs.org/api/v1/topics', {
                    params: {
                        page: this.postpage,
                        limit: 20
                    }
                })
                    .then(res => {
                        this.isLoading = false //数据加载成功,去除加载动画
                        this.posts = res.data.data
                    })
                    .catch(function (err) {
                        console.log(err)
                    })

            },
            renderList(value) {
                this.postpage = value
                this.getData()
            }

        },
        beforeMount() { //钩子函数,与methods平级
            this.isLoading = true //数据加载成功之前加载动画
            this.getData()  // 在页面加载之前获取数据
        }
    }
</script>

<style scoped>
    ul a {
        text-decoration: none;
        color: black;
    }

    ul a:visited {
        color: #8f918f !important;
    }

    .posts_header {
        height: 45px;
        line-height: 45px;
        margin-top: 20px;
        background: #f5f5f5;
        border: none;
        padding: 0 0 0 20px;
        margin-left: 5px;
        margin-bottom: -12px;
        border-radius: 5px 5px 0 0;
    }

    .tepic-tab {
        font-size: 14px;
        padding: 3px 4px;
        color: #80bd01;
        border-radius: 3px;
        margin: 0 7px;
        text-decoration: none;
    }

    .tepic-tab.active {
        background: #80bd01;
        color: white;
        text-decoration: none;
    }


    img {
        width: 30px;
        height: 30px;
        border-radius: 5px;
    }

    .posts {
        background: #e1e1e1;
    }

    ul {
        padding: 0;
        margin-left: 5px;
    }

    .posts ul li {

        list-style: none;
        height: 50px;
        line-height: 50px;
        font-size: 15px;
        background: #fff;
        border-bottom: .5px solid rgb(204, 204, 204);
        padding: 0 10px;
    }

    .posts ul li:hover {
        background: #f5f5f5;
    }

    .posts ul img {
        vertical-align: middle;
    }

    .posts ul .reply_count {
        color: #9E78C0;
        font-size: 13px;
        vertical-align: middle;
    }

    .posts ul .visit_count {
        color: #B4B4B4;
        font-size: 10px;
        vertical-align: middle;
    }

    .put_good,
    .put_top {
        background: #80bd01;
        color: white;
        border-radius: 3px;
        font-size: 12px;
        padding: 2px 4px;
        margin: 0 3px 0 3px;
    }

    .topList_tab {
        background: #E5E5E5;
        color: #999999;
        border-radius: 3px;
        font-size: 12px;
        padding: 2px 4px;
        margin: 0 3px 0 3px;
    }

    .last_reply {
        text-align: right;
        min-width: 50px;
        display: inline-block;
        white-space: nowrap;
        float: right;
        color: #778087;
        font-size: 12px;
    }

    .allCount {
        display: inline-block;
        width: 70px;
        text-align: center;
        margin-left: 5px;
    }
</style>