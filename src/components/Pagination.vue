<template>
    <div class="pagination">
        <button @click="changeBtn">首页</button>
        <button @click="changeBtn">«</button>
        <button v-if="judge">...</button>

        <button v-for="btn in pagebtns" :class="[{currentPage:btn==currentPage},'pagebtn']" @click="changeBtn(btn)">
            {{btn}}
        </button>
        <button @click="changeBtn">»</button>
    </div>
</template>

<script>
    import $ from 'jquery'

    export default {
        name: 'Pagination',
        data() {
            return {
                pagebtns: [1, 2, 3, 4, 5, '...'],
                currentPage: 1,
                judge: false
            }
        },
        methods: {
            changeBtn(page) {
                if (typeof page != 'number') {
                    switch (page.target.innerText) {
                        case '«':
                            $('button.currentPage').prev().click();
                            //jQuery方法，点击当前按钮的上一个按钮
                            break;
                        case '»':
                            $('button.currentPage').next().click();
                            //jQuery方法，点击当前按钮的下一个按钮
                            break;
                        case '首页':
                            this.pagebtns = [1, 2, 3, 4, 5, '...'];
                            this.changeBtn(1)
                            break;
                        default:
                            break;
                    }
                    return;
                }

                this.currentPage = page
                if (page > 4) {
                    this.judge = true
                } else {
                    this.judge = false
                }
                if (page == this.pagebtns[4]) {
                    this.pagebtns.shift()//移除第一个元素
                    this.pagebtns.splice(4, 0, this.pagebtns[3] + 1) //添加最后一个元素
                } else if (page == this.pagebtns[0] && page != 1) {
                    this.pagebtns.unshift(this.pagebtns[0] - 1)//先在第一个位置添加一个
                    this.pagebtns.splice(5, 1)//移除最后一个元素
                }
                this.$emit('handleList', this.currentPage)
            }
        }
    }
</script>

<style scoped>
    .currentPage {
        background: rgb(195, 195, 201);
        color: white;
    }

    .pagebtn {
        color: black
    }

    .pagination button {
        width: 50px;
        height: 25px;
        border: 1px solid #DDDDDD;
        border-style: none;
    }
</style>