<template>
    <div class="page">
        <div class="page__hd">
            <text class="page__title">{{title}}</text>
            <text class="page__desc"></text>
        </div>
        <div class="page__bd">
            <div class="section tc">
                <video class="video" id="myVideo" :src="src" :danmu-list="danmuList" enable-danmu danmu-btn controls></video>
                <div class="btn-area">
                    <input class="video-input" @blur="bindInputBlur"/>
                    <button @click="bindSendDanmu">发送弹幕</button>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
import config from '../../config'

export default {

    data () {
        return {
            inputValue: '',
            videoContext: {},
            title: '宣传片',
            src: 'http://www.jsssha.com/wp-content/uploads/2016/11/%E6%8C%82%E7%89%8C.mp4',
            danmuList: [
                {
                    text: '盛世华安好棒',
                    color: '#ff0000',
                    time: 1
                },
                {
                    text: '可以发弹幕居然',
                    color: '#ff00ff',
                    time: 2
                },
                {
                    text: '精彩的在后面',
                    color: '#ff0000',
                    time: 3
                }
            ]
        }
    },

    onReady: function (res) {
        this.videoContext = wx.createVideoContext('myVideo')
    },

    components: {
    },

    onLoad (options) {
    },

    created () {
    },

    methods: {
        bindInputBlur (e) {
            this.inputValue = e.mp.detail.value;
        },
        bindSendDanmu () {
            this.videoContext.sendDanmu({
                text: this.inputValue,
                color: this.getRandomColor()
            })
        },
        getRandomColor () {
            let rgb = []
            for (let i = 0 ; i < 3; ++i){
                let color = Math.floor(Math.random() * 256).toString(16)
                color = color.length == 1 ? '0' + color : color
                rgb.push(color)
            }
            return '#' + rgb.join('')
        }
    },

    watch: {
    }
}
</script>

<style scoped>
@import "../../../static/css/app.css";

video {
    width: 600rpx;
}
</style>
