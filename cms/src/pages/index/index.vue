<template>
    <div class="index">
        <div class="slider">
            <swiper class="swiper" :indicator-dots="indicatorDots" :autoplay="autoplay" :interval="interval" :duration="duration">
                <block v-for="item in swipers" :key="item.id">
                    <swiper-item>
                        <a :href="item.link" class="widget">
                            <image mode="widthFix" :src="item.pic" class="slide-image" width="" height=""></image>
                        </a>
                    </swiper-item>
                </block>
            </swiper>
        </div>

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

            <div class="widgets__list widgets__list_show">
                <a href="/pages/news/main" class="widget_more">
                    <text class="news-more">查看更多</text>
                    <image class="widget__arrow" src="../../../static/images/arrowright.png" mode="aspectFill" />
                    <div class="widget__line widget__line_first"></div>
                </a>
            </div>
        </div>
    </div>
</template>

<script>
import config from '../../config'

export default {

    data () {
        return {
            swipers: [],
            indicatorDots: true,
            autoplay: true,
            interval: 5000,
            duration: 1000,
            news: []
        }
    },

    components: {
    },

    created () {
    },

    onLoad() {
        this.slideshow();
        this.getNews();
    },

    methods: {
        slideshow() {
            var that = this;
            wx.request({
                url: config.API_URL.SLIDE,
                method: 'POST',
                data: {},
                header: {
                    'content-type': 'application/json',
                },
                success: function(res) {
                   that.swipers = res.data.data.list;
                },
            });
        },
        getNews() {
            var that = this;
            wx.request({
                url: config.API_URL.NEWS,
                method: 'POST',
                data: {
                    pageindex: 0,
                    pagesize: 3,
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

.index{
    background-color: #FBF9FE;
    font-family: -apple-system-font,Helvetica Neue,Helvetica,sans-serif;
    flex: 1;
    min-height: 100%;
    font-size: 32rpx;
}
.head{
    padding: 80rpx;
    line-height: 1;
}
.body{
    padding-left: 30rpx;
    padding-right: 30rpx;
    overflow: hidden;
}
.title{
    font-size: 52rpx;
}
.desc{
    margin-top: 10rpx;
    color: #888888;
    font-size: 28rpx;
}

.widgets__item{
    margin-top: 20rpx;
    margin-bottom: 20rpx;
    background-color: #FFFFFF;
    overflow: hidden;
    border-radius: 4rpx;
    cursor: pointer;
}
.widgets__info{
    display: flex;
    padding: 40rpx;
    align-items: center;
    flex-direction: row;
}
.widgets__info_show{
}
.widgets__info_show .widgets__info-img{
    transform: rotate(-90deg);
}
.widgets__info-name{
    flex: 1;
}
.widgets__info-img{
    width: 32rpx;
    height: 32rpx;
    transition: transform .4s;
    transform: rotate(90deg);
}

.widgets__list{
    display: none;
}
.widgets__list_show{
    display: block;
}
.widget{
    position: relative;
    padding-top: 26rpx;
    padding-bottom: 26rpx;
    padding-left: 40rpx;
    padding-right: 40rpx;
}
.widget_more{
    position: relative;
    padding-top: 26rpx;
    padding-bottom: 26rpx;
    padding-left: 0rpx;
    padding-right: 40rpx;
}
.widget__arrow{
    position: absolute;
    top: 28rpx;
    right: 44rpx;
    width: 32rpx;
    height: 32rpx;
}
.widget__line{
    content: " ";
    position: absolute;
    left: 40rpx;
    top: 0;
    right: 0;
    height: 2rpx;
    background-color: #F0F0F0;
}
.widget__line_first{
    left: 0;
    right: 0;
    background-color: #D8D8D8;
}
</style>
