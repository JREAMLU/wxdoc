<template>
    <div class="container">

        <div class="userinfo">
            <!-- 头像 -->
            <img class="userinfo-avatar" v-if="userInfo.avatarUrl" :src="userInfo.avatarUrl" background-size="cover" />
            <!-- 昵称 -->
            <div class="userinfo-nickname">
                <card :text="userInfo.nickName"></card>
            </div>
        </div>

        <!-- 搜索框 -->
        <div class="form-search">
            <input type="text" confirm-type="search" v-model='sip' class="form-control" placeholder="输入ip地址" />
            <button type='default' size='mini' @click='search'>搜索</button>
        </div>

        <!-- error message -->
        <div>{{msg}}</div>

        <div v-show="show === 1">
            <div class='show'>{{ipshow}}: {{ipinfo.ip}}</div>
            <div class='show'>城市id: {{ipinfo.city_id}}</div>
            <div class='show'>国家: {{ipinfo.country}}</div>
            <div class='show'>区域: {{ipinfo.area}}</div>
            <div class='show'>省份: {{ipinfo.province}}</div>
            <div class='show'>城市: {{ipinfo.city}}</div>
            <div class='show'>运营商: {{ipinfo.isp}}</div>
            <div class='show'>经度: {{longitude}}</div>
            <div class='show'>纬度: {{latitude}}</div>
        </div>

        <map id='map' :longitude="longitude" :latitude="latitude" scale="14" show-location style="width: 90%; height: 300px;"></map>

    </div>
</template>

<script>
import card from '@/components/card'
import config from '../../config'

export default {

    data () {
        return {
            show: 1,
            sip: '',
            userInfo: {},
            msg: '',
            ipinfo: {},
            ipshow: '当前所在ip',
            longitude: '',
            latitude: '',
        }
    },

    components: {
        card,
    },

    created () {
        this.getUserInfo();
        this.positionSearch();
        this.getLoc();
    },

    methods: {
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
                fail: function(res) {
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
                url: config.service_getipinfo_Host,
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
        },
        setMap(lat, long) {
            this.longitude = long;
            this.latitude = lat;
        },
        isValidIP(ip) {
            var reg = /^(\d{1,2}|1\d\d|2[0-4]\d|25[0-5])\.(\d{1,2}|1\d\d|2[0-4]\d|25[0-5])\.(\d{1,2}|1\d\d|2[0-4]\d|25[0-5])\.(\d{1,2}|1\d\d|2[0-4]\d|25[0-5])$/
            return reg.test(ip);
        },
        getUserInfo () {
            // 调用登录接口
            wx.login({
                success: () => {
                    wx.getUserInfo({
                        success: (res) => { this.userInfo = res.userInfo
                            console.log(res.userInfo);
                        }
                    })
                }
            })
        },
        clickHandle (msg, ev) {
            console.log('clickHandle:', msg, ev)
        }
    }
}
</script>

<style scoped>
.userinfo {
    display: flex;
    flex-direction: column;
    align-items: center;
}

.userinfo-avatar {
    width: 128rpx;
    height: 128rpx;
    margin: 20rpx;
    border-radius: 50%;
}

.userinfo-nickname {
    color: #aaa;
}

.form-search {
    margin-top: 10px;
}

.form-control {
    display: block;
    padding: 0 12px;
    margin-bottom: 5px;
    border: 1px solid #ccc;
    float: left;
}

.counter {
    display: inline-block;
    margin: 10px auto;
    padding: 5px 10px;
    color: blue;
    border: 1px solid blue;
}

.show {
    align: left;
}
</style>
