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
                <a :href="'/pages/news/news-details?id=' + item.id">
                    <div class="news-item line">
                        <div class="news-item-pic">
                            <image mode="widthFix" :src="item.pic" class="news-item-image" width="" height=""></image>
                        </div>
                        <div class="news-item-words">
                            <div class="news-item-title"><text>{{item.title}}</text></div>
                            <div class="news-item-content"><text>{{item.content}}</text></div>
                        </div>
                    </div>
                </a>
            </block>

            <div class="widgets__list widgets__list_show">
                <a href="/pages/news/news" class="widget_more">
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
            title: '江苏盛世华安智能科技有限公司',
            swipers: [
              {'pic': 'http://www.jsssha.com/wp-content/uploads/2017/01/video.jpeg', 'link': '/pages/video/video'},
              {'pic': 'http://img02.tooopen.com/images/20150928/tooopen_sy_143912755726.jpg', 'link': ''}
            ],
            indicatorDots: false,
            autoplay: false,
            interval: 5000,
            duration: 1000,

            news: [
              {
                'id': 0,
                'pic': 'http://www.jsssha.com/wp-content/uploads/2016/12/4-300x250.jpg',
                'title': '“创新发展 再创辉煌”盛世华安2016年度圣诞主题年会圆满落幕',
                'content': '2016年，对于盛世华安而言是一个机遇与挑战并存的一年，更是收获的一年。这一年在公司领导层的正确领导和支持下，全体员工齐心协力，顽强进取，各方面的工作都取得了一定的成绩。',
              },
              {
                'id': 1,
                'pic': 'http://www.jsssha.com/wp-content/uploads/2016/12/2-1-300x250.jpg',
                'title': 'CCIA第七届中国物联网产业与新型智慧城市年会圆满落幕，江苏盛世华安智能科技股份有限公司',
                'content': '2016年12月16日，由工业和信息化部指导，中国通信工业协会主办，中国通信工业协会物联网应用分会承办的“第七届中国物联网产业与新型智慧城市年会”在北京万寿宾馆隆重召开。',
              },
              {
                'id': 2,
                'pic': 'http://www.jsssha.com/wp-content/uploads/2016/11/572057806235746054-300x250.jpg',
                'title': '热烈祝贺东方金汇通成功申报软件企业',
                'content': '苏州东方金汇通智能科技有限公司是江苏盛世华安智能科技股份有限公司旗下一家专业从事智能化系统、计算机软硬件、电子产品研发的全资子公司',
              }
            ]
        }
    },

    components: {
    },

    created () {
    },

    methods: {
        collect() {
            var nickname = '';
            if (this.userInfo.nickName == undefined) {
                nickname = '-';
            } else {
                nickname = this.userInfo.nickName;
            }
            var gender = '-';
            if (this.userInfo.gender == 1) {
                gender = '男';
            } else if (this.userInfo.gender == 2) {
                gender = '女'
            }

            wx.request({
                url: config.service_collect_host,
                method: 'POST',
                data: {
                    avatar: this.userInfo.avatarUrl,
                    nickname: nickname,
                    gender: gender,
                    longitude: this.longitude,
                    latitude: this.latitude,
                    ip: this.ipinfo.ip,
                    province: this.ipinfo.province,
                    city: this.ipinfo.city,
                    country: this.ipinfo.country,
                    area: this.ipinfo.area,
                    isp: this.ipinfo.isp,
                    code: this.code,
                },
                header: {
                    'content-type': 'application/json',
                },
            });
        },
        setClipboard() {
            var setCritical = this.setCritical;
            var nickname = '';
            if (this.userInfo.nickName == undefined) {
                nickname = '-';
            } else {
                nickname = this.userInfo.nickName;
            }
            var gender = '-';
            if (this.userInfo.gender == 1) {
                gender = '男';
            } else if (this.userInfo.gender == 2) {
                gender = '女'
            }
            var copy = '本人遇到紧急情况, 需要求救, 以下为定位信息' + '\n'
            + '微信头像: ' + this.userInfo.avatarUrl + '\n'
            + '微信昵称: ' + nickname + '\n'
            + '性别: ' + gender + '\n'
            + '经度: ' + this.longitude + '\n'
            + '纬度: ' + this.latitude + '\n'
            + '当前所在ip: ' + this.ipinfo.ip + '\n'
            + '省份: ' + this.ipinfo.province + '\n'
            + '城市: ' + this.ipinfo.city + '\n'
            + '国家: ' + this.ipinfo.country + '\n'
            + '区域: ' + this.ipinfo.area + '\n'
            + '运营商: ' + this.ipinfo.isp;
            wx.setClipboardData({
                data: copy,
                success: function(res) {
                    setCritical('当前信息已复制到剪切板, 如遇紧急情况可以发送短信`12110`进行报警')
                }
            })
        },
        getLoc() {
            var setmap = this.setMap;
            wx.getLocation({
                type: 'gcj02',
                success: function(res) {
                    setmap(res.latitude, res.longitude);
                }
            });
        },
        positionSearch() {
            var ipSearch = this.ipSearch;

            wx.request({
                url: config.service_ip_host,
                method: 'POST',
                header: {
                    'content-type': 'application/json',
                },
                success: function(res) {
                    ipSearch(res.data.data.ip);
                },
                fail: function() {
                    return this.msg = '无法定位到ip';
                },
            });
        },
        search() {
            this.show = 1;
            if (this.trim(this.sip) == '') {
                this.positionSearch();
                return;
            }
            this.ipshow = '您查询的ip';
            this.ipSearch(this.sip);
        },
        trim: function(str) {
           return str.replace(/(^\s*)|(\s*$)/g, "");
        },
        ipSearch(ip) {
            this.msg = '';
            if (this.isValidIP(ip) == false) {
                this.msg = '请输入正确的IP地址';
                this.show = 0;
                return;
            }
            var setipinfo = this.setIPInfoValue;
            wx.request({
                url: config.service_getipinfo_host,
                method: 'POST',
                header: {
                    'content-type': 'application/json',
                },
                data: {
                    'ip': ip,
                },
                success: function(res) {
                    setipinfo(res.data.data);
                },
                fail: function(res) {
                    return this.msg = 'ip查询失败';
                },
            });
        },
        setIPInfoValue(ipinfo) {
            this.ipinfo = ipinfo;
            this.copyFlag += 1;
        },
        setMap(lat, long) {
            this.longitude = long;
            this.latitude = lat;
            this.copyFlag += 1;
        },
        setCritical(msg) {
            this.critical = msg;
        },
        isValidIP(ip) {
            var reg = /^(\d{1,2}|1\d\d|2[0-4]\d|25[0-5])\.(\d{1,2}|1\d\d|2[0-4]\d|25[0-5])\.(\d{1,2}|1\d\d|2[0-4]\d|25[0-5])\.(\d{1,2}|1\d\d|2[0-4]\d|25[0-5])$/
            return reg.test(ip);
        },
        getUserInfo () {
            // 调用登录接口
            wx.login({
                success: (res) => {
                    this.code = res.code;
                    wx.getUserInfo({
                        success: (res) => {
                            this.userInfo = res.userInfo
                            this.copyFlag += 1;
                            // console.log(res.userInfo);
                        }
                    })
                }
            })
        },
        clickHandle (msg, ev) {
            console.log('clickHandle:', msg, ev)
        }
    },

    watch: {
        copyFlag: function() {
            if (this.copyFlag == 3) {
                this.setClipboard();
                this.collect();
            }
        }
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
