<template>
    <div class="news">
        <text class="news-title">新闻动态</text>

         <block v-for="item in news" :key="item.id">
        <a :href="'/pages/news-detail/main?id=' + item.id">
            <div class="news-item line">
                <div class="news-item-pic">
                    <image mode="widthFix" :src="item.pic" class="news-item-image" width="" height=""></image>
                </div>
                <div class="news-item-words">
                    <div class="news-item-title"><text>{{item.title}}</text></div>
                    <div class="news-item-content"><text>{{item.intro}}</text></div>
                </div>
            </div>
        </a>
        </block>
    </div>
</template>

<script>
import config from '../../config'

export default {

    data () {
        return {
            news: []
        }
    },

    components: {
    },

    created () {
    },

    onLoad() {
        this.getNews();
    },

    methods: {
        getNews() {
            var that = this;
            wx.request({
                url: config.API_URL.NEWS,
                method: 'POST',
                data: {
                    pageindex: 3,
                    pagesize: 6,
                },
                header: {
                    'content-type': 'application/json',
                },
                success: function(res) {
                   that.news = res.data.data.list;
                },
            });
        }
    },

    watch: {
    }
}
</script>

<style scoped>
@import "../../../static/css/app.css";
</style>
